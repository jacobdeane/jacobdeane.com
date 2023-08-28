---
publishDate: 2019-06-20T20:07:39.696Z
title: Home Networks for IOT
excerpt: The Internet of Things (IOT) is here; its not secure, lacks long-term support and is going to steal your data.
image: ~/assets/images/home-networks-for-iot/network.jpg
category:  technology
tags:
  - iot
  - smart-home
  - technology
---
Just a decade ago, before the iPhone, you probably had only a couple of internet connected devices in your home and they were probably both computers.

Fast forward to today, for almost any electronic device you can imagine there is an internet connected version; TVs, thermostats, smoke detectors, cameras, lights, fridges, washing machines, games consoles, phones, tablets, speakers, watches, doorbells, coffee machines, kettles, air quality monitors, toys, smart locks & voice assistants. If you can imagine a device, it probably already exists. These devices are known as the Internet of Things (IOT).

Whilst having a tonne of smart devices can enrich your life, we are also opening our networks to any vulnerabilities these devices might have, often through a lack of proper support for updates to patch known security vulnerabilities. We can’t force all manufacturers to make perfectly secure devices – such a thing doesn’t exist, but there are some things we can do to mitigate the risks by reducing network access to devices that don’t need it.

### But I have a firewall?

When the internet was first created, there wasn’t much call for security – only universitys and military bases were using the internet, but soon some sort of protection from the outside world was needed. In the mid 90s firewalls were developed to restrict access to your network from the internet. Firewalls are good at keeping threats out of your network, but what if the threat is already inside your network, lurking in one of your IOT devices?

We should make the assumption that all devices are vulnerable and that all devices will be compromised, this is of course a worst case scenario, but its always best to hope for the best, plan for the worst.

> Hope for the best, plan for the worst.
<cite>Jack Reacher (Lee Child)</cite>

If we make the assumption that our IOT devices pose a serious threat to other devices on our network, then our best defence is to seperate them from sensative data such as that contained on our PCs and smartphones.

### Network Segregation

One way to segregate IOT devices would be to build an entirely seperate network, using sperate wifi, routers, modems, cables etc – but this is hardly practical in a home or even most business environments, unless you are the NSA or GCHQ…

The answer is to use a technology widley used in the enterprise world and that is network virtualisation. Virtual networks or VLANs allow multiple networks to run on the same hardware; a single set of physical network switches and access points can run multiple virtual networks, each seperated from each other with a firewall.

In enterprise environments, VLANs can be used to seperate departments and keep the flow of data controlled and managed. We can use VLANs at home to seperate our IOT devices from our trusted devices that contain our personal data. Thus if an IOT device is compromised, its access to our sensitive data is controlled or blocked entirely by a firewall.

### Hardware Requirements

Unfortunately there is some bad news, your average free ISP supplied modem / router / wifi combo box isn’t going to cut it – even your high end gaming router from the likes of Netgear, you know the ones that have eight antennas and look like an angry spider, they won’t do either.

What you need is some enterprise level gear – but it might be cheaper than you think.

In the world of enterprise networking, multi-function devices don’t really exist – instead of a single device like the one your ISP supplied, you have seperate modem, firewall/router, switches and access-points. This means more devices, but it also means greater flexibility, capabilty and reliability.

A basic setup might consist of the following:

* 1 x Modem – could be your ISP supplied modem in bridge mode, if you live in the UK, a great alternative is to use a BT openreach modem which can be picked up on eBay for around £20 – exact requirements will depend on your internet connection.
* 1 x Router / Firewall – lots of options here, a Ubiquiti EdgeRouter X is increadible value at around £40, or the more user friendly Ubiquiti UniFi Security Gateway for around £100. All sorts of enterprise routers can be found on eBay from £30 upwards – Juniper, Xyzel, Cisco are a few brands to look for.
* 1 x Managed Gigabit Network Switch – the managed part is what allows us to use VLANs, the switch will allow us to connect any devices that need ethernet. Gigabit is fast enough for most home requirements. Managed Gigabit Network Switches from Netgear, HP, Cisco can be found on eBay for around £50, depending on the number of ports you require.
* 1 x Wireless Access point – probably go for one that at least supports AC wireless. I’m a big fan of UniFi kit, a UniFi AC lite is about £75 new.

Nearly all network equipment works on IEEE standards which means you should be able to mix and match brands without any issues, but some brands offer a single control panel to manage your entire network which can be very convenient, but not essential.

My home network is almost all running on Ubiquiti’s UniFi system and is made up of the following equipment:

