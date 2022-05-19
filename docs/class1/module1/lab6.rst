Deploy BIG-IP VM
================

Now that the KVM hypervisor networking is properly configured, download
the latest **qcow2 BIG-IP** image from downloads.f5.com and perform the
following steps on the Ubuntu instance:

Unzip and copy the downloaded image file to the /var/lib/libvirt/images
directory

Next, create BIG-IP virtual machine using virt-install utility,
adjusting the image name (highlighted in red) as appropriate.

.. code:: console

    virt-install --name big-ip --ram 16384 --vcpus=8 --os-variant=centos7.0 \
    --network bridge=br0,model=virtio \
    --network bridge=br1,model=virtio \
    --network bridge=br2,model=virtio \
    --network bridge=br3,model=virtio \
    --accelerate \
    --disk path=/var/lib/libvirt/images/BIGIP-17.0.0-0.0.22.qcow2,bus=virtio,cache=none,size=96 \
    --noautoconsole --noreboot --import

Start the virtual machine and also set to autostart when Ubuntu is
rebooted:

.. code:: console

    virsh start big-ip
    virsh autostart big-ip

Get the console number of the BIG-IP virtual machine:

.. code:: console

    virsh list

After waiting a few minutes, connect to BIG-IP console using console ID
number. For example, if the number 1 was returned from the **virsh
list** command:

.. code:: console
    
    virsh console 1

Login to BIG-IP and change password for root from the default.
Additionally, while the admin password is also changed at the same time
as the root password, it’s marked as expired and must be changed the
next time the admin user logs in. To avoid having the change the admin
password later, use the following TMSH commands to change it now:

.. code:: console

    tmsh modify auth password admin
    tmsh save /sys config

Configure BIG-IP management interface and set IP address to second
elastic IP address of the /31 used for management and set management
route to the first elastic IP address of the /31 used for BIG-IP
management.

For example, if the Metal elastic IP address block is
**147.28.141.130/31**, configure the management IP address to be
**147.28.141.131** and the management route to be **147.28.141.130**.