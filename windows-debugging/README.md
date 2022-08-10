# Windows debugging

Use [kdnet](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-up-a-network-debugging-connection-automatically) to setup debugging.

## Tools one debuggee

* [sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite).
* [python](https://www.python.org/downloads/) for PoC.

## Qemu / virsh

To use `KDNET`, edit the features with `virsh edit $DOMAIN`:
```xml
<domain type='kvm'>
  <features>
    <hyperv mode='custom'>
      <vendor_id state='on' value='KVMKVMKVM'/> <!-- line to add -->
    </hyperv>
  </features>
</domain>
```
