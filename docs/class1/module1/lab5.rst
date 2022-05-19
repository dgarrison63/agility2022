Lab #5: Modify KVM Host Network Configuration
=============================================

Modify instance network configuration 
--------------------------------------

| **NOTE:** You will need to log in to the Ubuntu server via SSH
  instance using the auto-assigned Metal instance management IP address
  to complete this section. This management IP address does not need to
  be changed and is NOT the same as the BIG-IP management IP address.

| This lab uses KVM network bridging and the network
  configuration of the Ubuntu instance needs to be modified to support
  this mode. The layer 2 VLANs are tagged and defined as a subinterface
  of the **bond0** interface and the naming convention is **bond0.<VLAN
  number>.** 
  
  In this deployment example, the tagged interfaces are
  **bond0.1000, bond0.1001, bond0.1002 and bond0.1003**. Depending on
  the VLAN numbers that were auto assigned, your interface names may be
  different, and you will need to modify as needed.

Only the BIG-IP management interface - bond0.1000 in this case — uses a
public IP address while the rest of the new interfaces use RFC1918
private IP addresses.

Edit the interfaces configuration file and append the interface
configuration commands to the bottom of the file, adjusting the new
interface names to match your assigned VLAN numbers. 

| Additionally, the BIG-IP management IP address—highlighted below in
  red—need to be changed to match the **first** address of the elastic
  IP address blocks that you requested in a previous step.
| As an example, if the /31 elastic IP address block you requested was
  147.28.141.130/31, the IP address you would define on the Ubuntu
  network configuration would be 147.28.141.130. In a later step, you
  will assign the second IP address of the block—147.28.141.131—as the
  BIG-IP management IP address.

**Ubuntu #1**

vi /etc/network/interfaces

.. code:: console

    auto bond0.1000
    iface bond0.1000 inet manual
    pre-up sleep 5
    vlan-raw-device bond0

    auto br0
    iface br0 inet static
    address <first IP of BIG-IP mgmt address block>
    netmask 255.255.255.254
    bridge_ports bond0.1000
    bridge_stp off
    bridge-fd 0
    bridge_maxwait 0

    auto bond0.1001
    iface bond0.1001 inet manual
    pre-up sleep 5
    vlan-raw-device bond0

    auto br1
    iface br1 inet static
    address 192.168.10.10
    netmask 255.255.255.0
    bridge_ports bond0.1001
    bridge_stp off
    bridge-fd 0
    bridge_maxwait 0

    auto bond0.1002
    iface bond0.1002 inet manual
    pre-up sleep 5
    vlan-raw-device bond0

    auto br2
    iface br2 inet static
    address 192.168.20.10
    netmask 255.255.255.0
    bridge_ports bond0.1002
    bridge_stp off
    bridge-fd 0
    bridge_maxwait 0

    auto bond0.3999
    iface bond0.3999 inet manual
    pre-up sleep 5
    vlan-raw-device bond0

    auto br3
    iface br3 inet static
    address 169.254.22.42
    netmask 255.255.255.248
    bridge_ports bond0.3999
    bridge_stp off
    bridge-fd 0
    bridge_maxwait 0

Restart networking services to enable the new configuration.

.. code:: console

  systemctl restart networking
