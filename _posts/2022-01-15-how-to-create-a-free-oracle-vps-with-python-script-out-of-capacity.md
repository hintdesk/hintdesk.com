---
title: "How to create a free Oracle VPS with Python script (Out of capacity)?"
date: 2022-01-15 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: server
tags: oracle,vps
---

## Intro
Oracle allow us to create free resources (such as VM) at their cloud services for testing. For examples, a VM.Standard.A1.Flex with maximal 4 OCPU and 24GB RAM is a good deal to host a website. However, the deal isn't always available because the demand is always more then what Oracle can provide. If you try to create a free VM for now, you'll receive an error "Out of capacity" most of the time. In this post, I'll show you how to run a script which runs over the time and tries to create a VM until it gets success.

## Steps

- The script is a Python script. You need to install [Python](https://www.python.org/downloads/) on your computer. The script needs some settings so that it works properly. The values of variables can be get from following steps.
- Go to [Oracle Cloud](https://cloud.oracle.com/) and open an account. You'll need a master card for it. The **Home region** is where you want to create your VM. The **Home region** can't be changed after set. So choose your region wisely so that you can take advantage of the VM later.
- After creating an account, login to your account. On the the top right, click the profile icon and then select **User Settings**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_001.png'
    alt='User Settings'
%}

- Scroll down to **Resources**. Select **API Keys**. Click **Add API Key**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_002.png'
    alt='Add API Key'
%}

- Make sure **Generate API Key Pair** is selected, click **Download Private Key** and save it as file **oci_private_key.pem**.
- Click **Add**. Copy the contents from the textarea and save it as file **config**. 
- Put the **config** and **oci_private_key.pem** files together in same directory.
- Open the **config** file and edit the row of **key_file**. That is the path to your private key. NOTE: If you use Windows, the path must be an absolute path.

```terminal
key_file=oci_private_key.pem
```

- Go to [Oci Python script repository](https://github.com/chacuavip10/oci_auto) and download file **oci_auto.py**. Save it in the directory with the other files.
- Go back to [Oracle Cloud](https://cloud.oracle.com/) and **Create a VM instance**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_003.png'
    alt='Create a VM instance'
%}

- Find **Image and shape**. Click **Edit**. 

{%
    include image.html
    year='2022'
    month='01'
    file='15_004.png'
    alt='Image and shape'
%}

- Select the **Image** which you want to use, for example **Ubuntu**. 
- Click **Change shape**. Select **Ampere**. Set **Number of OCPUs** to **4** and **Amout of memory (GB)** to **24**. Then click **Select shape**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_005.png'
    alt='Image and shape'
%}

{%
    include image.html
    year='2022'
    month='01'
    file='15_006.png'
    alt='Image and shape'
%}

- Scroll down to **Networking**. Click **Edit**. Select **Do not assign a public IPv4 address**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_011.png'
    alt='Networking'
%}

{%
    include image.html
    year='2022'
    month='01'
    file='15_012.png'
    alt='Networking'
%}

- Scroll down to **Add SSH keys**. Click **Save Private Key** and **Save Public Key**. Store them to a safe place. We'll need it later.

{%
    include image.html
    year='2022'
    month='01'
    file='15_007.png'
    alt='Add SSH keys'
%}

- Before creating VM, press **F12** to open DevTools. Select tab **Network**.
- Click **Create**. You'll receive an error.

```terminal
Out of capacity for shape VM.Standard.A1.Flex in availability
domain OFDj:AP-SINGAPORE-1-AD-1. If you specified a fault domain,
try creating the instance without specifying a fault domain. If
that doesn’t work, please try again later. Learn more about host 
capacity.
```

{%
    include image.html
    year='2022'
    month='01'
    file='15_008.png'
    alt='DevTools'
%}


- In the DevTools, tab **Network**, search now for **instances** request. Right click on it. Select **Copy --> Copy as cURL (cmd)**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_009.png'
    alt='DevTools'
%}

- Copy the content to an editor. Find the part **--data-raw**.
- Open the **oci_auto.py** which you downloaded before and replace the values with the values in **--data-raw**.

```terminal
instance_display_name = 'instance-********'
compartment_id = 'ocid1.tenancy.oc1..********'
domain = "OFDj:AP-SINGAPORE-1-AD-1"  # availability_domain
image_id = "ocid1.image.oc1.ap-singapore-1.********"
subnet_id = 'ocid1.subnet.oc1.ap-singapore-1.********'
ssh_key = "ssh-rsa ******** ssh-key-2022-01-14"
```

- The setup is now finished. You can run the script with the following commands.

```terminal
pip install requests
pip install oci
python3 oci_auto.py
```

- If you see this, that means the script has run successfully.

{%
    include image.html
    year='2022'
    month='01'
    file='15_010.png'
    alt='Python'
%}

- It may take days until there is a free slot for you. The script will quit and give back a message when he creates a VM successfully.

{%
    include image.html
    year='2022'
    month='01'
    file='15_018.png'
    alt='VM created successfully'
%}

- After the VM gets created, go to [Instances](https://cloud.oracle.com/compute/instances/), select the newly created instance.
- Scroll down to **Resources**. Select **Attached VNICs**. Click on the newly create VNIC.
- Scroll down to **Resources**. Select **IPv4 Addresses**. **Edit** and select **Ephemeral public IP**. Click **Update**.

{%
    include image.html
    year='2022'
    month='01'
    file='15_013.png'
    alt='Public IP Addresses'
%}

{%
    include image.html
    year='2022'
    month='01'
    file='15_014.png'
    alt='Ephemeral public IP'
%}

- Open **PuTTY Key Generator**, **Load** the private key you saved before. Click **Save private key** to generate a key format (*.ppk) which can be used by Putty. Save it to a safe place.
- Open **PuTTY**, go to **Connection --> SSH --> Auth**, select the private key generated by **PuTTY Key Generator** before.

{%
    include image.html
    year='2022'
    month='01'
    file='15_016.png'
    alt='PuTTY'
%}

- In **PuTTY**, go to **Session**, enter the IP you get from instance details. Then **Open** the connection. You'll be asked for username which is also available in section of instance details.

{%
    include image.html
    year='2022'
    month='01'
    file='15_015.png'
    alt='Instance details'
%}

- If you have any question, you can start a new [discussion](https://github.com/hintdesk/hintdesk.com/discussions).



