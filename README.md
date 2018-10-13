This is a repo for the course: https://www.ipspace.net/Building_Network_Automation_Solutions

The vagrant lab is based on: https://github.com/Juniper/vqfx10k-vagrant/tree/master/light-2qfx-2srv with some light modifications.

This vagrantfile will spawn 2 instances of VQFX (light) and 2 Ubuntu servers
Both VQFX will be connected back to back with IP address  and eBGP pre-configured on their interfaces

### Tools
 - Ansible for provisioning
 - Junos module for Ansible

# Topology

        em0|                        em0|
    =============               =============
    |           |  em4 em5      |           |
    |   vqfx1   | ------------- |   vqfx2   |
    |           | ------------- |           |
    =============               =============
        em3|                        em3|
    =============               =============
    |    srv1   |               |    srv2   |
    =============               =============

# Provisioning / Configuration

Ansible is used to preconfigured both VQFX with an IP address on their interfaces
Both servers are preconfigured with an IP address and a route to their respective vQFX

# TODO

Remove default route for the port forwarded nics added by vagrant.