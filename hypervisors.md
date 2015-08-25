# An Overview of Hypervisors

## What is Xen?

Xen is a type-1 hypervisor like VMware ESXi and Microsoft Hyper-V. Xen
has the unique ability to provide a greater level of separation between
various components than other hypervisors available. In small deployments,
Linux users will most often compare Xen to KVM while larger scale deployments,
will most often be compared to VMware ESXi and OpenStack.

## What is the difference between the types of hypervisors?

Hypervisors were classified by Gerald J. Popek and Robert P. Goldberg in
their 1974 article "Formal Requirements for Virtualizable Third Generation
Architectures". The [two types the hypervisors][1] they identified are:

### Native or Bare Metal or Type-1

A native or bare metal hypervisor is commonly referred to as a type-1
hypervisor. This kind of hypervisor is directly in control of the hardware
and has all the resources of the system available to it and as a result it
should provide the best possible performance for the different virtual
machines running on the system. While there are clearly upsides to this
approach, the downside is that the hypervisor must implement drivers for every
piece of hardware that your system needs to support or use. Examples of
type-1 hypervisors are Xen, VMware ESXi, Microsoft Hyper-V, Linux KVM,
FreeBSD bhyve, and Mac OS X Hypervisor.framework.

### Hosted or Type-2

A hosted hypervisor is one that runs on top of the existing operating system
which allows the hypervisor to leverage the existing hardware support from
the operating system. This leads to a smaller code base that hopefully easier
to maintain and more portable. Examples of type-2 hypervisors are VMware
Workstation, Oracle Virtualbox, and QEMU. The downside these types have is
since they are not in full control of the hardware they are dependent on the
operating system for resources which can lead to a loss in performance.

## A blended approach

Those familiar with KVM and QEMU will note that they are often used together
yet are mentioned above as different types. As many hypervisors have evolved
they have blended their approach to leverage advantages from the two different
types. In the case of KVM, it was added to the Linux kernel where it is able
to use all the existing drivers and hardware support available in the Linux
kernel. KVM further leverages the existing operating system by running each
virtual CPU and emulated devices as an user-space process. The advantage
is a blended environment that allows KVM to approach near native system
performance for many workloads with few compromises.

## Improving performance

Xen's roots go back to the time before the x86 instruction set contained
special instructions to make it possible to virtualize privileged
instructions. The Xen developers looked at the parts of
the system that were hard to virtualize given that environment.
Primarily these difficult to virtualize components came in the form of
hardware behaviors and privileged instructions. Instead of looking to
emulate hardware interfaces within the hypervisor, they instead provided
a software interface in an operation called paravirtualization [2].
The result was that difficult to emulate hardware devices were unnecessary
and the state of the virtualized machine would be updated as if the original
hardware manipulations had happened faster and without privileged instructions.

[1]: https://en.wikipedia.org/wiki/Hypervisor#Classification
[2]: https://en.wikipedia.org/wiki/Paravirtualization
