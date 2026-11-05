---
title: "Linux Home Lab Project with VMWare"
date: 2026-10-26
categories: [Linux]
tags: [linux, homelab, vmware]     # TAG names should always be lowercase
comments: true
---

I set up this dual-node virtualization lab to practice hands-on Red Hat Enterprise Linux (RHEL 9) system administration in a realistic environment. To simulate real-world scenarios and RHCSA exam practice, I integrated a local simulation harness containing 25 practical tasks.

---

## Lab Environment

* **Hypervisor**: VMware Fusion running on macOS.
* **Nodes**: Two RHEL 9 virtual machines (`node1` and `node2`).
* **Network Interfaces on node1**:
  * `enp2s0`: Primary management network interface.
  * `rhcsa0`: Dedicated interface for practicing static network assignments.
* **Simulation Harness**: Local practice suite with a web interface at `http://localhost:8200` and automated CLI grading tools.

---

<details markdown="1">
<summary><strong>Task 1: Configure networking and set hostname</strong></summary>

```text
configure networking and set hostname

Configure the spare network interface rhcsa0 on this system with the
following static IPv4 settings, make it start automatically at boot, and
set the system hostname:

  IPv4 address : 172.25.250.11/24
  Gateway      : 172.25.250.254
  DNS server   : 172.25.250.254
  Hostname     : node1.lab.network17.com

A connection profile named rhcsa0, already bound to the rhcsa0 interface, is
present on this system -- configure that profile and keep it bound to rhcsa0.
rhcsa0 is a dedicated, isolated test interface; do NOT reconfigure the NIC you
are connected through (e.g. enp1s0), or you will lose access to this system.
```

---

## Step-by-Step Walkthrough

### Step 1: Virtual Machine Cluster Setup & Console Verification

<img src="/assets/img/linux-homelab/02-vmware-node-verification.png" width="900" alt="Console verification on node1 and node2">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 1: Dual RHEL 9 virtual machines (node1 and node2) running side-by-side in VMware Fusion with active root terminal access.
</p>

Two independent RHEL 9 virtual machines (`node1` on the left, `node2` on the right) are running in VMware Fusion. Each node operates with its own virtual disk, network adapters, and dedicated resources. Logged in as `root` on both machines and ran a quick echo command (`This is node1` and `This is node2`) to confirm terminal responsiveness and ensure commands are directed to the correct node.

---

### Step 6: Checking Current Network Status

<img src="/assets/img/linux-homelab/06-baseline-nmcli-device-status.png" width="900" alt="nmcli connection show and nmcli device status on node1">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 6: Checking network profiles and devices with nmcli.
</p>

On `node1`, checked current connections and devices:
```bash
nmcli connection show
nmcli device status
```
* `enp2s0`: Connected ethernet interface.
* `rhcsa0`: Dummy network interface profile.

---

### Step 7: Launching `nmtui`

<img src="/assets/img/linux-homelab/07-launching-nmtui-terminal.png" width="900" alt="Launching nmtui in the terminal">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 7: Opening the NetworkManager Text User Interface (nmtui).
</p>

Launched `nmtui` in the terminal to configure network settings and hostname via the interactive menu.

---

### Step 8: `nmtui` Main Menu

<img src="/assets/img/linux-homelab/08-nmtui-menu-interface.png" width="900" alt="nmtui main selection menu">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 8: nmtui main menu options.
</p>

The menu provides three options: **Edit a connection**, **Activate a connection**, and **Set system hostname**. Selected **Edit a connection** to start configuring network parameters.

---

### Step 9: Selecting Connection to Edit

<img src="/assets/img/linux-homelab/09-nmtui-connection-selection.png" width="900" alt="Selecting connection profile in nmtui">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 9: List of available network connections in nmtui.
</p>

The connection list shows available Ethernet connections (`enp2s0` and `rhcsa0`). Selected `<Edit>` to configure connection details.

---

### Step 10: Entering Static IPv4 Settings

<img src="/assets/img/linux-homelab/10-nmtui-edit-ipv4-configuration.png" width="900" alt="Editing IPv4 configuration in nmtui">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 10: Entering manual IPv4 settings, gateway, and DNS servers.
</p>

