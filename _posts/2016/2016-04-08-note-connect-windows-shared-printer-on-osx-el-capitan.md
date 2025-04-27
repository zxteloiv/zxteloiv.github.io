---
layout: post
title:  "Connect to a printer shared by Windows server on OS X El Capitan"
date:   2016-04-08 18:50 +0800
tags: mac osx printer el_capitan windows
categories: note
---

I use a macbook behind a router connected to the organization network. My printer is connected to and shared by a Windows PC. I know the IP address of that PC and the printer name shared by that PC. And that printer is not directly connected to ethernet and doesn’t have an IP address.

## 1. download driver

If Mac don’t have appropriate driver for your printer, you need to download one.

In my case, I have HP LaserJet 400 M401 PCL 6 and I download it from http://printerdriverseries.com/hp-laserjet-pro-400-driver-download/ (where the link provided is also from HP official site)

I use OSX 10.11 but I downloaded 10.10 driver because of the lack of HP driver solution but only a setup guide application for 10.11, which will find your printer and download the driver for you.

However, I couldn’t connect to that PC using IP address because it is shared by a Windows, and that HP Setup Application doesn’t help.

But I need the driver really, so I downloaded it from other sources.

The driver app is a pkg (in a dmg) and installs driver files.

## 2. connect

Then I open **System Preference - Printers & Scaners**, click the + sign on the bottom of the right list box.

In the opened dialogue, if you don’t have an **Advanced** icon on the tool bar, right click on that toolbar and click on **Customize Toolbar...**, then drag the **Advanced** icon out.

Under the Advanced page, choose the (connection protocol) **Type** to **Windows printer via spoolss**.

URL is : `smb://ip_address/somename(sub_space_using_%20)`,

> for example, the IP address for that Windows server is 172.16.1.123 and the shared name of the printer is “My Printer” (without quotation).  
> URL is smb://172.16.1.123/My%20Printer

The **Name** below could be anything you’d like to name the printer to add to your mac, and the printer driver should **Use** the very one you just downloaded and installed.

