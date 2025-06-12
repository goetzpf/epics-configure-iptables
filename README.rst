========================
epics-configure-iptables
========================

Overview
--------

This script solves a problem with UDP unicasts that are sent to machines that
have several servers listening on the same UDP port.

The default in Linux is that only one of the processes will receive the UDP
unicast. 

This script sets iptables rules that convert a UDP unicast on a given port
to a UDP broadcast on the same port. This ensures that *all* processes
listening on that port receive the UDP package.

If no interfaces are specified on the command line, the script enables iptables
rules for all interfaces.

Usage with EPICS
----------------

In `EPICS <https://epics-controls.org/>`_ servers use port 5064 for Channel
Access and UDP port 5075 for PV Access. If several servers run on the same
host, they cannot all be reached by UDP unicasts.

Particularly for EPICS two systemd configuration files are provided. These
start the script for port 5064 (channel access) an port 5075 (pv access) once
when the host is started and the network interfaces are up.

Installation
------------

Simply copy epics-configure-iptables to directory /usr/local/bin::

  sudo cp -a epics-configure-iptables /usr/local/bin

Installation of systemd services
--------------------------------

Two service files, caserver-configure-iptables.service and
pvaserver-configure-iptables.service allow to run epics-configure-iptables once
after the network has been brought up.

Install with these commands::

  sudo cp -a caserver-configure-iptables.service /etc/systemd/system
  sudo cp -a pvaserver-configure-iptables.service /etc/systemd/system

Enable with::

  sudo systemctl enable caserver-configure-iptables.service"
  sudo systemctl enable pvaserver-configure-iptables.service"

And start with::

  sudo systemctl start caserver-configure-iptables.service"
  sudo systemctl start pvaserver-configure-iptables.service"