Configured IPv4 settings to `<Manual>` and entered:
* **Addresses**: `172.25.250.11/24`
* **Gateway**: `172.25.250.254`
* **DNS servers**: `172.25.250.254`
* Verified that **Automatically connect** is checked.

---

### Step 11: Saving Connection Changes

<img src="/assets/img/linux-homelab/11-nmtui-connection-saved.png" width="900" alt="Saving connection profile in nmtui">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 11: Returning to the connection list after saving.
</p>

Selected `<OK>` to save the configuration changes and returned to the connection menu.

---

### Step 12: Setting System Hostname

<img src="/assets/img/linux-homelab/12-nmtui-set-hostname-dialog.png" width="900" alt="Setting system hostname via nmtui">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 12: Entering the hostname node1.lab.network17.com in nmtui.
</p>

Navigated back to the main menu, selected **Set system hostname**, and entered `node1.lab.network17.com`.

---

### Step 13: Hostname Confirmation

<img src="/assets/img/linux-homelab/13-nmtui-hostname-confirmed.png" width="900" alt="Hostname update confirmation">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 13: Confirmation popup confirming the hostname update.
</p>

Confirmed that the hostname was updated to `node1.lab.network17.com`. Selected `<OK>` and exited `nmtui`.

---

### Step 14: Verifying IP Address Configuration

<img src="/assets/img/linux-homelab/14-verify-ip-addr-discrepancy.png" width="900" alt="ip addr command output">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 14: Checking IP addresses with ip addr.
</p>

Ran `ip addr` in the terminal to inspect the active network configuration:
```bash
ip addr
```
Checked the assigned IP addresses on each network interface to verify the changes.

---

### Step 15: Verifying Routing Table

<img src="/assets/img/linux-homelab/15-verify-ip-route-table.png" width="900" alt="ip route command output">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 15: Inspecting the kernel routing table with ip route.
</p>

Ran `ip route` to check the default gateway and network routes:
```bash
ip route
```
Verified that the default route via `172.25.250.254` is in place.

---

### Step 16: Verifying Hostname

<img src="/assets/img/linux-homelab/16-verify-hostname-command.png" width="900" alt="hostname command output">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 16: Checking the active system hostname.
</p>

Ran `hostname` to confirm the active hostname:
```bash
hostname
```
Output returned `node1.lab.network17.com`.

---

### Step 17: Verifying DNS Configuration

<img src="/assets/img/linux-homelab/17-verify-resolv-conf-dns.png" width="900" alt="cat /etc/resolv.conf output">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 17: Checking /etc/resolv.conf for nameserver entries.
</p>

Verifying that DNS is properly configured.

---

### Step 18: Rebooting the System

<img src="/assets/img/linux-homelab/18-execute-reboot-persistence-test.png" width="900" alt="Executing reboot command on node1">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 18: Executing the reboot command.
</p>

Issued the `reboot` command to restart the system:
```bash
reboot
```

---

### Step 19: System Rebooting

<img src="/assets/img/linux-homelab/19-rhel-system-rebooting.png" width="900" alt="Red Hat Enterprise Linux boot splash screen">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 19: RHEL 9 rebooting.
</p>

If the network configuration remains intact after rebooting, then it's a success!

</details>

---

<details markdown="1">
<summary><strong>Task 2: Configure package repositories</strong></summary>

```text
Configure the package repositories in /etc/yum.repos.d/app.repo. Define two
repositories — BaseOS and AppStream — both enabled with GPG check disabled,
pointing at the provided package source:

  BaseOS    -> file:///opt/rhcsa-repo/BaseOS
  AppStream -> file:///opt/rhcsa-repo/AppStream
```

---

## Step-by-Step Walkthrough

### Step 1: Checking System Credentials & Switching to Root

<img src="/assets/img/linux-homelab/20-task02-config-info.png" width="900" alt="Checking configuration info and elevating to root">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 20: Checking root password in the exam configuration portal and switching to root on node1.
</p>

Checked the exam configuration information page (`http://localhost:8200/config`), which lists the host IP addresses and root password (`trootent`) for `node1.example.com`. Switched to `root` using `su - root`.

---

### Step 2: Reviewing Task 02 Prompt & Opening Repository File

