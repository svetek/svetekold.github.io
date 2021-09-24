---
layout: default
title: Microsoft LAPS (Local Administrator Password Security)
nav_order: 1
nav_exclude: false
has_children: false
parent: Configuration
---
## Table of contents
{: .no_toc .text-delta }

1. TOC 
{:toc}
# Microsoft LAPS (Local Administrator Password Security)

## Download and Install LAPS on Active Directory

Download [LAPS](https://www.microsoft.com/en-us/download/details.aspx?id=46899)    
  
Install LAPS on Domain Controler:
![](images/LAPS_01.png)  
![](images/LAPS_02.png)  
  
Create two security group, one for readers (LAPS-readers) and one for writers (LAPS-reset):  
![](images/LAPS_03.png)  

Copy path organisation unit  
![](images/LAPS_04.png)

Run powershell script (Replace organisation unit path):  
```
Import-module AdmPwd.PS
Update-AdmPwdADSchema

Set-AdmPwdComputerSelfPermission -OrgUnit "CN=Computers,DC=churchoregon,DC=local"
Set-AdmPwdReadPasswordPermission -OrgUnit "CN=Computers,DC=churchoregon,DC=local" -Allowedprincipals "LAPS-readers"

Set-AdmPwdReadPasswordPermission -OrgUnit "CN=Computers,DC=churchoregon,DC=local" -Allowedprincipals "LAPS-reset"
Set-AdmPwdResetPasswordPermission -OrgUnit "CN=Computers,DC=churchoregon,DC=local" -Allowedprincipals "LAPS-reset" 
```  
Open Group Policy Managment:  
![](images/LAPS_05.png)  
  
Create new GPO container with LAPS-PWD:    
![](images/LAPS_06.png)   
![](images/LAPS_07.png)  
![](images/LAPS_08.png)  
Edit new GRO LAPS  
  
![](images/LAPS_09.png)  
  
Open Computer Configuration -> Polices -> Administrative Template -> LAPS    
![](images/LAPS_10.png)  
  
Edit "Password Settings" switch to Enabled  
![](images/LAPS_11.png)  
  
Edit "Enable local admin password managment" switch to Enabled   
![](images/LAPS_12.png)  
 
## Install LAPS on PC 
  
Need install LAPS on PC, or setup installation use GPO

## How to read password and make reset  
  
Run LAPS UI on DC. Enter Computer name and press "Search".
  
![](images/LAPS_13.png)    

## How to install LAPS on all PC in domain  
### Create share for apps
Open Server Manager / File and Storage Services -> Shares  
Task -> New Share for create  
![](images/LAPS_14.png)  

Select SMB Share - Quick and press Next   
![](images/LAPS_15.png)  

Type name GPO-APPS ad press Next  
![](images/LAPS_16.png)  

Press Next  
![](images/LAPS_17.png)  

Press Next  
![](images/LAPS_18.png)  

### Setup LAPS package use GPO on all Computer in domain  
Open GPO LAPS and go to Computer -> Polices -> Software Settings -> Software Installation, right mouse click select New -> Package  
![](images/LAPS_19.png)  
Select network patch apps like on screenshot    
![](images/LAPS_20.png)  
Select "Assigned" mode and press OK  
![](images/LAPS_21.png)  

run gpupdate command in powershell console on DC  

### Check ExtendedRights permissions on OU
To get information on the groups and users able to read the password (ms-MCS-AdmPwd) for a specific Organizational Unit (OU), run the following command.  
```
Find-AdmPwdExtendedRights -identity "CN=Computers,DC=vixon,DC=local" | Format-Table ExtendedRightHolders
```

### Set all passwords
```
Get-ADComputer -Filter * -SearchBase “CN=Computers,DC=vixon,DC=local” | Reset-AdmPwdPassword -ComputerName {$_.Name}
```
### List all passwords 
```
Get-ADComputer -Filter * -SearchBase "CN=Computers,DC=vixon,DC=local" | Get-AdmPwdPassword -ComputerName {$_.Name}
```

## REFERENCES
[INSTALLATION LAPS](https://www.veeam.com/blog/microsoft-laps-deployment-configuration-troubleshoot-guide.html)
