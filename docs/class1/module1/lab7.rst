Lab #7: Configure BIG-IP
=======================

Instead of using the BIG-IP web UI to configure the BIG-IP, you will use
the BIG-IP CLI and TMSH commands to configure the BIG-IP instances.
You will need to supply a unique license key for the BIG-IP
as well as adjust references to IP addresses to match the IP addresses
you are using.

Once all of the TMSH commands have been entered on the BIG-IP
instance, 

**NOTE:** The KVM console can be a little difficult to work with and you
may want to use SSH to configure the BIG-IP instances instead. Also,
highlighted below in red are entries that you may have to change;
however, if you have used the same RFC1918 IP addresses, then the only
items you will have to change are the license key and the virtual server
elastic IP address block.

**BIG-IP #1**

.. code:: bash

   tmsh modify sys global-settings hostname bigip-1.example.com
   tmsh create net vlan external interfaces add {1.1}
   tmsh create net vlan internal interfaces add {1.2}
   tmsh create net vlan ha interfaces add {1.3}
   tmsh create net self 192.168.20.11/24 vlan internal allow-service default
   tmsh create net self 192.168.10.11/24 vlan external allow-service default
   tmsh create net self 192.168.30.11/24 vlan ha allow-service default
   tmsh modify sys global-settings gui-setup disabled
   tmsh mv cm device bigip1 bigip-1.example.com
   tmsh modify sys db tmrouted.tmos.routing value enable
   tmsh create net routing bgp my_bgp_config local-as 65000 neighbor add {
   192.168.10.10 { remote-as 65000 } } network add { <virtual server
   elastic IP address block/CIDR> } graceful-restart { restart-time 120 }
   tmsh modify /sys dns name-servers add { 8.8.8.8 }
   tmsh modify /sys ntp servers add { pool.ntp.org }
   tmsh install /sys license registration-key <license key>
   tmsh save sys config