<img src="/assets/img/linux-homelab/21-task02-prompt-and-vi.png" width="900" alt="Reviewing Task 02 prompt and opening app.repo">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 21: Reviewing Task 02 requirements and opening /etc/yum.repos.d/app.repo with vi.
</p>

Reviewed the task requirements for Task 02: configure package repositories in `/etc/yum.repos.d/app.repo` with `BaseOS` and `AppStream` pointing to `file:///opt/rhcsa-repo/BaseOS` and `file:///opt/rhcsa-repo/AppStream`. Opened the file with `vi`:
```bash
vi /etc/yum.repos.d/app.repo
```

---

### Step 3: Defining Repository Stanzas in `app.repo`

<img src="/assets/img/linux-homelab/22-task02-editing-app-repo.png" width="900" alt="Configuring BaseOS and AppStream repositories in vi">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 22: Writing the repository definitions in /etc/yum.repos.d/app.repo and saving with :wq.
</p>

Defined both repository stanzas in `/etc/yum.repos.d/app.repo`:
```ini
[BaseOS]
name=BaseOS
baseurl=file:///opt/rhcsa-repo/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=BaseOS
baseurl=file:///opt/rhcsa-repo/AppStream
enabled=1
gpgcheck=0
```
Saved and exited the editor with `:wq`.

---

### Step 4: Cleaning Cache & Verifying Repositories

<img src="/assets/img/linux-homelab/23-task02-yum-clean-repolist.png" width="900" alt="Running yum clean all and yum repolist -v">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 23: Cleaning YUM cache and verifying configured repositories with yum repolist -v.
</p>

Ran `yum clean all` to remove cached metadata, followed by `yum repolist -v` to verify that the local BaseOS and AppStream repositories loaded properly:
```bash
yum clean all
yum repolist -v
```
Confirmed that the repository metadata was loaded from `/opt/rhcsa-repo/`, and marked Task 02 as `Done` in the exam simulator.

</details>

---

<details markdown="1">
<summary><strong>Task 3: Configuring and Securing an Apache HTTP Service</strong></summary>

```text
On Node1, configure the Apache HTTP Server to meet the following requirements:

* The Apache web service must be installed, enabled, and running.
* The web server must listen on TCP port 85.
* The service must be accessible from both the local system and external hosts.
* When curled or accessed via a web browser, the server must display the following message:
  Welcome to the Apache Web Server!
```

---

## Step-by-Step Walkthrough

### Step 1: Checking Initial HTTP Service Status

<img src="/assets/img/linux-homelab/24-task03-httpd-failed-status.png" width="900" alt="Checking httpd status">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 24: Inspecting httpd service status with systemctl status httpd showing failed status.
</p>

Checked current `httpd` status. The service was in `failed` state due to `Permission denied` when trying to bind to a non-standard port under SELinux:
```bash
systemctl status httpd
```

---

### Step 2: Verifying Package Installation

<img src="/assets/img/linux-homelab/25-task03-dnf-install-httpd.png" width="900" alt="Installing httpd with dnf">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 25: Checking httpd package installation with dnf install -y httpd.
</p>

Ran `dnf install -y httpd` to ensure the Apache package is installed:
```bash
dnf install -y httpd
```
Confirmed that `httpd-2.4.63-13.el10.aarch64` is already installed.

---

### Step 3: Opening Apache Configuration

<img src="/assets/img/linux-homelab/26-task03-vi-httpd-conf.png" width="900" alt="Opening httpd.conf with vi">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 26: Opening /etc/httpd/conf/httpd.conf with vi.
</p>

Opened the primary Apache configuration file:
```bash
vi /etc/httpd/conf/httpd.conf
```

---

### Step 4: Configuring Listen Port 85

<img src="/assets/img/linux-homelab/27-task03-editing-listen-85.png" width="900" alt="Adding Listen 85 in httpd.conf">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 27: Adding Listen 85 directive in httpd.conf and saving with :wq.
</p>

Added the `Listen 85` directive in `/etc/httpd/conf/httpd.conf` and saved the file with `:wq`.

---

### Step 5: Allowing Port 85 in SELinux Policy

