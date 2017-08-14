# Connect a Debian KVM guest to the Internet

We use KVM to provision hardware between multiple services.  Once those services are installed we need to provide a public IP to them.
We usually only have a /24 network assigned from a the hosting provider, this only gives us 5 public IP address.
That's not a problem as only the Proxy VM needs one. 

* You need to bind the additional addresses to the host nic.
* Add a macvtap network connection to the guest VM, in addition to the bridged network.
* Add a static network config using the IP on the guest VM s