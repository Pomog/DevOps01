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
- System maintenance on Ubuntu
```bash
sudo apt update -y && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y
```
## Install WordPress
the ownership to the user www-data, which is potentially insecure
```bash
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```
- Configure Apache for WordPress
```bash
vim /etc/apache2/sites-available/wordpress.conf
```
```bash
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```
Enable the site with:
```bash
sudo a2ensite wordpress
```
Enable URL rewriting with:
```bash
sudo a2enmod rewrite
```
Disable the default “It Works” site with:
```bash
sudo a2dissite 000-default
```
```bash
sudo service apache2 reload
```
- Configure database
```bash
sudo mysql -u root
CREATE DATABASE wordpress;
CREATE USER wordpress@localhost IDENTIFIED BY '<your-password>';

GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;

FLUSH PRIVILEGES;
quit
```
- Configure WordPress to connect to the database
```bash
sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
```
```bash
sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/password_here/<your-password>/' /srv/www/wordpress/wp-config.php
```
```bash
sudo -u www-data nano /srv/www/wordpress/wp-config.php
```
```
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );
```
Delete those lines. Then replace with the content of https://api.wordpress.org/secret-key/1.1/salt/.

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

## Runlevels of SysV
- Runlevel 0: Shutdown the system.
- Runlevel 1: Single-user mode (often used for system maintenance). Only the root user is allowed, and many services (like networking) are not started.
- Runlevel 2: Multi-user mode without networking.
- Runlevel 3: Multi-user mode with networking.
- Runlevel 4: Reserved for local administration or could be customized for specific needs. In many distributions, runlevel 4 is essentially the same as runlevel 3.
- Runlevel 5: Multi-user mode with networking and with a graphical desktop environment.
- Runlevel 6: Reboot the system.

```bash
/etc/systemd/system/multi-user.target.wants/
```
- This dir is used by systemd to manage which services are started when the system reaches the multi-user target (roughly equivalent to the traditional runlevel 3 in SysV init systems)
- multi-user.target in systemd is roughly equivalent to runlevel 3 in SysV init. It starts all the necessary services for a multi-user, non-graphical environment.

## Provisioning
```
   config.vm.provision "shell", inline: <<-SHELL
     dnf update
     dnf install httpd wget unzip git -y
     mkdir /opt/devopsdir2
     free -m
   SHELL
```

## Install and configure WordPress - Ubuntu Server 20.04 LTS
- 
```bash
sudo apt update
sudo apt install apache2 \
                 ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
```

## Vagrant-hostmanager
The vagrant-hostmanager plugin automates the management of your hosts file, ensuring that your virtual machines’ hostnames and IP addresses are always correctly mapped. This simplifies accessing your Vagrant environments by hostname instead of IP address, which is especially useful in multi-machine setups.

### Key Features
- Automatic Hosts File Updates:
Automatically adds or removes entries in your /etc/hosts file (or its equivalent) as VMs are started, halted, or destroyed.

- Multi-Machine Support:
Works seamlessly with environments that include multiple VMs, ensuring that all hostnames are correctly configured.

- Simplified Network Management:
Eliminates the need for manual editing of the hosts file, reducing configuration errors and saving time.

### Installation
```bash
vagrant plugin install vagrant-hostmanager
```
After installation, enable and configure the plugin within your Vagrantfile to suit your environment's needs. This setup allows for streamlined development and testing, as all your VMs are accessible by easily remembered names.


## READ
```bash
man 7 signal
```
- Signals are used to communicate between processes or between the OS and a process. They can:
✅ Stop a process
✅ Restart a process
✅ Terminate a process
✅ Tell a process to reload its configuration

```bash
man systemd
```