<img src="/assets/img/linux-homelab/28-task03-semanage-port-85.png" width="900" alt="Configuring SELinux port policy">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 28: Adding TCP port 85 to http_port_t SELinux type with semanage port.
</p>

Configured SELinux to permit Apache to bind to non-standard TCP port 85:
```bash
semanage port -a -t http_port_t -p tcp 85
```

---

### Step 6: Enabling and Starting Apache Service

<img src="/assets/img/linux-homelab/29-task03-systemctl-enable-start.png" width="900" alt="Enabling and starting httpd service">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 29: Enabling and restarting httpd service.
</p>

Enabled the service to start automatically at boot and restarted `httpd`:
```bash
systemctl enable --now httpd
systemctl restart httpd
```

---

### Step 7: Configuring Firewall for Port 85

<img src="/assets/img/linux-homelab/30-task03-firewall-add-port.png" width="900" alt="Configuring firewalld for port 85">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 30: Allowing TCP port 85 in firewalld and reloading rules.
</p>

Opened TCP port 85 permanently in `firewalld` to allow external network access:
```bash
firewall-cmd --permanent --add-port=85/tcp
firewall-cmd --reload
```

---

### Step 8: Verifying Active Service Status

<img src="/assets/img/linux-homelab/31-task03-httpd-active-running.png" width="900" alt="Verifying httpd active running status">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 31: Verifying active (running) status of httpd.
</p>

Ran `systemctl status httpd` to verify that the service is `active (running)` and enabled at boot:
```bash
systemctl status httpd
```

</details>

<details markdown="1">
<summary><strong>Task 4: Manage Users and Groups</strong></summary>

```text
Manage users and groups

Requirements:
- Create the group named "sysadms".
- Create users natasha and harry; they should be members of the supplementary group sysadms.
- Create a user sarah who should have a non-interactive shell and should not be a member of sysadms.
- Set passwords for all users as "trootent".
- Configure sudo so that all members of the sysadms group can execute all commands with the privileges of any user (including root) without entering a password.
- Configure sudo so that the user harry can execute the passwd command with the privileges of any user (including root) without entering a password.
- Configure sudo so that all members of the sysadms group can execute the useradd command with the privileges of any user (including root).
```

---

## Step-by-Step Walkthrough

### Step 1: Creating the sysadms Group

<img src="/assets/img/linux-homelab/32-task04-groupadd-sysadms.png" width="900" alt="Creating group sysadms">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 32: Creating the administrative group sysadms using groupadd.
</p>

Created the requested administrative group `sysadms`:
```bash
groupadd sysadms
```

---

### Step 2: Creating Users natasha and harry with Supplementary Group

<img src="/assets/img/linux-homelab/33-task04-useradd-natasha-harry.png" width="900" alt="Creating users natasha and harry">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 33: Creating users natasha and harry with supplementary group sysadms.
</p>

Created `natasha` and `harry`, assigning `sysadms` as their supplementary group:
```bash
useradd -G sysadms natasha
useradd -G sysadms harry
```

---

### Step 3: Configuring User sarah with Non-Interactive Shell

<img src="/assets/img/linux-homelab/34-task04-usermod-sarah-nologin.png" width="900" alt="Setting non-interactive shell for sarah">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 34: Setting non-interactive shell (/sbin/nologin) for user sarah.
</p>

Created or updated user `sarah` to have a non-interactive login shell (`/sbin/nologin`) and no membership in `sysadms`:
```bash
useradd -s /sbin/nologin sarah
usermod -s /sbin/nologin sarah
```

> User passwords for `natasha`, `harry`, and `sarah` are set to `trootent` as required:
> ```bash
> echo "trootent" | passwd --stdin natasha
> echo "trootent" | passwd --stdin harry
> echo "trootent" | passwd --stdin sarah
> ```

---

### Step 4: Accessing Sudoers Configuration with visudo

<img src="/assets/img/linux-homelab/35-task04-visudo-command.png" width="900" alt="Opening visudo">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 35: Opening sudoers configuration file safely using visudo.
</p>

Used `visudo` to safely edit the sudoers configuration with syntax validation:
```bash
visudo
```

---

### Step 5: Verifying Absolute Command Paths

