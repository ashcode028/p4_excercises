# p4_excercises
This repo helps to gain hands-on experience in writing custom programs for the data
plane switches using the P4 language. You will build a custom virtual network using mininet that
provides a P4-based software switch, bmv2, which is based on the v1model’s
simple_switch_grpc target. The bmv2 target switch can be managed statically using the thrift
API. The bmv2 switch can also be dynamically managed via the control plane using the
P4Runtime API.

In the next few sections, you will set up your machine with the VM image, test few tutorial
examples with template code (you write the remainder code), and develop your custom
application.
## Setting up your machine
1. Install virtual box
Download and install the appropriate executable for Windows / Ubuntu and follow
the instructions

2. Download and setup the VM from the following link. This VM contains mininet, bmv2
switch tools (P4, P4Runtime), and other necessary tools such as wireshark.
