# check_vmware_status
Simple Nagios plugin to query/change the power state of a VMware ESX virtual machine

This code is provided here but released through the Nagios Exchange (https://exchange.nagios.org).

```
Usage:  ./check_vmware_status -H <hostname> -n <VM name> [options]

Required:
        -H hostname     The hostname of the VMware server (no default)
        -n vmname       The name of the VM guest machine (must match and be unique)

Optional:
        -c command      One of: check, poweron, poweroff (default is "check")
        -u user         User to SSH as (default=root)
        -p password     Password to use (only for testing, see below)
        -P port         SSH port to use (default=22)

This command will SSH to the VMware ESX server as USER and either check
the power status of a virtual machine, or set the power state (and confirm
that it ends up in the desired state).  Note that the ESX server will
need its SSH server running and SSH firewall adjusted to allow access
from the Nagios host.  Also, it is strongly recommended that you install
an SSH key with no passphrase to /etc/ssh/key-<user>/authorized_keys
to enable the nagios user on the Nagios host to SSH to the ESX server
without needing a password or passphrase.  Otherwise, this plugin will
fail and Nagios will timeout the service check with unknown results.

Examples:

  Return OK if the host windows.guest is powered on, or CRITICAL if it is powered off:
        ./check_vmware_status -H vmserver.domain.com -n windows.guest

  Return OK if the host linux.spare.guest is powered off, or CRITICAL if it is powered on:
        ./check_vmware_status -H vmserver.domain.com -n linux.spare.guest -r

  Turn off virtual machine broken.server:
        ./check_vmware_status -H vmserver.domain.com -n broken.server -c poweroff

  Turn on virtual machine fixed.server:
        ./check_vmware_status -H vmserver.domain.com -n fixed.server -c poweron

Note that poweroff and poweron will perform a power state check after
changing the power state of the machine to verify that it matches the
desired state.  If it does, the check will return OK.  If it does not,
it will return CRITICAL.
```