<img src="/assets/img/linux-homelab/36-task04-which-passwd-useradd.png" width="900" alt="Finding binary paths with which">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 36: Resolving absolute paths for passwd and useradd commands.
</p>

Checked the exact binary paths for commands specified in sudo rules:
```bash
which passwd
which useradd
```
Output confirmed paths:
- `/usr/bin/passwd`
- `/usr/sbin/useradd`

---

### Step 6: Configuring Sudo Rules in sudoers

<img src="/assets/img/linux-homelab/37-task04-editing-sudoers.png" width="900" alt="Adding sudo rules in visudo">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 37: Adding sudoers permission rules for sysadms group and harry.
</p>

Appended the required sudo permission rules at the end of the sudoers configuration:
```text
%sysadms ALL=(ALL) NOPASSWD: ALL

harry ALL=(ALL) NOPASSWD: /usr/bin/passwd

%sysadms ALL=(ALL) /usr/sbin/useradd
```
Saved the configuration and exited using `:wq`.

---

### Step 7: Verifying Configuration Completion

<img src="/assets/img/linux-homelab/38-task04-visudo-completed.png" width="900" alt="Completing visudo edit">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 38: Completing sudoers rule modification and returning to the shell prompt.
</p>

`visudo` validated the syntax on save with no parse errors, returning cleanly to the root shell prompt:
```bash
visudo
```

</details>

<details markdown="1">
<summary><strong>Task 5: Crontab Configuration and Access Control</strong></summary>

```text
Crontab Type 1

Requirements:
- Schedule a job to run every day at 14:32:
  - User: natasha
  - Command: /bin/echo hiya
- Configure the system so that only the user tom is allowed to create cron jobs.
```

---

## Step-by-Step Walkthrough

### Step 1: Checking Crond Service Status

<img src="/assets/img/linux-homelab/39-task05-crond-initial-status.png" width="900" alt="Checking crond service status">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 39: Verifying initial crond service status and configuration requirements.
</p>

Checked whether the cron daemon `crond` is active and running:
```bash
systemctl status crond
```
The service was active (running) and enabled on boot.

---

### Step 2: Accessing User Crontab for natasha

<img src="/assets/img/linux-homelab/40-task05-crontab-natasha-edit.png" width="900" alt="Opening crontab for user natasha">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 40: Opening crontab editor for user natasha using crontab -u natasha -e.
</p>

Opened `natasha`'s crontab using root privileges:
```bash
crontab -u natasha -e
```

---

### Step 3: Setting the Scheduled Task for natasha

<img src="/assets/img/linux-homelab/41-task05-editing-cron-job.png" width="900" alt="Configuring cron schedule and command">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 41: Configuring daily schedule at 14:32 to run /bin/echo hiya.
</p>

Configured the required schedule (`minute: 32`, `hour: 14`, daily) and command:
```text
32 14 * * * /bin/echo hiya
```
Saved the file and exited `:wq`.

---

### Step 4: Configuring Cron Access Control

<img src="/assets/img/linux-homelab/42-task05-vi-cron-allow.png" width="900" alt="Opening /etc/cron.allow">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 42: Confirming crontab installation and opening /etc/cron.allow.
</p>

After saving `natasha`'s cron job (`crontab: installing new crontab`), opened `/etc/cron.allow` to restrict cron access:
```bash
vi /etc/cron.allow
```

---

### Step 5: Restricting Cron Access to User tom

<img src="/assets/img/linux-homelab/43-task05-editing-cron-allow.png" width="900" alt="Adding user tom to /etc/cron.allow">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 43: Adding user tom to /etc/cron.allow to restrict crontab creation.
</p>

Added `tom` to `/etc/cron.allow`:
```text
tom
```
When `/etc/cron.allow` exists, only users listed within this file are permitted to create cron jobs, satisfying the requirement that only `tom` is allowed to create cron jobs. Saved and exited with `:wq`.

---

### Step 6: Enabling and Verifying Crond Service

<img src="/assets/img/linux-homelab/44-task05-crond-service-verification.png" width="900" alt="Enabling and verifying crond service">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 44: Enabling and verifying crond service status.
</p>

