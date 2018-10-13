This is a repo for the course: https://www.ipspace.net/Building_Network_Automation_Solutions

The vagrant lab is based on: https://github.com/Juniper/vqfx10k-vagrant/tree/master/light-2qfx-2srv with some modifications.

This vagrantfile will spawn 2 instances of VQFX (light - only RE and no PFE) and 2 Ubuntu servers (for testing)
Both VQFX will be connected back to back with IP address  and eBGP pre-configured on their interfaces.
Servers have static route for 10.10/16 pointing to respective vqfx.

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

Ansible is run from WSL on the same workstation as vagrant.

# TODO

Remove default route for the port forwarded nics added by vagrant, so i block unintended network connectivity for the srv on the vagrant port forwarded nics.