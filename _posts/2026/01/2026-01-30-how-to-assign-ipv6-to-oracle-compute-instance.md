---
title: "How to assign an IPv6 Address to an Oracle compute instance?"
date: 2026-01-30 00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: server
tags: oracle
---

## Introduction
This guide provides step-by-step instructions for assigning an IPv6 address to an active Oracle Cloud Compute Instance. Follow the procedures below to ensure proper configuration and connectivity.

## Procedure

1. **Select the Target Instance**
   - Navigate to the list of compute instances and select the instance to which you wish to assign an IPv6 address.
   
   {% include image.html year='2026' month='01' file='30_001.png' alt='Select instance' %}

2. **Access the Virtual Cloud Network (VCN)**
   - In the **Details** tab, locate **Instance Details** and click on the associated Virtual Cloud Network.
   
   {% include image.html year='2026' month='01' file='30_002.png' alt='Select virtual cloud network' %}

3. **Add an IPv6 Prefix to the VCN**
   - In the **IP Administration** tab, click **Add CIDR Block/IPv6 Prefix**.
   
   {% include image.html year='2026' month='01' file='30_003.png' alt='Select virtual cloud network' %}
   - Select **Assign an Oracle allocated IPv6 /56 prefix** and click **Add CIDR Blocks/Prefixes**.
   
   {% include image.html year='2026' month='01' file='30_004.png' alt='Add CIDR Blocks/Prefixes' %}

   - After completion, you should see two entries as shown below:
   
   {% include image.html year='2026' month='01' file='30_005.png' alt='Add CIDR Blocks/Prefixes' %}

4. **Assign an IPv6 Prefix to the Subnet**
   - Under **Subnets**, select the relevant subnet.
   
   {% include image.html year='2026' month='01' file='30_006.png' alt='Add CIDR Blocks/Prefixes' %}
   - In the **IP Administration** tab, click **Add IPv6 Prefix**.
   
   {% include image.html year='2026' month='01' file='30_007.png' alt='Subnets --> Ip administration' %}
   - Choose **Assign an Oracle allocated IPv6 /64 Prefix** and click **Add IPv6 Prefix**.
   
   {% include image.html year='2026' month='01' file='30_008.png' alt='Add IPv6 prefix' %}

5. **Update the Routing Table**
   - Go to the **Routing** tab and select the appropriate routing table.
   
   {% include image.html year='2026' month='01' file='30_009.png' alt='Route Tables' %}
   - In **Route Rules**, click **Add Route Rules**.
   
   {% include image.html year='2026' month='01' file='30_010.png' alt='Add Route Rules' %}
   - Set the following:
     - **Type:** IPv6
     - **Target Type:** Internet Gateway
     - **Destination CIDR Block:** ::/0
   
   {% include image.html year='2026' month='01' file='30_011.png' alt='Add Route Rules' %}
   - You should now see two entries similar to the following:
   
   {% include image.html year='2026' month='01' file='30_012.png' alt='Add Route Rules' %}

6. **Configure Security Lists**
   - In the **Security** tab, select the relevant security list.
   
   {% include image.html year='2026' month='01' file='30_013.png' alt='Security list' %}
   - Under **Security Rules**, click **Add Ingress Rules**.
   
   {% include image.html year='2026' month='01' file='30_014.png' alt='Ingress Rules' %}
   - Add the following rule:
     - **Source CIDR:** ::/0
     - **IP Protocol:** All Protocols (or TCP with Destination Port 443)
   
   {% include image.html year='2026' month='01' file='30_015.png' alt='Ingress Rules' %}
   - The entries should appear as follows:
   
   {% include image.html year='2026' month='01' file='30_016.png' alt='Ingress Rules' %}
   - On the same tab, click **Add Egress Rules** and repeat the above configuration for egress traffic.
   
   {% include image.html year='2026' month='01' file='30_017.png' alt='Egress Rules' %}
   
   {% include image.html year='2026' month='01' file='30_015.png' alt='Ingress Rules' %}

7. **Assign an IPv6 Address to the VNIC**
   - Return to **Compute > Instances** and select your compute instance. In the **Networking** tab, select the VNIC.
   
   {% include image.html year='2026' month='01' file='30_018.png' alt='VNIC' %}
   - In the **IP Administration** tab, click **Assign IPv6 Address**.
   
   {% include image.html year='2026' month='01' file='30_019.png' alt='Assign IPv6 address' %}
   - Leave all settings at their default values and click **Assign**. A new IPv6 address will be generated.
   
   {% include image.html year='2026' month='01' file='30_020.png' alt='Assign IPv6 address' %}
   
   {% include image.html year='2026' month='01' file='30_021.png' alt='Assign IPv6 address' %}

8. **Configure the Compute Instance for IPv6**
   - Connect to your compute instance via SSH. List the netplan configuration files:
     
     ```sh
     ls /etc/netplan
     ```
   - By default, you should see a file named **50-cloud-init.yaml**. Open it for editing:
     
     ```sh
     sudo nano /etc/netplan/50-cloud-init.yaml
     ```
   - Add the following line to enable DHCP for IPv6:
     
     `dhcp6: true`
   
   {% include image.html year='2026' month='01' file='30_022.png' alt='Enable dhcp6' %}

9. **Verify IPv6 Connectivity**
   - To confirm that your instance now has IPv6 connectivity, run:
     
     ```sh
     curl -6 ifconfig.me
     ```