Ensured the daemon is enabled and running:
```bash
systemctl enable --now crond
systemctl status crond
```
Confirmed that `crond.service` is active (running) and loaded properly.

</details>

<details markdown="1">
<summary><strong>Task 6: Configure a Collaboration Directory</strong></summary>

```text
Collaborative Directory

Requirements:
- Create the directory "/home/manager".
- Group ownership of "/home/manager" should go to the "sysadms" group.
- The directory should have full permission for all members of the "sysadms" group, but not for other users except "root".
- The SGID (SETGID) bit must be set so that newly created files inherit group ownership from the directory.
```

---

## Step-by-Step Walkthrough

### Step 1: Creating the Directory and Setting Group Ownership

<img src="/assets/img/linux-homelab/45-task06-mkdir-chown-sysadms.png" width="900" alt="Creating /home/manager and changing group to sysadms">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 45: Creating the /home/manager directory and setting group ownership to sysadms.
</p>

Created the requested directory `/home/manager` and changed its group ownership to `sysadms`:
```bash
mkdir -p /home/manager
chown :sysadms /home/manager
```

---

### Step 2: Configuring Permissions with the SGID Bit

<img src="/assets/img/linux-homelab/46-task06-chmod-sgid-permissions.png" width="900" alt="Setting SGID bit and 2770 permissions">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 46: Applying SGID bit and 2770 permissions on /home/manager.
</p>

Configured directory permissions using numeric mode `2770`:
```bash
chmod 2770 /home/manager
```

- **Special permission (`2`)**: Sets the SGID (Set Group ID) bit (`chmod g+s`), ensuring that any files and subdirectories created inside `/home/manager` automatically inherit the `sysadms` group ownership rather than the primary group of the creating user.
- **Owner (`7`)**: Full `rwx` permissions for the directory owner (`root`).
- **Group (`7`)**: Full `rwx` permissions for members of `sysadms`.
- **Others (`0`)**: No permissions (`---`) for any other users on the system.

</details>

<details markdown="1">
<summary><strong>Task 7: Configure Time Synchronization with Chrony</strong></summary>

```text
Configure your system

Requirements:
- Configure this system to synchronise its time from the NTP server:
    ntp1.example.com
- Use the iburst option on the server line.
- Ensure the chronyd service is enabled and running.
```

---

## Step-by-Step Walkthrough

### Step 1: Checking Initial Time and NTP Status

<img src="/assets/img/linux-homelab/47-task07-timedatectl-initial-status.png" width="900" alt="Checking timedatectl status">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 47: Checking current system time, timezone, and NTP service status using timedatectl.
</p>

Checked current time synchronization parameters on `node1`:
```bash
timedatectl
```
The system confirmed active NTP service and current timezone configuration.

---

### Step 2: Inspecting chronyd Service Status

<img src="/assets/img/linux-homelab/48-task07-chronyd-service-status.png" width="900" alt="Checking chronyd status">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 48: Inspecting chronyd service status showing active but disabled state.
</p>

Inspected the `chronyd` systemd service status:
```bash
systemctl status chronyd
```
While active and running, the service unit was marked as `disabled`, meaning it would not survive a reboot without explicit enablement.

---

### Step 3: Enabling the chronyd Service on Boot

<img src="/assets/img/linux-homelab/49-task07-systemctl-enable-chronyd.png" width="900" alt="Enabling chronyd service">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 49: Enabling chronyd to start automatically at system boot.
</p>

Enabled and ensured `chronyd` runs automatically upon boot:
```bash
systemctl enable --now chronyd
```
Systemd created the required symlink in `/etc/systemd/system/multi-user.target.wants/`.

---

### Step 4: Configuring the NTP Server in /etc/chrony.conf

<img src="/assets/img/linux-homelab/50-task07-editing-chrony-conf.png" width="900" alt="Editing /etc/chrony.conf">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 50: Adding the server directive with iburst in /etc/chrony.conf.
</p>

Opened `/etc/chrony.conf` and added the specified NTP server with the `iburst` option:
```bash
vi /etc/chrony.conf
```
Appended the following line to the configuration:
```text
server ntp1.example.com iburst
```
Saved and exited using `:wq`.

---

### Step 5: Restarting chronyd Service

