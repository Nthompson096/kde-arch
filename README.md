# kde-arch
My own arch linux ISO with KDE built inside it.

# How to install

simply install archiso and use this script inisde a copied /usr/share/archiso/configs/

can either be releng/ or baseline/
pick your poison essentially.
also recommend [reading the wiki](https://wiki.archlinux.org/title/archiso).

place this inside to where you copied either baseline/ or releng/

```
#!/bin/bash

if [ "$(id -u)" -eq 0 ]; then
  echo "Creating the ArchISO."
  # Commands to execute if the user is root
else
  echo "Run as sudo."
  exit 1
fi

if [[ -d work/ ]]; then
	rm -r work/ && mkarchiso -v -w work -o /path/to/your/iso/dest/ .
elif [[ ! -d work/ ]]; then
	mkarchiso -v -w work -o /path/to/your/iso/dest/ .
fi
```

Edit packages.x86_64 and include:
```
plasma-meta
sddm
konsole
dolphin
firefox
spice-vdagent
# A web browser is optional; but it can help.
# the spice deamon is also optional but it's good to have for VM's mainly for copy-paste under KVM.
```
Next edit in airootfs/root.zlogin
```
systemctl enable sddm.service --now
spice-vdagent
```
add in airootfs/etc/sddm.conf:
```
[Autologin]
User=root
Session=plasma.desktop

[Users]
MinimumUid=0
```
edit in airootfs/root/.config/powermanagementprofilesrc:
```
[Screen]
...
BlankTime=0
StandbyTime=0
SuspendTime=0
OffTime=0
...
```
Edit your motd in, you've guessed it airootfs/etc/motd and make it blank.

to edit the ISO name after a successful build you would need to do the following and edit these lines:
```
iso_name="kde-archlinux"
iso_label="ARCH_$(date --date="@${SOURCE_DATE_EPOCH:-$(date +%s)}" +%Y%m)"
iso_publisher="KDE Arch Linux <https://archlinux.org>"
iso_application="Arch Linux Live with KDE"
```
That's it, go build! you won't be able to install this inside a actual OS unless if you use calamares; which I'm having a hard time with because it's an linux distro-agnostic independent installer.
