---
title:  "Wave-Particle Duality"
date:   2014-02-17 22:37:00
categories: ['physics']
primary: Physics
---
_I'd modified this for my university's [Physics blog](https://physociety.wordpress.com/2014/02/17/its-a-wave-its-a-particle-its-a-wave-particle/)_.

Amateur physics enthusiasts like to question everything. One popular question is "What is light?"[^1]. Especially if they're in school and have just learnt that light is a "wave-particle". Even the mildly disinterested chap is piqued by this. How can something be a wave and particle?

For many, this is the stepping stone to the vastly unintuitive Quantum Mechanics. By consequence, people try to convince themselves (in vain) that light if both a wave and a particle - quite literally. There are some beautiful stories spun around this concept. 

> In experiment X, light is a particle whenever you peek[^2] at it and becomes a wave when you don't!
>
>    -- tragically named Pop-Sci fact

The trouble with such facts is that they're misleading if you don't know what's really going on. This encourages people to ask how light knows to "change" from particle to wave and vice-versa. A more common question is, "In that case, is light **really** a wave or a particle?".

While it achieves its purpose of getting people to think about science, it fails miserably in satisfactorily answering questions. The most common "Because [fancy science term] says so" is escapism and partly incorrect in most cases. So people end up thinking that it's beyond their grasp and forget it. Or worse, brag to friends and write blog posts about their new-found knowledge :P

Before we address this question, let's quickly summarize the Indian high-school way of getting axiomatically used to the idea of wave-particles. 

deBroglie's equation [^3] made it possible to assign wavelengths to particles and momenta to waves. Why did it make sense? Because it was related to the well revered Quantum Mechanics [^4]. Then, there's the photoelectric effect [^5] vs. interference patterns from Young's double slit experiment. The former is explained by light being a particle (called photon), the latter is explained by conventional wave-mechanics of light. TL;DR, _It's a wave... It's a particle... It's a wave-particle!_

Now we have context to address the wave-particle duality. Let's break it down - particles have equations of motions and waves, equations of propagation. In that case, does a wave-particle have both or neither? Upon short-lived rumination, you might arrive at a more fundamental question: do the words motion or propagation even make sense with respect to these wave-particles?

Let's go down another rung of the ladder that is abstraction. We are dangerously close to the realm of non-pragmatic philosophy here. With that in mind, 

- **How do we identify something?** By its _characteristics_. As the cliched example goes, the presence of leaves can be identified from the rustling sound it makes when the wind blows past it. 
- **What are we doing when we identify something?** __Classifying__ it with a name - leaf, bazooka, a royale with cheese. 

The same applies for waves and particles. Conventionally, anything with a frequency, wavelength, etc. is called a wave and anything with mass, momentum and so on is called a particle. To humor me, let's invent an entity __thaum__. A __thaum__ can have the following properties:

1. momentum
2. wavelength
3. frequency
4. energy

Let's forget about how we can measure them and what they mean for now and assume they have meaning and can be measured[^6]. So, anything with these 4 properties is a __thaum__. An electron, for instance, has all 4 of these (courtesy of QM) - however, with different meanings for the four properties - and hence, can be classified as a __thaum__. You can probably see where this is going.

So does light, and replace __thaum__ with __wave-particle__ and voila. It's a trivial matter of semantic distinction. Physicists could've called it a million other things, but they chose this. So, we've to live with it. A wave-particle is not part-wave, part-particle - it's a completely different entity. And that, marks the end of the main aim of this article - clearing the wave-particle misnomer. 

### Read on at the risk of being more confused...
__Disclaimer: physics and math present__

QM says that everything is a wave-particle.[^7] Any entity in a pure particle state would have to be infinitely concentrated at a point and anything with a pure wave state will have infinitely spread out momentum states. The uncertainty principle [^4] helps in understanding this conflict. Moreover, a particle can only be represented as a Kronecker delta function which cannot be normalized. So, it creates a whole host of new problems - the wave-particle picture plays well with the framework of Quantum Mechanics. For more confusion, look up __principle of complementarity__.

**What does the frequency of an electron mean?** In the context of a wave-particle, we have Planck's relation $$ E = hf $$ where $$ E $$ is the energy of the electron, $$ h $$ is the Planck constant and $$ f $$ is the frequency of the electron. So, what is this $$ f $$? It's definitely not easy to measure, because this "electron wave" is in the realm of complex numbers - the thing that oscillates like a wave is a complex number and is called the wavefunction of the electron. The same wavefunction from Schrodinger's equation.

**What does the frequency of a photon mean?** With QED in mind, it's slightly easier to explain this. Some context first. The curl of the Maxwell-Faraday equation and time derivative of Ampere's law combined with clever vector identities give us separate wave equations for the electric and magnetic field. The solution to these wave equations can be represented as the sum of normal mode functions by fixing boundary conditions and separation of variables (spatial and time). With some more work, it can be shown that the normal modes are tthe same as that of an equivalent harmonic oscillator. So, an electric field is equivalent to an infinite number of independent harmonic oscillators. When the electromagnetic field is quantized, we replace these harmonic oscillators with their quantum counterparts.

Each quantum harmonic oscillator has a frequency. This can be measured classically when the state is coherent and distinguishable from the ground state. 

TL;DR the quantized electromagnetic field is made of quantum harmonic oscillators with their own frequencies. 

A photon is simply energy added to these oscillators and the classical measurements we get out of the oscillators tip us off about the frequency - the frequency of a photon can be thought of as the frequency of the quantum harmonic oscillator it is added to or taken away from.

That's it for now.

[^1]: If you're ever caught by one, a good way to annoy them is to say "the thing that comes out of a flashlight!" and smirk.

[^2]: In QM parlance, whenever you "observe" it

[^3]: $$ \lambda = \frac{h}{p} $$ where $$ \lambda $$ is the deBroglie wavelength, $$ h $$ is the Planck constant and $$ p $$ is the momentum. 

[^4]: In QM, momentum and position are observables and conjugate variables. A well-known result of Fourier analysis is the uncertainty relation $$ \Delta x\Delta y\leq\frac{1}{2} $$ for two conjugate variables $$ x $$ and $$ y $$. This tells us that our measurements of position and momentum must not commute and is an intuitive basis for the commutation relation $$ [x,p]=\iota\hbar $$.

[^5]: Shine light on a metal and energy comes out.

[^6]: It would be very remiss of me if I didn't mention that the phrases "have meaning" and "can be measured" are quite misleading when ascribed to the 4 properties mentioned.

[^7]: Quantum Field Theory says that everything is a field - particles are special excited states of the quantum field which is a localized wave. 

*[QM]: Quantum Mechanics

*[QED]: Quantum Electrodynamics