<img src="/assets/img/linux-homelab/51-task07-restart-chronyd.png" width="900" alt="Restarting chronyd">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 51: Restarting chronyd service to apply configuration changes.
</p>

Restarted the service to reload the updated NTP server configuration:
```bash
systemctl restart chronyd
```

---

### Step 6: Verifying Time Synchronization

<img src="/assets/img/linux-homelab/52-task07-timedatectl-verification.png" width="900" alt="Verifying time sync with timedatectl">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 52: Verifying synchronized system clock and active NTP service.
</p>

Verified that time synchronization remains active and operational:
```bash
timedatectl
```
The output confirmed `System clock synchronized: yes` and `NTP service: active`.

</details>

<details markdown="1">
<summary><strong>Task 8: Configure AutoFS for On-Demand Network Mounts</strong></summary>

```text
Configure AutoFS

Requirements:
- Configure AutoFS to automatically mount the remote NFS share /rhome/remoteuser5 from node5.domain250.example.com to the local mount point /rhome/remoteuser5 on demand.
- The configuration must persist across system reboots.
```

---

## Step-by-Step Walkthrough

### Step 1: Installing AutoFS and NFS Utilities

<img src="/assets/img/linux-homelab/53-task08-install-autofs-nfs-utils.png" width="900" alt="Installing autofs and nfs-utils packages">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 53: Checking and installing autofs and nfs-utils packages using dnf.
</p>

Installed the automounter daemon `autofs` and client NFS utilities `nfs-utils`:
```bash
dnf install -y autofs nfs-utils
```
Both packages were verified as installed and up-to-date on the system.

---

### Step 2: Configuring the Master Map File (/etc/auto.master)

<img src="/assets/img/linux-homelab/54-task08-edit-auto-master.png" width="900" alt="Configuring /etc/auto.master">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 54: Configuring the master map in /etc/auto.master to define the base mount directory and secondary map.
</p>

Edited `/etc/auto.master` to register the base mount directory `/rhome` and specify its mapping configuration file `/etc/auto.rhome`:
```bash
vi /etc/auto.master
```
Appended the following entry to the master map:
```text
/rhome /etc/auto.rhome
```
Saved the configuration and exited using `:wq`.

---

### Step 3: Opening the Secondary Map File (/etc/auto.rhome)

<img src="/assets/img/linux-homelab/55-task08-open-auto-rhome.png" width="900" alt="Opening /etc/auto.rhome with vi">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 55: Creating and opening the AutoFS secondary map file /etc/auto.rhome with vi.
</p>

Opened `/etc/auto.rhome` with `vi` to configure the individual mount map definition:
```bash
vi /etc/auto.rhome
```

---

### Step 4: Configuring the Remote NFS Mount Map

<img src="/assets/img/linux-homelab/56-task08-edit-auto-rhome.png" width="900" alt="Editing /etc/auto.rhome mount configuration">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 56: Specifying the mount key, mount options, and remote NFS export path in /etc/auto.rhome.
</p>

Defined the target mount key, filesystem options, and source NFS share in `/etc/auto.rhome`:
```text
remoteuser5 -rw,sync node5.domain250.example.com:/rhome/remoteuser5
```

- **Key (`remoteuser5`)**: The subdirectory name created dynamically under `/rhome` upon access (`/rhome/remoteuser5`).
- **Options (`-rw,sync`)**: Mount options specifying read-write access and synchronous data writing.
- **Location (`node5.domain250.example.com:/rhome/remoteuser5`)**: The remote NFS server hostname and exported directory path.

Saved and exited using `:wq`.

---

### Step 5: Enabling and Restarting the AutoFS Service

<img src="/assets/img/linux-homelab/57-task08-enable-restart-autofs.png" width="900" alt="Enabling and restarting autofs service">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 57: Enabling the autofs service to start at boot and restarting the daemon.
</p>

Enabled `autofs` to start automatically on system boot and restarted the service to apply the new maps immediately:
```bash
systemctl enable --now autofs
systemctl restart autofs
```
With AutoFS active, any command attempting to access `/rhome/remoteuser5` dynamically triggers the NFS mount from `node5.domain250.example.com`, unmounting automatically after an idle period.

</details>
