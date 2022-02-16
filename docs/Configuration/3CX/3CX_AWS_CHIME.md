---
layout: default
title: 3CX AWS Chime setup
nav_order: 2
nav_exclude: false
has_children: false
parent: 3CX Phone system
grand_parent: Configuration
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# 3CX and AWS Chaim integration
## Configuration AWS SIP Connector on 3CX

### Buy new PSTN number
Go to Amazon Chaim console use URL: https://console.chime.aws.amazon.com/  
![](images/3CX_aws_chime_01.png)  
Go to Phone number management - Orders tab, press "Provision phone numbers" button  
![](images/3CX_aws_chime_02.png)  
On Provision phone numbers windows select "Business Calling" and press Next button  
![](images/3CX_aws_chime_03.png)  
Select phone number which you need and press "Provision" button  
![](images/3CX_aws_chime_05.png)
Switch to the "Invetory" tab and you can see your selected phone number

For set calling name, select PSTN number and press Actions -> Update default calling name, set name and save. Information will be updated after 7 days.   
![](images/3CX_aws_chime_24.png)


### Create Voice connector for 3CX
Go to Voice connectors and create new one  
![](images/3CX_aws_chime_07.png)  
Fill Voice connector name and select AWS region, Encryption mode switch to Disabled  
![](images/3CX_aws_chime_08.png)  
Open created connector and go to Termination tab for configure outbound calls, swith Termination status to enabled    
![](images/3CX_aws_chime_09.png)
Allowed host list need to be add 3CX public IP  
![](images/3CX_aws_chime_10.png)  
In section Credentials (Recommended) need to be assigned login and password for SIP TRUNK    
![](images/3CX_aws_chime_11.png)  
![](images/3CX_aws_chime_12.png)  
Setup Calling plan, select countries where users can make outgoing calls and press save  
![](images/3CX_aws_chime_13.png)  
Go to Origination tab for configure inbound routes, switch Origination status to Enabled.  
In Inbound routes section press "New" button and fill fields:   
* Host: <3CX Public IP>, Port: 5060, Protocol: TCP, Priority: 1,  Weight:5 and press Save  
![](images/3CX_aws_chime_14.png)  
Next step need assign phone number to created voice connector. On "Phone numbers" tab press Assign from inventory button and select PSTN number witch you want assign.   
![](images/3CX_aws_chime_15.png)
## Configuration AWS SIP Connector on 3CX
Open 3CX web ui admin panel and go to SIP Trunks, press "Add SIP Trunk" need select "Amazone Chime Voice Connector"  
![](images/3CX_aws_chime_16.png)   
Fill important fields:
> Name of Trunk;   
> Registat/Server/IP: <On AWS Chime console in voice connector Termination tab: Outbound host name>;  
> Type of Authentication: Do not require - IP Based;  
> Authentication ID: created username on AWS Chime voice connector;  
> Authentication Password: created password on AWS Chime voice connector;

![](images/3CX_aws_chime_17.png)
> Main Trunk No: type PSTN phone number start from +1;  
> Set destination for calls during office hours;   
> Set destination for calls outside office hours;

![](images/3CX_aws_chime_18.png)   
On DIDs tab, add Single DID (your PSTN number)   
![](images/3CX_aws_chime_19.png)

On Options tab:
> Transport protocol set: UDP  
> Move up codec G729

![](images/3CX_aws_chime_20.png)  
![](images/3CX_aws_chime_19_a.png)  
![](images/3CX_aws_chime_21.png)

Press Ok button for save changes.

Go to Outbound Rules:  Press Add button for create new rule:
> Rule Name:   
> Calls to number starting: +1  
> Calls to Numbers with a length of: 12  
> Route 1: select Amazone Chime Voice Connector, Strip Digits: 0

![](images/3CX_aws_chime_22.png)  
![](images/3CX_aws_chime_23.png)  

# 3CX and AWS Chaim faxes 
## Inbound faxes 
For turn on fax server, go to 3CX WEB UI -> Advanced -> Fax Server  
![](images/3CX_FAXES_01.png)  
  
Type Default Email Address for recive faxes, Enable G.711 to T.38 Fallback, for save press OK button     
![](images/3CX_FAXES_02.png)  
  
Go to SIP Trunks, open AWS sip trunk, on DIDs tab add additional DID  
![](images/3CX_FAXES_03.png)  

Go to Inbound Routes, Add new DID rule, type Name: Fax Rule, select DID for assign.  
Set Route calls to Send fax to -> <users who will recived fax to PDF>  
![](images/3CX_FAXES_04.png)  

## Outbound fax 
For outboud faxes, need use ATA devices with fax machine.  
Or use software for SipToFax, like:  
http://www.t38faxvoip.com/  

## References 
https://www.3cx.com/docs/amazon-chime-voice-connector-sip-trunk/  
https://docs.aws.amazon.com/chime/latest/ag/network-config.html#bandwidth   
