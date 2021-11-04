---
layout: default
title: 3CX Deploy on Azure 
nav_order: 2
nav_exclude: false
has_children: false
parent: Configuration
---
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



