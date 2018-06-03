# Hands On With Metasploitable2
Nicholas Folse
March 2, 2018 

This tutorial covers setting up Metasploitable2 and Kali Linux using VirtualBox.

# Overview

[Metasploitable2](https://metasploit.help.rapid7.com/docs/metasploitable-2) is a virtual machine configured as an intentionally vulnerable Ubuntu Linux target. [Kali Linux](https://www.kali.org/) is a security-focused distribution of Linux that contains many tools that can be used for vulnerability and penetration testing. [VirtualBox](https://www.virtualbox.org) is a free-for-personal-use virtualization software. 

![Metasploitable2 test environment](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520011892723_image.png)


Our goal is to setup a small virtual network so we can test Metasploitable2 in a sand-boxed environment. We will create a dual-homed Kali virtual machine and a single-homed Metasploitable2 instance. We use the “Host-only Network” feature in Virtual Box to connect the two VMs together using a virtual switch.

# Configuring the Virtual Envrionment

This section describes the steps required to setup the virtual network for this exercise. You should be able to complete these tasks on Windows, Linux, or MacOS.

**Prerequisites:**

1. Install VirtualBox
2. Download Metasploitable2
3. Download Kali Linux (iso)

**VirtualBox Setup**

1. Create a host-only network (File > Preferences > Networking > Host-only Networks. Configure the network as shown in the following screen-shots.
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520012392973_image.png)
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520012400251_image.png)


**Metasploitable2 VM**
The Metasploitable2 VM is provided as a VMWare workstation disk image. We can use this directly in VirtualBox by creating a new virtual machine then configuring the VM to use the VMDK as the primary storage device.

1. Download and extract the Metasploitable2 image. You should have a folder containing the VMDK and several other files. The VMDK should be about 1.9 GiB in size.
2. Create a new virtual machine in Virtual Box. The VM should be Linux/Ubuntu (64-bit). On the **Hard Disk** selection page, choose the VMDK file you just downloaded.
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520012819665_image.png)
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520012876329_image.png)

3. Open the VM properties and change the network settings to use the Host-only Adapter you created earlier.
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013064645_image.png)

4. You should now be able to run the new VM by double-clicking on it in Virtual Box. 
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520012960668_image.png)


**Kali Virtual Machine**

1. Create a new Kali virtual machine. There are numerous tutorials on the web about how to do this.
2. After you’ve completed the basic configuration, change your network settings to match the following screen-shots:
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013328365_image.png)
![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013334264_image.png)

## Testing the Configuration
1. Launch the Metasploitable2 VM and let it run in the background
2. Launch the Kali VM and test basic network connectivity

Your Kali VM should be setup with *two* networks. The NAT network provides internet connectivity for updates. The host-only network allows you to communicate with the MSP2 VM.
**Check Kali Networking**

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013625749_image.png)


Kali doesn’t always automatically detect the correct network setup. 

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013742055_image.png)


If you can only enable one interface at a time, delete the **eth1** interface and re-create it. On the IPv4 page of the **host-only network**, choose “Use this connection only for resources on this network”.

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013814452_image.png)


Your network is configured correctly when you can reproduce the following result:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520013904850_image.png)

# Playground

You’re now ready to start playing with Metasploitable2. Kali includes a good variety of tools to get you started. You’ll probably want to start with a vulnerability scanner. 
**nmap**
nmap is a simple port scanner. 
**Sparta**
Sparta is a multi-staged scanner that will detect and identify open ports and services on a target machine or range.
**OpenVAS**
OpenVAS is a full-blown vulnerability scanner. It is more complicated to setup on Kali, but it’s a good exercise. Running the OpenVAS against the MSP2 target should result in detection of numerous vulnerabilities as shown in the following screenshot:

![OpenVAS scan of Metasploitable2 target](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520014311587_image.png)


**Armitage/Metasploit Framework**
Next, you can move on to attacking the host. Armitage is a very helpful GUI-based front-end to Metasploit. The following screen-shot demonstrates a successfully executed remote shell penetration to the MSP2 target:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C252F1BC4FE9D9B16E7A9551E7387F11BD337E03085773054AD6C11C40977E99_1520014600741_image.png)

## Next Steps

You can take two alternative approaches to playing with Metasploitable2. First, you can practice and explore attacking. Second, you can practice defending. 
**Attacking**
This is easy. The web is filled with examples about how to carry out exploits.
**Defending**
This is more useful in a dev-ops environment. Your goal should be to resolve all the issues identified by your vulnerability scanner by “fixing” the Metasploitable2 image.


## **References:**

Metasploitable2 - https://metasploit.help.rapid7.com/docs/metasploitable-2
Metasploitable2 Exploitability Guide - https://metasploit.help.rapid7.com/docs/metasploitable-2-exploitability-guide
Kali Linux - https://www.kali.org/
Virtual Box - https://www.virtualbox.org