* 1 x BT Openreach Huawei HG612 Modem, running custom firmware to support G.INP (which allows faster internet speeds) and to allow more advanced diagnostics over its second ethernet port (<https://kitz.co.uk/routers/hg612unlock.htm>)
* 1 x Ubiquiti USG router / firewall
* 1 x Ubiquiti UniFi CloudKey, a small device that runs the UniFi management software to control the network – this software can be run on a Raspberry Pi or a NAS instead if you prefer
* 1 x Ubiquiti UniFi Switch 24
* 1 x Ubiquiti UniFi Switch 16-150W, a Power Over Ethernet (POE) switch that can provide up to 15W of power to attached devices over a single ethernet cable. I power a couple of network cameras, my wireless access points, the UniFi cloudkey and a Raspberry Pi
* 1 x Ubiquiti UniFi AP AC Pro Access Point – this easily covers my whole house
* 1 x Ubiquiti UniFi AC Mesh Access Point – this provides WiFi for my back garden so I can connect my astronomy gear to my home network really easily

Total cost was around £850, which might seem a bit extravegant, but quite a large chunk of that is the POE switch, which I only wanted to make my cabling neater. Ubiquiti makes high end equipment for a pretty reasonable price and you can manage and importantly update all the devices from a single place; security updates are important, especially for your network kit as its likely the first point of attack. Have you ever updated the firmware on your router? Another thing to note is I’ve been running most of this setup for about 4 years and I have never needed to reset it, except for fairly regular firmware updates – it has worked flawlessly 100% of the time – can you say that about your own router?

If you went the eBay route, you could probably pick up a good base setup for around £200. A high end consumer / gaming all-in-one modem / router / access point can easily cost £200 – and you will get a much more reliable setup using enterprise kit.

### Setting up VLANs

As your exact network setup will almost certainly differ from mine, I won’t go into too much detail about exactly how to setup a UniFi based network, instead I shall outline the principles of how to set up VLANs to secure your network.

The basic principle is that devices on a LAN can all talk to each other, to make our network more secure we want to segregate devices onto separate virtual LANs, we will then add some firewall rules to control the traffic between them.

To keep things simple, we will setup 3 VLANs, one for your trusted devices (PCs, laptops, printers, smartphones), one for your IOT devices (media devices, speakers, TVs, lights etc) and finally, one for your guest WiFi – more on that later.

You can of course have almost as many VLANs as you want, but most WiFi access points will have a limited number of networks that they can create, and you’ll need a separate WiFi network for each VLAN that will have wireless devices on it as the only way to properly segregate WiFi traffic is to have separate WiFi networks.

Each VLAN will have its own subnet, a separate IP pool, which allows for a much larger number of devices on your network; it also makes it easier to keep the traffic separate as we can apply firewall rules to specific subsets. Typically VLANs can be numbered from 1 to 4094, but to keep things easy to remember, I have used VLAN 10, 20 & 30 and the set IP address ranges of each network match the VLAN number. (10.0.10.XX, 10.0.20.XX & 10.0.30.XX)

Each of my Networks are corporate, except the guest network. By default, in UniFi, corporate networks can communicate freely with each other, so we will add some firewall rules later to restrict that. Other brands may do things differently, so make sure you read the manual for your router / firewall and switches.

VLANs work by adding a tag to the start of the packets that contain the data going across the network – then you can add a tag to specific ethernet ports on your switch and to the WiFi networks so that only traffic tagged with the same VLAN as the port is allowed through. Some devices including switches, WiFi access points and firewall will need to be able to talk to all VLANs, to achieve this we make sure their switch ports are set up as trunk ports. In UniFi you set a trunk port by changing the port to ‘All Networks’.

In summary, we created 3 new networks, assigned them VLANs, created corresponding WiFi networks assigned to the same VLANs and we set each port on our switches to the correct VLAN for the device connected to it. That is pretty much it for VLANs, all we need to do is to add some firewall rules to restrict traffic between the VLANs.

### Firewall Rules

As mentioned earlier, currently devices on different VLANs are free to communicate with each other, there are no restrictions in place, at least on the UniFi system, other brands may vary. We need to add some firewall rules.

Most firewalls work in a similar fashion, rules are read from the top down, so the first rule overrides any later rules in the list. This means we can add any exceptions we want, and then add a rule to block all other traffic.

The rules we want to add will go under LAN In, as we will apply the rules on traffic coming into the firewall from the LAN. This can be a little confusing, the trick is to remember to look at it from the point of view of the firewall itself.

My first rule allows established connections from the IOT VLAN to the trusted VLAN – this means if a connection has been established already, then the IOT device is free to continue communicating, but is unable to create new connections. This is vital to allow IOT devices to be reached from the trusted VLAN, but to block the IOT devices from making connections on their own. IOT devices can reply, but they cannot initiate communication.

My second rule drops all other traffic from my IP cameras – I have 4 cheap Chinese network IP cameras that I use for CCTV, but I don’t trust them at all and I don’t want video of my house to be streamed across the internet, so these devices have all their traffic dropped unless they are replying to a connection from my PC to manage them, or from the NAS to record footage.

I have a rule to allow my local DNS server to be used on all the VLANs. My DNS server, allows me to use domains such as apple tv.home instead of having to remember the IP address; it also runs Pi-Hole, a network level ad-blocker which blocks connections to known advert servers.

I have a couple of firewall rules that allow specific ports on the SONOS and Apple TV to allow them to work across the VLANs. To get SONOS to work across a VLAN is actually a little more tricky – you will also need to set up an IGMP proxy, to move multicast traffic across the VLANs.

Finally I have a rule that drops all other traffic from my IOT VLAN to my other VLANs. IOT devices can still access the internet, but not anything on my trusted VLAN.

### Guest Networks

I mentioned earlier that I also have a guest network set up. This isn’t strictly necessary, but if you are going to the effort of separating your trusted devices from the rest of your network, it also makes sense to separate you guests too. On UniFi, it’s really simple to set up a guest network, you can just create a network and set it to be a ‘Guest network’, I also applied a VLAN to it to keep things tidy.

By default on UniFi, devices connected to a guest network are automatically firewalled from the rest of the network and isolated from other devices on the guest network – the guest network provides internet access and nothing else. I also set up a rule to restrict the bandwidth available to guests to half my full internet speed – so a guest can’t take all my internet! Not really needed at all, but a cool feature I thought would be fun to play with.

### IOT on Lockdown

The Internet of Things may be unsecure, and it might be an easy target for a nefarious hacker, but with a properly designed network we can massively reduce the impact of what a compromised device can do. We probably can’t make IOT secure, but we can stop it from stealing your data.