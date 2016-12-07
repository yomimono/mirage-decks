## whatever happened to unikernels

a crisis of confidence in /n/ slides

### "unikernels"

first let's talk about non-unikernels (or "kernels", as y'might call 'em)

### kernelspace/userspace

* your rad application
* [optionally, inside a runtime]

it wants some stuff that you don't want to program yourself, like network, storage, etc

### kernelspace/userspace

* your rad application
* [optionally, inside a runtime]
* asking the kernel for stuff

### kernelspace/userspace

* your rad application
* [optionally, inside a runtime]
<--- (via syscalls) --->
* asking the kernel for stuff

### the kernel is privileged

the kernel defines the API it'll answer to
your language's bindings or runtime have to talk on its terms

### the kernel's software is different from your application code

different assumptions, APIs, and invariants
probably a different language than you're used to

### why do we need a kernel?

support for hardware (ex: drivers)
support for software (ex: filesystems)
management of resources (ex: backgrounding a process)

### except <!-- time check: 7 min in rehearsal -->

a lot of operating systems are now running in *virtual machines* on top of a *hypervisor*

### virtual machines

something that looks like an entire computer (and might think it is)... but it isn't
run >1 "computer" (operating system + other stuff) on only 1 physical machine

### hypervisor

software that provides the illusion of isolation to virtual machines
generally, you also want it to support hardware and manage resources

### so what we often really have is:

application
(runtime)
kernel
hypervisor

### but

a lot of stuff is duplicated between the kernel and the hypervisor
the operating system bundling the kernel usually assumes it will be multiplexing between many different programs/users & will have to support lots of different hardware

### is that true?

usually we try not to have >1 service per machine anymore
most hypervisors provide virtualized hardware & the set of drivers the OS needs to target is small

### what's the kernel doing for us, then?

software abstractions (filesystems, network)
via its API

### but its API is probably a bad fit for us

see: sockets

### what can we do about it?

our rad application + libraries that know how to talk to hypervisor
(runtime)
hypervisor

### that's what a unikernel is

our rad application + libraries that know how to talk to hypervisor (+ runtime)

### library operating system

"library operating system" is the bunch of libraries that help us write applications that can run without a traditional OS
(sorry that I don't mean library like "little free library" :( if you want to run an application you use in your library with a library operating system, let's talk!))

### why that's rad

we can write, read, and debug those libraries in a language we're used to, with the abstractions & invariants we're used to
systems code for everyone :D

## some personal experiences

the library operating system project I work with is called MirageOS
it's a framework for generating unikernels in OCaml
(OCaml is a programming language I like)

### demo time

what's that URL that's hosting these slides, anyway?

### what we built & ran

* an artifact that runs directly on a hypervisor which we specified entirely in the language we wrote the application in
* if we want to run on a different hypervisor or with different libraries, we need to rebuild it entirely
* (the "with different libraries" includes "with a different patchset")

### why I like this

if I don't like how something works, I can rewrite it
if I don't like an API, I can substitute my own
if something doesn't work, I'm more equipped to find out why and fix it
the set of stuff I have to care about patching is way, way smaller

### what isn't fixed

I still have to care about the security of my application & the libraries it uses
I still have to write applications that do the right thing
I still need something to run my code on
sometimes there are bugs :(
I like a programming language that isn't very popular

## library operating systems

there are a lot of them, and most aren't in OCaml
some are better fits for more traditional ways of thinking

### potentially of interest for people running arbitrary legacy stuff

Rump: NetBSD kernel as a set of userspace libraries
Rumprun: link your application against these libraries automatically
OSv: POSIX interfaces to freshly-written userspace library implementations of kernel functionality

### potentially of interest for people who like other programming languages

HaLVM
IncludeOS
ClickOS
... [unikernel.org]

## "whatever happened to unikernels?"

I can't tell you about unikernels, but here's what happened to MirageOS

### swallowed by a whale

a lot of contributors got super distracted
[screenshot of whale in menu bar]

### but also, saw something real shiny

solo5, bhyve/xhyve/hyperkit, and the lightweight hypervisor (zOMG MirageOS 3)
what's the difference between a unikernel running in a lightweight hypervisor & a process running in an OS?

### boundaries and isolation

we trust* hypervisors to provide stronger isolation than traditional operating systems
a lot of people want massively disaggregated and decentralized infrastructure

* admittedly, for values of "we" and "trust"

## what is a unikernel?

one answer: an attempt to do a better job of aligning systems abstractions to application abstractions
another answer: containers are so last year

## thanks :)
