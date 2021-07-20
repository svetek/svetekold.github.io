---
layout: default
title: Install Microsoft Remote Desktop (RDP Client)
nav_order: 1
nav_exclude: false
has_children: false
parent: Windows Virtual Desktop
grand_parent: User Guides
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Install Microsoft Remote Desktop Windows 10 (RDP Client) from msi package (Recommended!!!)
Download MSI on your PC use that URL:   
[Windows 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)    
[Windows 32-bit](https://go.microsoft.com/fwlink/?linkid=2098960)   
[Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)  
  
When downloading is done, run installation MSI double click on file:   
![](../images/rdclient_install_from_msi_01.png)  
Press RUN button  
![](../images/rdclient_install_from_msi_02.png)  
On Welcome installation windows press Next  
![](../images/rdclient_install_from_msi_03.png)  
Click on checkbox "I accept ...." and press Next  
![](../images/rdclient_install_from_msi_04.png)  
Click on "Install for all users" and press Next  
![](../images/rdclient_install_from_msi_05.png)  
Press Yes  
![](../images/rdclient_install_from_msi_06.png) 
Installation is done. Press Finish for close window and run app.    
![](../images/rdclient_install_from_msi_07.png)  

![](../images/step_07.png)     
  
Installation is done.  

# Install Microsoft Remote Desktop Windows 10 (RDP Client) from Microsoft store
If you use Windows 10 run Microsoft Store just type in search "store" and press on Microsoft Store  
![](../images/step_01.png)     
On new windows Microsoft Store in search input type "Remote Desktop" and select "Microsoft Remote Desktop"  
![](../images/step_02.png)    
Next step we need press "Get" button for for start downloading app   
![](../images/step_03.png)   
On popup window "Use across your devices" press "No, thanks" button  
![](../images/step_04.png)  
You will see starting process download and Install app  
![](../images/step_05.png)  
When installation process done, you will press "Launch" button  
![](../images/step_06.png)  
![](../images/step_07.png)     
  
Installation is done.  Go to "ADD WORKSPACE" for configuration Microsoft Remote Desktop.  


# ADD WORKSPACE ACCESS 
Now you need add User Workspace. Press "ADD" -> "Workspaces"  
![](../images/step_08.png)  
Email or Workspace URL type: https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery  and press "Subscribe button"  
![](../images/step_09.png)    
Type your work username / password  
![](../images/step_10.png)   
Press "Work or school account"    
![](../images/step_11.png)  
Done, now you can try connect to the SessionDesktop or WVD APPs.  
![](../images/step_12.png)   

# Configure redirection folders  
## OS X  
Open Microsoft Remote Desktop app and got to "Preferences...".  
![](../images/WVD_OSX_CLIENT_01.png)   

Open DropDown "if folder redirection is enabled for RDP. ..." and select "Choose folder".  
![](../images/WVD_OSX_CLIENT_02.png)   

Select local folder for redirect to WVD session host.  
![](../images/WVD_OSX_CLIENT_03.png)   

Try connect and check redirection folder  
![](../images/WVD_OSX_CLIENT_04.png)   

