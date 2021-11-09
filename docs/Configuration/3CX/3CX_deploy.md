---
layout: default
title: 3CX Deploy on Azure 
nav_order: 2
nav_exclude: false
has_children: false
parent: Configuration
---
## Table of contents
{: .no_toc .text-delta }

1. TOC 
{:toc}
# 3CX Deploy on Azure

## Create VM 3CX Phone System on Microsoft Azure Portal  
Open portal.azure.com, in search box type 3CX and create 3CX Phone System.  
You will forward to wizard Create VM.
  
![](images/3CX_deploy_azure_01.png)
  
When VM is created, you need take public IP adress and open URL on browser on your PC like:   
http://<public IP VM>:5015
You will see open wizard page 3CX.  
  
![](images/3CX_deploy_azure_02.png)
![](images/3CX_deploy_azure_03.png)

On 3CX customer portal take subscription number for registration new instance and paste to the http://<public IP VM>:5015 and press Next   

![](images/3CX_deploy_azure_04.png)  
  
Type Username and Password for manage new instances   
  
![](images/3CX_deploy_azure_05.png)
  
Confirm public IP address  
  
![](images/3CX_deploy_azure_06.png)
  
Select Static or Public type IP address on instance  
  
![](images/3CX_deploy_azure_07.png)
  
Type FQDN or select No option for 3CX will generate custom FQDN for your instances  
  
![](images/3CX_deploy_azure_08.png)
  
Type admin email address for get some notification   
  
![](images/3CX_deploy_azure_09.png)
  
Set up Country and timezone for instance   
  
![](images/3CX_deploy_azure_10.png)
  
Internal Extension type, and setup first extension  
  
![](images/3CX_deploy_azure_11.png)
  
Select Coutries that calls can be made to  
  
![](images/3CX_deploy_azure_12.png)
  
Select Language interface  

![](images/3CX_deploy_azure_13.png)
  
SetUP is Done, you can login to web instance page   

![](images/3CX_deploy_azure_14.png)
![](images/3CX_deploy_azure_15.png)

For registration instance on customer portal 3CX, you will go to instance web page -> Settings -> Instance Manager  

![](images/3CX_deploy_azure_17.png)
  
Select all checkboxes for provide access to custommer portal 3CX for manage them  
  
![](images/3CX_deploy_azure_18.png)
![](images/3CX_deploy_azure_19.png)
  
Now you can see instance on customer portal 3CX, done. 
  
![](images/3CX_deploy_azure_16.png)

## Open ports on Firewall VM 3CX Phone System 

For Azure need add 2 rules for open RTP trafic:  
1. UDP 9000-10999  
2. UDP 7000-8999  
  
Table with description 3CX ports
PROTOCOL | PORT (DEFAULT) | DESCRIPTION | PORT FORWARDING REQUIRED
TCP | 5001 or 443 | HTTPs port of Web Server. This port can be configured | Yes – if you intend on using a 3CX client, Bridge Presence, Remote IP Phones from outside your LAN and 3CX WebMeeting functionality
TCP | 5015 | This port is used for the online Web-Based installer wizard (NOT 3CX config command line tool) only during the installation process | Optional - During the installation process when the Web-Based installer is used from external source
UDP & TCP | 5060 | 3CX Phone System (SIP) | Yes – if you intend on using VoIP Providers and Remote Extensions that are NOT using the 3CX Tunnel Protocol / 3CX SBC
TCP | 5061 | 3CX Phone System (SecureSIP) TLS | Yes – if you intend on using Secure SIP remote extensions
UDP & TCP | 5090 | 3CX Tunnel Protocol Service Listener | Yes -if you intend on using remote extensions using the 3CX Tunnel Protocol (within the 3CX clients for Windows / Android / iOS) or when using the 3CX Session Border Controller
UDP | 9000-10999 7000-8999 | 3CX Media Server (RTP) – WAN audio/video/t38 streams  3CX Media Server (RTP) – LAN audio/video/t38 streams | Yes – if you intend on using remote extensions, WebRTC or a VoIP Provider  No - If you have strict routing on your LAN though, you must allow traffic from/to your 3CX server on there ports (Also applies to site-to-site VPNs)
TCP | 2528 | 3CX SMTP Server - Must allow PBX passthrough on the network for the PBX to send email notifications via the 3CX SMTP | No

Reference:  
  
https://www.3cx.com/docs/ports/  