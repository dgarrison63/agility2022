.. role:: red
.. role:: bred

BIG-IP 261 Deploying F5 on Equinix Metal and Equinix Fabric
===========================================================

With the release of Equinix Metal, enterprises now have a great option
for deploying resources on bare metal at the network edge. Additionally,
since Equinix Metal is integrated into the Equinix Fabric, it provides
an excellent location for a centralized access endpoint into an
organization’s multi-cloud environment.

This lab provides will step you through the process of deploying an
scalable application delivery infrastructure on top of Equinix Metal
using the F5 BIG-IP. The BIG-IP tier providesadvanced layer 4/7 traffic management and security services.
In this configuration, the BIG-IP instances can operate in either an “active/
active” or “active/standby” mode depending upon application requirements
and services utilized. However, due to lab resource contstraints, during the lab, you will deploy a 
single insstance the BIG-IP.

Additionally, you will be able to choose whether you want to deploy BIG-IP on either the VMware or KVM hypervisors - your choice.


Accessing the Virtual Lab
-------------------------

If you are not familiar with the process for joining an F5 training course, refer to:

- `How to join a training course <https://help.udf.f5.com/en/articles/3832165-how-to-join-a-training-course>`_
- `How to use the training course interface <https://help.udf.f5.com/en/articles/3832340-training-course-interface>`_

To access your lab and lookup the necessary IP addresses, you should have
received an email with your personal "Lab Portal Link". You will see the Lab Guide link and the VM's you'll spend all of your time attached to.