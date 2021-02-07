# ghettoVCB

## Description

The ghettoVCB script performs backups of virtual machines residing on ESX(i) 3.x, 4.x, 5.x, 6.x & 7.x servers using methodology similar to VMware's VCB tool. The script takes snapshots of live running virtual machines, backs up the  master VMDK(s) and then upon completion, deletes the snapshot until the next backup. The only caveat is that it utilizes resources available to the ESXi Shell running the backups as opposed to following the traditional method of offloading virtual machine backups through a VCB proxy.

## Download

Latest ghettoVCB VIB and Offline Bundle can be downloaded from [here](https://github.com/lamw/ghettoVCB/releases)

## Install

You can quickly install/update ghettoVCB by downloading and installing either the [VIB or offline bundle](https://github.com/lamw/ghettoVCB/releases) using the following commands. If you wish to update to latest ghettoVCB release and are using the ghettovcb.conf file and wish to have the setting persist, make sure to use the *update* command instead of *install*

More details on using the ghettoVCB VIB/Offline Bundle can be found in this blog post [here](https://www.virtuallyghetto.com/2015/05/ghettovcb-vib-offline-bundle-for-esxi.html)

Install VIB
```
esxcli software vib install -v /vghetto-ghettoVCB.vib -f
```

Install offline bundle
```
esxcli software vib install -d /vghetto-ghettoVCB-offline-bundle.zip -f
```


### cron support
Add the following lines to /etc/rc.local.d/local.sh on your ESXi servers, and reboot
```
#
# setup ghettoVCB
#
GHETTODIR=/vmfs/volumes/vm/git/ghettoVCB
# kill cron
/bin/kill $(cat /var/run/crond.pid) # Gets the cron service pid and simply kills it.
# The next line writes a typical cron line to the crontab
/bin/echo "00 07 * * * ${GHETTODIR}/ghettoVCB.sh -g ${GHETTODIR}/ghettoVCB.conf -a -e ${GHETTODIR}/ghettoVCB.exclude > /vmfs/volumes/vmbackup/ghetto/ghettoVCB-backup-\$(hostname -s)-\$(date +%Y-%m-%d-%H%M%S).log" >> /var/spool/cron/crontabs/root
# Finally we start the cron service again
/usr/lib/vmware/busybox/bin/busybox crond   
```


### Additional Documentation & Resources
>>>>>>> e1e53ff... adding info on how to add to crontab
- [ghettoVCB Documentation](http://communities.vmware.com/docs/DOC-8760)
- [ghettoVCB VMTN Group](http://communities.vmware.com/groups/ghettovcb)
- [ghettoVCB Restore Documentation](http://communities.vmware.com/docs/DOC-10595)

## Licensing

The MIT License (MIT)

Copyright (c) 2021 www.virtuallyghetto.com!

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
