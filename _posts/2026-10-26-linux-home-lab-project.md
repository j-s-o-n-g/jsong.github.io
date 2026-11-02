---
title: "Linux Home Lab Project"
date: 2026-10-26
categories: [Linux]
tags: [linux, homelab]     # TAG names should always be lowercase
comments: true
---

I set up this dual-node virtualization lab to practice hands-on Red Hat Enterprise Linux (RHEL 9) system administration in a realistic environment. To simulate real-world scenarios and RHCSA exam practice, I integrated a local simulation harness containing 25 practical tasks.

This post covers **Task 01: Configure networking and set hostname**, walking through each step from initial environment checks to configuring network parameters via NetworkManager and verifying persistence after reboot.

---

## Lab Environment

* **Hypervisor**: VMware Fusion running on macOS.
* **Nodes**: Two RHEL 9 virtual machines (`node1` and `node2`).
* **Network Interfaces on node1**:
  * `enp2s0`: Primary management network interface.
  * `rhcsa0`: Dedicated interface for practicing static network assignments.
* **Simulation Harness**: Local practice suite with a web interface at `http://localhost:8200` and automated CLI grading tools.

---

## Task 01 Requirements

* **Static IPv4 Address**: `172.25.250.11/24`
* **Gateway**: `172.25.250.254`
* **DNS Server**: `172.25.250.254`
* **Hostname**: `node1.lab.network17.com`
* **Autoconnect**: Enabled so the connection starts automatically on boot.
* **Target Interface**: Configure the profile bound to `rhcsa0`. Avoid reconfiguring the active management interface (`enp2s0`) to prevent losing access to the machine.

---

## Step-by-Step Walkthrough

### Step 1: Virtual Machine Cluster Setup

<img src="/assets/img/linux-homelab/01-vmware-dual-nodes.png" width="900" alt="VMware Fusion Dual RHEL Node Setup">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 1: Dual RHEL 9 virtual machines (node1 and node2) running side-by-side in VMware Fusion.
</p>

Two independent RHEL 9 virtual machines (`node1` on the left, `node2` on the right) are running in VMware Fusion. Each node operates with its own virtual disk, network adapters, and dedicated resources.

---

### Step 2: Console Verification

<img src="/assets/img/linux-homelab/02-vmware-node-verification.png" width="900" alt="Console verification on node1 and node2">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 2: Verifying root terminal access and shell responsiveness on both nodes.
</p>

Logged in as `root` on both machines and ran a quick echo command (`This is node1` and `This is node2`) to confirm terminal responsiveness and ensure commands are directed to the correct node.

---

### Step 3: Simulator Source Tree

<img src="/assets/img/linux-homelab/03-simulator-source-structure.png" width="900" alt="Simulator repository directory structure">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 3: RHCSA practice simulator source tree containing tasks, exams, tests, and CLI tools.
</p>

Checked the local simulator directory structure, which organizes task definitions, exam profiles, and automated grading scripts into `tasks/`, `exams/`, `tests/`, and `bin/`.

---

### Step 4: Accessing the Simulator Web Interface

<img src="/assets/img/linux-homelab/04-rhcsa-simulator-web-ui.png" width="900" alt="RHCSA Exam Simulator Web Dashboard">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 4: Simulator web interface at http://localhost:8200 displaying 25 practical tasks.
</p>

Opened the simulator web interface at `http://localhost:8200`. The exam consists of 25 practical tasks designed to replicate an RHCSA exam environment with an active countdown timer.

---

### Step 5: Reviewing Task 01 Requirements

<img src="/assets/img/linux-homelab/05-task01-network-requirements.png" width="900" alt="Task 01 networking and hostname requirements">
<p style="font-size: 14px; text-align: center; color: #666;">
  Figure 5: Task 01 prompt: Configure networking and set hostname.
</p>

Reviewed the instructions for Task 01:
* Configure interface `rhcsa0` with static IP `172.25.250.11/24`.
* Set Gateway to `172.25.250.254` and DNS to `172.25.250.254`.
* Set the hostname to `node1.lab.network17.com`.
* Ensure autoconnect is enabled.

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
