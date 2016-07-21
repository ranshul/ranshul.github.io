---
title:  "Large Data - I"
date:   2015-07-21 22:37:00
categories: ['Data', 'Pre-MR']
primary: Data
---

In the introductory Data Structures and Algorithms course I took, there was a passing mention of a scenario where the data wouldn't fit into primary memory. At that point of time, implementing (read reinventing the wheel) a Red Black Tree and B-Tree from scratch seemed more important. Along with that, I'd taken a course on Database Systems where a million different ideas were thrown at me in detail. For inherently stupid reasons, I never realized how some of those ideas were useful in dealing with large data. This post is to explore those options and hopefully, temper the magnitude of embarrassment that is bound to occur along the way.

A good place to start is the maximum size of a `vector` of integers in STL. The documentation warns that it is the maximum *potential* size. It's a succinct way of saying that `vector` can theoretically handle that capacity but may fail well before that.

{% highlight c++ %}
#include <iostream>
#include <vector>
int main(){
  std::vector<int> v;
  std::cout << v.max_size() << std::endl;
  return 0;
}
{% endhighlight %}

The maximum size depends on library, platform and system. For a static array declaration, the theoretical upper limit is the process' data segment(DS) limit set by the OS. In a POSIX system, the `sbrk` function is used to change the limit of a process' DS.

When that limit is reached, we have different options depending on the size of the data and the scalability desired.

<h2> Memory Mapped I/O </h2>
If the data exceeds the memory limit, but is small enough to fit into a hard disk, memory mapped IO establishes a mapping between the file and the process' address space. It ***does not*** transfer the entire to the RAM immediately[^1]; the operating system's `mmap` allows pointer access to the data like memory. The actual disk reads are performed after a specific location is accessed. These locations are in the virtual address space of the operating system. The difference between plain I/O and memory-mapped I/O is the relatively greater overhead in plain file I/O. With the `MAP_SHARED` flag in UNIX, it is possible to let one process communicate with other processes that have the same area mapped.

The Boost library provides memory-mapped file support for C++. Windows provides a different API for this and UNIX systems have [mmap](https://en.wikipedia.org/wiki/Mmap) in the `sys/mman.h` header. We'll use Python's `mmap` [module](https://docs.python.org/2/library/mmap.html).

{% highlight python %}
import os
import time

f = open('mat.txt','r')
sta_time = time.time()
for i in range(0,1024*1024): #assume file is big enough
  f.seek(14,os.SEEK_CUR)
  c = f.read(1)
  #do something to c
  f.seek(-10,os.SEEK_CUR)
  c = f.read(1)
  #do more things to/with c

fin_time = time.time()
f.close()
print fin_time - sta_time
{% endhighlight %}

This is the plain file I/O version. The same thing is done with `mmap`. The two are benchmarked using `time.time()`. In Windows, `time.clock()` must be used. For a more confident benchmark, use the `timeit` module. However, some caching optimizations may interfere with the best-of-3 runs benchmarking in `timeit`.

{% highlight python %}
import os
import time
import mmap

f = open('mat.txt','r')
m = mmap.mmap(f.fileno(),0,access=mmap.ACCESS_READ)
sta_time = time.time()
tmp = 0
for i in range(0,1024*1024): #assume file is big enough
  tmp += 14
  c = m[tmp] # or use seek and read with m
  #do something to c
  tmp -= 10
  c = m[tmp]
  #do more things to/with c

fin_time = time.time()
f.close()
print fin_time - sta_time
{% endhighlight %}

Most string operations like slice (in the array notation) can be done on an `mmap` object. If changes are to be made, the access flags are modified accordingly. To record the changes made by an `mmap` object, the `flush()` method must be called. NumPy offers [memmap](http://docs.scipy.org/doc/numpy/reference/generated/numpy.memmap.html) as well. This can be used in conjuction with the `pickle` module if needed. However, there will be an overhead from processing the stored segment to convert it back into a Python object.

One of the drawbacks of memory mapped IO is page faults. As the size approaches maximum capacity, the misses increase. If this repetitively occurs, memory mapped I/O can be [slower](http://lists.freebsd.org/pipermail/freebsd-questions/2004-June/050371.html) than normal file I/O. If the file size exceeds the addressable space, then only a portion of the file can be mapped at a time. It also tries to obtain a contiguous block of addresses. The size of the address space is related to the underlying architecture. An IA-32 architecture can directly address 2<sup>32</sup> bytes (around 4 GB) while an Itanium architecture (64-bit x86 or IA-64) can address 2<sup>64</sup> bytes. But in an AMD64 architecture, only 48 bits are used. This needs careful handling to ensure portability.

The next alternative is to use HDF5.

<h2> Hierarchical Data Format </h2>
HDF5 specifically targets numerical data.  It is very useful if the data can be organized hierarchically. Its Python interface is `h5py`. It doesn't take long to [install](www.h5py.org) it. Naturally, it works well with NumPy. Once the file has been created, it can be used like a key-value structure. Internally, HDF5 splits the data, flattens it before storing and indexes the locations by a B-Tree.
{% highlight python %}
f = h5py.File("test.hdf5")
test = np.ones((100,53))
f["test"] = test
print f["test"]
print f["test"][3:7]
f["test"][9] = 2
print f["test"]

smartds = f.create_dataset("opt",(500,1008),dtype=np.float32)
{% endhighlight %}

`smartds` takes up space in the hard disk only after some values are written into it. `f` has to be flushed to clear the buffers and write to the disk. Slicing operations for both read and write have overheads which visibly slow down things when the number of slices increase. Familiarity with NumPy array syntax makes it very easy to use HDF5. However, some care must be taken when using NumPy operations. For example, if block-wise matrix multiplication were to be implemented, the `dot` function needs to be used with care. Its implementation makes it memory intensive when used with HDF5.

As far as multithreading and the `multiprocessing` module in Python is concerned, there are quite a number of things to be taken care of thanks to the Global Interpreter Lock[^2].

HDF5 has more flexibilities to offer - attributes (metadata) and groups. The organization is a lot like a filesystem. It's internals bear some similarity to that of databases. The serialization used for concurrency is similar. Since it's not a distributed database, LSH isn't used. HDF5 automatically breaks down the data into chunks of shapes that are automatically determined or specified manually. The chunks are flattened out while storing in the disk, but their shape is maintained during retrieval. A B-Tree is used to index those coordinates. [PyTables](http://www.pytables.org/) is a good alternative to h5py.

HDF5 seems like a satisfactory solution for a single machine. But that is not the end of the scalability spectrum. The next step is to add more machines to do the processing. This is an absolute requirement when the data won't fit on a single disk. There is a host of problems and complexity faced in making this jump. From what I've heard and read, the realm of distributed computing is unforgiving.

I decided to go down that rabbit hole and that warrants a separate [post]({% post_url 2015-07-22-largedata2 %}). You'll know why.

<h3>Footnotes</h3>

[^1]: stop judging the usage of semicolon.
[^2]: An interesting write-up on the GIL can be found [here](http://www.jeffknupp.com/blog/2012/03/31/pythons-hardest-problem/).
