# SDN-CONTROL-PLANE-HA
### High Availability Cluster:

A controller failure can quickly paralyze the entire network, the issue of control plane HA is critical as controllers are responsible for the functions of network switches. In linux the High availability project promotes reliability, availability and serviceability.
The Linux-HA main product is Heartbeat, a GPL-licenced portable cluster management program for high-availability
clustering. Other Famous HA tools are, Corosync and Pacemaker.

#### Heartbeat Features:
1. A heartbeat signal or massage at regular intervals to finds if cluster nodes working correctly.
2. Heartbeat program runs specialized scripts automatically, supporting two or more nodes clusters.



#### HA setup steps:
Pre-configuration Requirements:
1. Set the VM’s network adapter as host only/ Bridged adapter.
2. Assign hostname to primary node with IP address to all the cluster nodes.
3. To confirm the node is dead a script called STONITH( shoot the other node in the head) is used.

Configuring heartbeat, Heartbeat has three main configuration files ( on both nodes exactly same files):
1. /etc/ha.d/authkeys: The authkeys file is owned by root and chmod 600. It has two lines; auth directive with an
associated method ID number and the other line is the authentication method and the key that go with the ID
number of the auth directive. The three supported authentication methods are: crc, mds and sha1.
2. /etc/ha.d/ha.cf: ha.cf is the main configuration file defines the main heartbeat vairables.
  a. Keepalive: the time between heartbeat in seconds.
  b. Deadtime: time to wait without hearing from a cluster member before declaring it dead.
  c. Warntime: time to wait before issuing “late hearbeat” warning.
  d. Auto_failback: can be set to either on or off, if set to on; after failure if primary node comes back than
     secondary node falls back to secondary state. If set to off, after the primary comes back, it will be
     secondary.
  e. /var/log/ha-log: this stores all the heartbeat logs and can be used for troubleshooting when warning
     occurs.
  f. List all the nodes in the cluster ( use uname -n to find the node name)
3. /etc/ha.d/haresources: this files consists of the node name that will act as primary node, VIP, and the
resource script. Resource script is the startup script that run the necessary commands before the heartbeat
starts its operation.

#### Commands in Heartbeat:
1. To start heartbeat: /etc/init.d/heartbeat start
2. To stop heartbeat: /etc/init.d/heartbeat stop
3. To check the logs: tail -f /var/log/messages

#### Testing SDN Floodlight HA cluster:
1. For two member cluster, start both the clusters. Use ifconfig and host to check if proper ip address and hostname are assigned.
2. Now start the heartbeat service on all nodes, you will see an alias IP on one of the nodes. That's the primary node you had set in the haresources file.
3. To test failover, stop the floodlight and hearbeat service on primary node. You will see an alias ip comes up in the secondary node (test with ifconfig command). And the alias IP from primary is removed automatically as it is down.
4. To test failback, start again the primary node floodlight controller and heartbeat service, An alias IP will automatically will come on it, and the floodlight controller will still work with alias IP.
