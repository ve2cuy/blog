# My home data center

*Document converti depuis [ve2cuy.com/blog](https://ve2cuy.com/blog/index.php/my-home-data-center/) — dernière modification : 2014-01-30*

---

<img src="images/My-home-data-center/datacenter000x1080-768x512.png" alt="datacenter000x1080" width="315" />

Here’s an update video of my home Data Center

Enjoy!

▶️ Vidéo YouTube : [Voir la vidéo](https://www.youtube.com/watch?v=N6ug8hyH6P0)

---

1. [Introduction](#a1)
2. [Pictures & videos](#a2)
3. [Detailed view of the racks](#a3)
4. [Electricity](#a4)
5. [Servers under vCenter](#a5)
6. [Hardware and topology](#a6)
7. [Home VOIP services](#a7)
8. [BladeCenter](#a8)
9. [Reference manuals](#a99)

# 1 – Introduction

In another career, I taught telecommunications, operating systems and computer networks. Nowadays, I mainly teach application development for iOS in Xcode. For those of you who read French, I have an ‘[Introduction to Xcode](http://tim.cstj.qc.ca/cours/xcode/wp/)’ blog. But I still like to play with computers so, just to keep my hand in; I’ve built myself a small data center at home. So what kind of testing goes on here?

- Hardware:  enterprise class servers – Dell, HP, IBM –  SAS, RAID 5, 6 + spare, RAID 50, 60, hot swap, UPS, F.C. SAN, Fibre switch, tape backup (IBM LTO3), …
- OS – Linux, Windows, OpenVMS, z/OS, HP-UX, IBM AIX, …
- Virtualization – VMware vSphere Hypervisor, vCenter server, vMotion, openStack, Xen, …
- Network – switch, CLI, LACP, MSTP, ACL, topology, fiber optic, vlan, firewall, traffic shaper, trust and untrust LAN, IPV6, …
- VOIP – Asterisk, VOIP, POE (Dell powerConnect 3448P), IP phones (Nortel 1230), SIP, …
- And much more …

Enjoy your tour in my data center!

# 2 – Let’s look at a few pictures

Here are some pictures of the data center taken in the summer of 2014:

<img src="images/My-home-data-center/datacenter04-200x300.png" alt="datacenter04" width="100" />

<img src="images/My-home-data-center/datacenter03-200x300.png" alt="datacenter03" width="100" />

<img src="images/My-home-data-center/datacenter01-200x300.png" alt="datacenter01" width="100" />

<img src="images/My-home-data-center/datacenter02-300x200.png" alt="datacenter02" width="150" />

<img src="images/My-home-data-center/datacenter00-300x200.png" alt="datacenter00" width="150" />

▶️ Vidéo YouTube : [Voir la vidéo](https://www.youtube.com/watch?v=N1TZylWMVC4)

# 3 – Detailed view of the racks

To get a detailed view of the hardware in the data center, click on the picture below: Note: what you don’t see in this picture: IBM system P5 (AIX),  IBM AS400, HP rx2620, 2 x VAX-4000 200, Micro VAX and a  Juniper NetScreen.

**Winter 2014**

<img src="images/My-home-data-center/datacenter-map-1920-768x482.png" alt="dataCenter-map-1920" width="315" />

**Summer 2014**

<img src="images/My-home-data-center/detail-datacenter-2014-07-31x1080-1.png" alt="detail-datacenter-2014.07.31x1080" width="800" />

# 4 – Electricity

In no way, it would be possible to supply this kind of system without attention to electricity. My first step was to install an electrical box to provide a power of  240 volts at 40 amp, which will provide a maximum of 9.6kw/hr when needed. Note: The servers are rarely open all at once, average consumption is 1-2 kw / hr. The data center electrical box is connected to the main electrical box using an 8-3 CU wire. In addition, UPS are installed behind the rack No. 1. Electricity distribution to the servers is controlled by a Sentry power tower using a web interface.  

<img src="images/My-home-data-center/dsc00034.jpg" alt="DSC00034" width="315" />

<img src="images/My-home-data-center/dsc00040.jpg" alt="DSC00040" width="315" />

<img src="images/My-home-data-center/dsc00045.jpg" alt="DSC00045" width="315" />

 Sentry power tower:

<img src="images/My-home-data-center/srv-ptxlhf16-2-06.jpg" alt="srv-ptxlhf16-2-06" width="276" />

 Sentry power tower web interface:

<img src="images/My-home-data-center/sentry-01-768x579.png" alt="sentry 01" width="315" />

# 5 – vCenter server

Most of my i86 servers run VMware vSphere Hypervisor.  Centralize management is obtained through vCenter server. vCenter server is running in an auto start ‘windows server 2008R2 VM’ on the HP DL380 G5.   For additional reliability, the 380 is connected to a UPS. Here’s a short video screen capture of me parsing my vCenter resources with a vSphere Client.

▶️ Vidéo YouTube : [Voir la vidéo](https://www.youtube.com/watch?v=eyDaw7eduZ8)

## 5.1 The map

<img src="images/My-home-data-center/capture-d_c3a9cran-2014-01-30-c3a0-21-11-34-768x480.png" alt="vCenter - map" width="315" />

## 5.2 Novell server

<img src="images/My-home-data-center/novell-server-768x504.png" alt="novell server" width="315" />

## 5.3 Novell client – under windows 8.1

<img src="images/My-home-data-center/capture-d_c3a9cran-2014-01-30-c3a0-20-09-37-768x585.png" alt="Capture d’écran 2014-01-30 à 20.09.37" width="315" />

<img src="images/My-home-data-center/novell-client-win8-1-768x505.png" alt="novell client win8.1" width="315" />

## 5.4 Novell iManager

<img src="images/My-home-data-center/novell-annuaire-768x850.png" alt="Novell - Annuaire" width="315" />

<img src="images/My-home-data-center/novell-arborescence.png" alt="Novell - Arborescence" width="315" />

# 6 – Hardware and topology

Currently being written…

1. [Network diagram](#a61)
2. [Network switches and routers (including fibre channel)](#a62)
3. [Servers](#a63)

### 6.1 – Network diagram

<img src="images/My-home-data-center/schema-reseau-maison-768x476.png" alt="schema reseau maison" width="315" />

### 6.2 – Network switches and routers (including fibre channel)

#### – Hp 352 fibre channel switch

The 3 SAN of the data center are interconnected with a HP 352  fibre channel switch.

<img src="images/My-home-data-center/hp-352-fibre.jpg" alt="hp-352-fibre" width="300" />

 The switch configuration is achieved trough a WEB interface:

<img src="images/My-home-data-center/fibre-channel-switch1.png" alt="fibre channel switch" width="315" />

 And here’s a few volumes resynching in Windows server 2012 running on a PowerEdge 2850 II with 2 x 2Gb F.C. controller.

<img src="images/My-home-data-center/raid-5-resynching-768x608.png" alt="RAID 5 - RESYNCHING" width="315" />

### 6.3 – Servers

**Intelligent Platform Management Interface** ([**IPMI**](http://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface)) Most of my servers have an IPMI card (Dell DRAC, HP iLO, …) for remote monitoring and control. **iLO2 on my HP.DL380-G5 (ESXi, Asterisk, …)**

<img src="images/My-home-data-center/hp-dl380-ilo-02-768x469.png" alt="HP-DL380-iLO-02" width="315" />

<img src="images/My-home-data-center/hp-dl380-ilo-01-768x615.png" alt="HP-DL380-iLO-01" width="315" />

 **DRAC5 on my PowerEdge 2950.II.09 (pfSense server):**

<img src="images/My-home-data-center/pe2950-09-drac-768x884.png" alt="PE2950-09-DRAC" width="315" />

 Adding memory in the DL585 G5 and DL580 G5

<img src="images/My-home-data-center/i01.jpg" alt="i01" width="315" />

<img src="images/My-home-data-center/i03.jpg" alt="i03" width="315" />

<img src="images/My-home-data-center/i02.jpg" alt="i02" width="315" />

 more to come …

### Storage and NAS

**Data domain DD500** – Running FreeNAS. 4 x 2TB Raid5 + 11 x 500GB Raid6 = 10.5TB of storage   

<img src="images/My-home-data-center/dd580.jpg" alt="dd580" width="306" />

# 7 – Home VOIP services

What a pleasure to be able to speak, by phone, to all our friends and acquaintances, regardless of their geographical location without breaking the bank. That is why we decided to move to IP telephony. Our VOIP home network consists of 5 Avaya 1230 phones (reflash with SIP firmware), a Power Over Ethernet switch, a AsteriskNOW server – VM machine on DL380 – with the FreePBX administrative GUI and an external  SIP/ IAX2 trunk provider (voip.ms).

### 7.1 – Hardware

- Access to the Internet
- A POE Switch – Dell powerConnect 3448P

<img src="images/My-home-data-center/img_pro_1302_3_a.jpg" alt="img_pro_1302_3_a" width="250" />

- 5 Avaya 1230 ip Phones

<img src="images/My-home-data-center/avaya-1230.jpg" alt="Avaya-1230" width="189" />

- 1 HP DL380G5 – running a VM of asteriskNow

<img src="images/My-home-data-center/dl380g5-768x163.jpg" alt="dl380g5" width="315" />

### 7.2 – Software

- [Asterisknow](http://www.asterisk.org/downloads/asterisknow)
- Management with a GUI:

<img src="images/My-home-data-center/asterisk-768x536.png" alt="Asterisk" width="315" />

### 7.3  – An  SIP/IAX2 trunk provider

- [voip.ms](https://www.voip.ms) ( great prices! )

<img src="images/My-home-data-center/capture-d_c3a9cran-2014-01-31-c3a0-13-24-10-768x768.png" alt="Capture d’écran 2014-01-31 à 13.24.10" width="189" />

- A little effort of configuration

And Voilà, it’s up and running!

# 8 – BladeCenter

This one is quite a beast; noisy and power hungry!  It runs on 240 volts only. There are 11 LS20 (2 \* dual core AMD CPU) and 3 HS20 (2 \* dual core intel CPU) blade in the bladeCenter. Most of the blades are running VMware vSphere Hypervisor.

<img src="images/My-home-data-center/800px-ibm_bladecenter_front-768x576.jpg" alt="800px-IBM_bladecenter_(front)" width="315" />

 Blades configuration is achieved trough a WEB interface:

<img src="images/My-home-data-center/bladecenter-leds-768x705.png" alt="bladeCenter-Leds" width="315" />

<img src="images/My-home-data-center/bladecenter-power-768x879.png" alt="bladeCenter-power" width="315" />

<img src="images/My-home-data-center/bladecenter-status-768x768.png" alt="bladeCenter-status" width="315" />

 Configuration of  the layer 2-3 GbE integrated switch  is also achieved trough a WEB interface:

<img src="images/My-home-data-center/bladecenter-switch-768x838.png" alt="bladeCenter-Switch" width="315" />

# Reference manuals

[P](fichiers/netscreen-ug_500.pdf)[owerEdge-2950ii_owner’s manual_en-us](fichiers/poweredge-2950_owners-manual_en-us.pdf) [MR2324.4C Management Guide](fichiers/mr2324-4c_management_guide.pdf) [Juniper Netscreen 500](fichiers/ug_500.pdf) [Sentry PTXL v52](fichiers/sentry_ptxl_v52-modele-du-home-datacenter.pdf) [HP DL580 G5](fichiers/dl580_g5.pdf) [HP-DL585 G5 – User manual](fichiers/hp-dl585g5-user-manual.pdf) [HP Integrity RX2620](fichiers/hp-integrity-rx2620.pdf) [HP 9000 rp3440 user guide](fichiers/hp-9000-rp3440-user-guide.pdf) [Power connect 3448P User’s Guide](fichiers/powerconnect-3448_users-guide_en-us.pdf) [SUN sunfire t2000 admin guide](fichiers/sun-sunfire-t2000-admin-guide-819-2546-11.pdf) [SUN sunfire t2000 install guide](fichiers/sun-sunfire-t2000-install-guide-819-2546-11.pdf)
