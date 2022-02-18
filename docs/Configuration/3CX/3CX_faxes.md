---
layout: default
title: 3CX faxes
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
   
# 3CX Configure faxes  
## Inbound faxes
For turn on fax server, go to 3CX WEB UI -> Advanced -> Fax Server  
![](images/3CX_FAXES_01.png)

Type Default Email Address for recive faxes, Enable G.711 to T.38 Fallback, for save press OK button     
![](images/3CX_FAXES_02.png)

Go to SIP Trunks, open AWS sip trunk, on DIDs tab add additional DID  
![](images/3CX_FAXES_03.png)

Go to Inbound Routes, Add new DID rule, type Name: Fax Rule, select DID for assign.  
Set Route calls to Send fax to -> users who will recived fax to PDF    

![](images/3CX_FAXES_04.png)  

## Outbound fax
For outboud faxes, need use ATA devices with fax machine.  
Or use software for SipToFax, like:  
http://www.t38faxvoip.com/  