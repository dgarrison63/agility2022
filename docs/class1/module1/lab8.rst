Lab #8: Configure BIRD Routing Daemon BGP on KVM Host
=====================================================

The BIRD routing daemon provides BGP routing capability and will be used
to establish BGP neighbors with both the Equinix Metal routers as well
as the BIG-IP instance. Equinix Metal provides a convenience script
that performs the initial configuration of the BIRD routing engine. To
use the script, perform the following:

.. code:: console

    git clone https://github.com/packethost/network-helpers.git
    cd network-helpers
    pip3 install jmespath
    pip3 install -e .
    ./configure.py -r bird | tee /etc/bird/bird.conf

The script configures BIRD to establish BGP neighbors with the two
Equinix Metal router instances. However, BIRD needs to be configured to
also establish a BGP neighbor with the BIG-IP as well. The neighbor IP
address for the BIG-IP is the external VLAN self-ip address.

Modify the BIRD configuration file and add a static route to the BIG-IP
external VLAN self-ip address and add the BIG-IP as a BGP neighbor

**Ubuntu #1**

nano vi /etc/bird/bird.conf

Locate the **protocol static** section and add the following between the
curly braces:

route 192.168.10.11/32 via 192.168.10.10;

At the bottom of the file, add the following:

.. code:: console

    protocol bgp neighbor_v4_3 {
    export filter packet_bgp;
    local as 65000;
    neighbor 192.168.10.11 as 65000;
    }


Save that file and restart the BIRD service:

systemctl restart bird

Confirm BIRD BGP neighbors
--------------------------

Using the BIRD utility, confirm that that the two Metal routers and the
BIG-IP are neighbors and that the virtual server IP address block is
being advertised:

birdc show route

The output should look similar to the below (elastic IP address block
highlighted for clarity):

BIRD 1.6.8 ready.

192.168.10.11/32 via 192.168.10.10 on br1 [static1 2022-02-02] ! (200)
39.178.82.246/31 via 192.168.10.10 on br1 [neighbor_v4_3 2022-02-02 from
192.168.10.11] ! (100/?) [i]
169.254.255.2/32 via 139.178.83.46 on bond0 [static1 2022-02-02] \*
(200)
169.254.255.1/32 via 139.178.83.46 on bond0 [static1 2022-02-02] \*
(200)

You may further validate that BGP neighbors have been established:

birdc show protocols

The output should look similar to the below (BIG-IP neighbor highlighted
in red):

BIRD 1.6.8 ready.
name proto table state since info
direct1 Direct master up 22:28:57
kernel1 Kernel master up 22:28:57
static1 Static master up 22:28:57
device1 Device master up 22:28:57
neighbor_v4_1 BGP master up 22:29:58 Established
neighbor_v4_2 BGP master up 22:31:01 Established
neighbor_v4_3 BGP master up 22:29:47 Established