# Git
```
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/Pomog/DevOps01
git push -u origin main
```
# Run Oracle VirtualBox on Windows
```powershall
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor
(Get-CimInstance -ClassName Win32_ComputerSystem).HypervisorPresent
```
```
Win+R OptionalFeatures
```
- Check
    - Virtualization is enabled in BIOS.
    - Disabled: WSL2, Virtual Machine Platform, Windows Hyper-V Platform.
    - Virtualization-Based Security (VBS) services are disabled: Application Guard, Windows Sandbox, Guarded Host.
    - Check that Windows security features do not block the virtual machine.

# Vagrant 01
- using GitBash
```GitBash
pwd
```
## CentOS
- change working dir
```GitBash
mkdir centos
mkdir ubuntu
```
```GitBash
cd centos
```
- to find Vagrant Box (https://portal.cloud.hashicorp.com/vagrant/discover)
```GitBash
vagrant init eurolinux-vagrant/centos-stream-9
```
- result
```
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```
- RUN
```GitBash
vagrant up
```
- to see downloaded boxes, status
```GitBash
vagrant box list
vagrant global-status --prune
vagrant status
vagrant ssh
vagrant halt
```
## Ubuntu
- change working dir
```GitBash
cd ubuntu
```
- to find Vagrant Box (https://portal.cloud.hashicorp.com/vagrant/discover)
```GitBash
vagrant init ubuntu/jammy64
```

## CentOS Stream is a Red Hat–based distribution that uses dnf
- System maintenance on CentOS Stream
```bash
sudo dnf upgrade --refresh -y && sudo dnf autoremove -y && sudo dnf clean all
```

## Journald logs (systemd):
Instead of storing everything in plain-text log files, Ubuntu also uses systemd-journald to collect system logs.
You can see these logs by using the journalctl command. For example:
```bash
sudo journalctl -xe
```
Or filtering by a specific service:
```bash
sudo journalctl -u <service-name>
```