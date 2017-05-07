training.suse.com/training/

sonder von vuut opensuse book

4 tracks: enterprise linux, openstack cloud, enterprise storage, systems management
administrator (SCA)
engineer    (SCE)
architecht 3 admins, 2 engineers, and 1 of anything

yast, syscon "The old Novell stuff", ... zypper?

"Blooms taxonomy"

flavors of suse
sle maintenance model
install
gnome
yast
o
SusEfirewall2 configures iptables
/etc/sysconfig/SuSEfirewall2 and .d/
almost any machine needs dhcp 57/58, maybe dns 53

configure raid
raid 0 pooled "nothing much"
raid 1 mirroring "you find out the first diskk died when the second dies."
raid 5 distributed - data lost when 2 disks fail
raid 6 distributed - data lost when 3 disks fail
raid 10 mirrored stope sets - 0 performance, 1 tolerance. data lost when a disk
  fails in each stripe set.

signals
15 SIGTERM the default
9 KILL
1 HUP (generally stop, reread, and start again)

killall a bit like pkill
nice -20 - 19 (higher = nicer)

hwclock

7 files:
normal file
directories
links (soft & hard)
pockets
pipes
block devices
char devices

ls -F shows filetype

long-running
Control-Z
bg
jobs
disown %1
exit


software-version-release.architecture.rpm
header
{gpg signature}
scripts
CPIO archive

ls | args rm

rpm -F package.rpm (only update if installed)
rpm -U package.rpm (update or install)

modules
/lib/modules/<kv>/
hardware driver kernel modules
/lib/modules/<kv>/kernel/drivers

lsmod
insmod
rmmod
modprobe figures out deps for you, rather than manually doing insmod etc
