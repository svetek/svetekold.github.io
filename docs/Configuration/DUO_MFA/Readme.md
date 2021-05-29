---
layout: default
title: DUO MFA integration with Azure Active Directory
nav_order: 2
nav_exclude: false
has_children: false
parent: Configuration
---

# DUO protect Microsoft Azure Active Directory
see https://duo.com/docs/azure-ca 

Login https://duosecurity.com 
and go to Application - Protect an Application   
![](images/DUOMFA_1.png)  

In search field type Azure Active Directory.  
And press Protect button on Microsoft Azure Active Directory line.   
![](images/DUOMFA_2.png)  

Next step press Autorize button  
![](images/DUOMFA_3.png)  
  
Next step need type Azure AD Admin auth credentials  
![](images/DUOMFA_4.png)  

Press Accept on this step  
![](images/DUOMFA_5.png)  

That step can be if you try auth not Azure Administrator  
![](images/DUOMFA_6.png)  

Use MFA if that enabled on your configuration  
![](images/DUOMFA_7.png)  

On this step need copy JSON on "Custom control" and press "Save" on the end page
![](images/DUOMFA_8.png)  
![](images/DUOMFA_9.png)  
![](images/DUOMFA_10.png)  
![](images/DUOMFA_11.png)  
![](images/DUOMFA_12.png)  
![](images/DUOMFA_13.png)  
![](images/DUOMFA_14.png)  
  
Go to portal Azure AD, click on "Security" menu  
![](images/DUOMFA_15.png)   
  
Go to "Conditional Access"  
![](images/DUOMFA_16.png)  

Go to Custom control (Preview)  
![](images/DUOMFA_17.png)  

Create New custom control     
![](images/DUOMFA_18.png)  
 
Give name "RequiredDuuoMfa", paste JSON when we copy from DUO page. and press "Save"   
![](images/DUOMFA_19.png)  

Go to Azure AD, security, Conditional Access, Policies and press "New policy"  
![](images/DUOMFA_20.png)  

Type name Require Duo MFA, but first need create security group with name "DUO Users"  
In Cloud apps or actions need select All cloud apps or Specified cloud apps for DOU MFA AUTH
In Grant select "Grant access" and enable checkbox "RequireDuoMfa"   
![](images/DUOMFA_23.png)  
![](images/DUOMFA_22.png)  
![](images/DUOMFA_24.png)  

Enable policy set "ON", if you get error with setup to on. 
Need go to Azure AD - Properties - Manage Security defaults and 
Enable Security defaults switch to "No"  
to be continued ....!!!