# Week 3: Enterprise Server Deployment and Operating System Installation

# 

# Course: ITEP 414, System Administration and Maintenance 

# Student: Balbieran, Timothy Kyl Q. 

# Section: BSIT 4E 

# Date: August 15, 2026

# 

# Project Overview

# 

# This project covers deploying an Ubuntu Server virtual machine for a fictional startup, ABC Startup Solutions, as part of a Junior System Administrator role. The work included installing Ubuntu Server, configuring it, verifying that it works correctly, comparing it against Windows Server and Rocky Linux, and documenting the whole process so another administrator could reproduce it without extra help.

# 

# Learning Objectives

# Explain the purpose of an operating system in enterprise environments.

# Differentiate BIOS and UEFI firmware.

# Explain the stages of the computer boot process.

# Compare Ubuntu Server, Windows Server, and Rocky Linux.

# Install and configure a Linux server in a virtual machine.

# Enable secure remote administration using SSH.

# Verify server functionality and document the process professionally.

# Virtual Machine Specifications

# Component	Specification

# Name	Ubuntu-Server-Week03

# RAM	4 GB

# CPU	2 Virtual Processors

# Storage	40 GB

# Network	NAT

# Hostname	server01

# Installation Summary

# 

# Ubuntu Server LTS was installed in VirtualBox using the manual (non-unattended) installer. Language was set to English, keyboard layout to English (US), and DHCP was accepted for networking. Storage used Guided, Use Entire Disk with LVM on the full 40 GB disk. The hostname was set to server01, a non-root administrator account was created, and OpenSSH server was selected for installation. For the bring-home activity, Windows Server 2025 Standard Evaluation (Desktop Experience) was also installed in a separate VM with matching specs (4 GB RAM, 2 vCPUs, 40 GB disk) so it could be compared directly against the Ubuntu deployment. The computer name for that VM was changed from the default WIN-4QO74L2FN8G to WinServer01.

# 

# Configuration Summary

# Setting	Value

# Username	balbieran

# Network Interface	enp0s3

# Assigned IPv4 Address	10.0.2.15/24

# Network Configuration	DHCP

# Network Mode	NAT

# File System	ext4

# Partition Scheme	Guided, Use Entire Disk with LVM (ubuntu-vg volume group)

# Disk Size	40.00 GB total (18.996 GB root, 2.000 GB boot)

# Verification Results

# Command	Purpose	Result

# hostname	Verify hostname	Returned server01

# ip addr	View IP address	enp0s3 up, 10.0.2.15/24

# ping -c 4 google.com	Test internet connectivity	4 packets transmitted, 4 received, 0% packet loss

# sudo apt update \&\& sudo apt upgrade -y	Update system	Completed after resolving a mirror error (see Challenges below)

# systemctl status ssh	Verify SSH service	active (running) after manual install (see Challenges below)

# BIOS vs UEFI Highlights

# 

# BIOS is older firmware that boots using the Master Boot Record, caps disk support at 2 TB, and has no built-in security. UEFI is the modern replacement, boots using the GUID Partition Table, supports much larger drives, boots faster, and includes Secure Boot to block unsigned or tampered code from running. Nearly all new hardware ships with UEFI today, including the VM used for this project, and BIOS is mostly limited to legacy systems now.

# 

# Boot Process Flowchart

# 

# Power On -> BIOS/UEFI Initialization -> Boot Device Detection -> GRUB Boot Loader -> Linux Kernel -> init/systemd -> Services Start -> Login Prompt

# 

# See diagrams/BootProcessFlowchart.png for the full flowchart.

# 

# Challenges Encountered

# During sudo apt upgrade, the package linux-firmware-amd-misc kept failing to download from the Philippines Ubuntu mirror with a 403 Forbidden error. This did not affect the server since it is optional firmware not needed by the VM. It was resolved by re-running sudo apt update and then sudo apt full-upgrade -y --fix-missing to skip the unavailable package.

# The "Install OpenSSH server" checkbox during Ubuntu setup did not apply correctly, and systemctl status ssh came back with "Unit ssh.service could not be found." This was fixed by manually installing SSH with sudo apt install openssh-server -y, then enabling and starting it with sudo systemctl enable ssh and sudo systemctl start ssh.

# Reflection

# 

# Working through this project made it clear that installing a server is really just the first step, the real work is in configuring it correctly and then actually checking that everything works the way it should. Following the Ubuntu installer step by step was straightforward, but running into the SSH issue afterward was a good reminder that checking a box during setup does not always mean the setting actually took effect. Verifying every service manually with commands like systemctl status ssh turned out to be just as important as the installation itself.

# 

# Comparing Ubuntu Server against Windows Server and Rocky Linux also helped connect the BIOS versus UEFI research and the boot process flowchart to something concrete, instead of just being facts to memorize. Seeing how the server actually boots, from firmware initialization through GRUB and into systemd, made the whole process feel less abstract. Installing Windows Server side by side with Ubuntu also made the differences in licensing, interface, and typical use cases much easier to understand than just reading about them would have been.

# 

# The mirror error and the SSH issue were both small problems in the end, but they were useful because they forced troubleshooting rather than just following steps that worked on the first try. That felt closer to what an actual system administrator deals with day to day. Documenting each step, including the problems and how they were fixed, also reinforced why clear documentation matters. If another administrator had to pick this server up later, they would be able to see exactly what was done and why, instead of having to guess or start over. Overall, this project was a solid introduction to what deploying and maintaining a real server actually involves, beyond just clicking through an installer.

# 

# References

# Canonical Ltd. (2024). Ubuntu Server documentation. https://documentation.ubuntu.com/server/

# Microsoft. (2024). Windows Server evaluations. https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025

# Red Hat, Inc. (2024). What is Rocky Linux? https://www.redhat.com/en/topics/linux/what-is-rocky-linux

# Microsoft. (2024). UEFI firmware. https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-uefi

