---
layout: default
title: Microsoft LAPS (Local Administrator Password Security)
nav_order: 1
nav_exclude: false
has_children: false
parent: Configuration
---
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

## REFERENCES
[INSTALLATION LAPS](https://www.veeam.com/blog/microsoft-laps-deployment-configuration-troubleshoot-guide.html)
