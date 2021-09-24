---
layout: default
title: FSLogix User Profiles
nav_order: 4
nav_exclude: false
has_children: false
parent: Configuration
---
# FSLogix UserProfiles


## ON VM DC
#### ENABLE Acelerator Network on VM D2sV4
```
$VMResGroup="RG-US-WVD-VM-WEST"
$VMWVDNAME="vm-wvd-west-2111" 

 # Acelerator Network Enabled 
$nic = Get-AzNetworkInterface -ResourceGroupName $VMResGroup -Name $VMWVDNAME
$nic.EnableAcceleratedNetworking = $true
$nic | Set-AzNetworkInterface 
```

#### JOIN TO AD
```
add-computer –domainname "miatech.local"  -restart
```
#### JOIN EXIST VM TO HOSTPOOL
https://docs.microsoft.com/en-us/azure/virtual-desktop/create-host-pools-powershell#register-the-virtual-machines-to-the-azure-virtual-desktop-host-pool
Download MSI package
    https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv
    https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH




Download https://github.com/Azure-Samples/azure-files-samples/releases
and unzip package
```
#Create temp folder
New-Item -Path 'C:\temp' -ItemType Directory -Force | Out-Null

$wc = New-Object System.Net.WebClient
Write-Output $wc.DownloadFileTaskAsync("https://github.com/Azure-Samples/azure-files-samples/releases/download/v0.2.3/AzFilesHybrid.zip", "C:\temp\AzFilesHybrid.zip")

Expand-Archive -LiteralPath 'C:\temp\AzFilesHybrid.zip' -DestinationPath C:\temp\AzFilesHybrid
cd C:\temp\AzFilesHybrid 

Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
#Install-PackageProvider -Name NuGet -Force
Import-Module -Name .\AzFilesHybrid.psd1

Connect-AzAccount

$location = "westus2"
$SubscriptionId = "Microsoft Azure Sponsorship"
$ResourceGroupName = "RG-US-WVD-INFRA-WEST"
$StorageAccountName = "churchwvd"
$DomainAccountType = "ServiceLogonAccount" # ComputerAccount or ServiceLogonAccount
#$OrganizationalUnitDistinguishedName = "CN=Computers,DC=churchoregon,DC=local"
$OrganizationalUnitDistinguishedName = "OU=azure,DC=churchoregon,DC=local"

Select-AzSubscription -SubscriptionName $SubscriptionId
Get-AzStorageAccount -ResourceGroupName $ResourceGroupName

New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $StorageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2

join-AzStorageaccountForAuth -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -DomainAccountType $DomainAccountType -OrganizationalUnitDistinguishedName $OrganizationalUnitDistinguishedName
$storageaccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $StorageAccountName
$storageaccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions
$storageaccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties

net use W: \\churchwvd.file.core.windows.net\fslogixpe /user:Azure\churchwvd
```

Create 2 Group on DC:
    FSLogix Share Elevated Contributor
    FSLogix Share Contributor
And need make sync use Azure Connect!!!



## Install FSLogix Agent on AVD (WVD) VMs
```
$registryPath = "HKLM:\SOFTWARE\FSLogix\Profiles"


$scriptStartTime = get-date

#Create temp folder
New-Item -Path 'C:\temp\apps' -ItemType Directory -Force | Out-Null

#Download all source file async and wait for completion
$scriptActionStartTime = get-date
Write-host ('*** STEP 0 : Download all sources [ '+(get-date) + ' ]')
$files = @(
    @{url = "https://dl.duosecurity.com/duo-win-login-latest.exe"; path = "c:\temp\apps\duo-win-login-latest.exe"}
    @{url = "https://download.microsoft.com/download/4/8/2/4828e1c7-176a-45bf-bc6b-cce0f54ce04c/FSLogix_Apps_2.9.7654.46150.zip"; path = "c:\temp\apps\fslogix.zip"}
    @{url = "https://teams.microsoft.com/downloads/desktopurl?env=production&plat=windows&download=true&managedInstaller=true&arch=x64"; path = "c:\temp\apps\Teams.msi"}
    @{url=  "https://go.microsoft.com/fwlink/?linkid=844652"; path = "c:\temp\apps\OneDriveSetup.exe"}
    @{url=  "https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv"; path = "c:\temp\apps\Microsoft.RDInfra.RDAgent.Installer-x64-1.0.3050.2500.msi"}
    @{url=  "https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH"; path = "c:\temp\apps\Microsoft.RDInfra.RDAgentBootLoader.Installer-x64.msi"}
)

foreach ($f in $files)
{
    Write-Output "DOWNLOAD $f"
    $wc = New-Object System.Net.WebClient
    Write-Output $wc.DownloadFileTaskAsync($f.url, $f.path)
}

$scriptActionDuration = (get-date) - $scriptActionStartTime
Write-Host "Total source Download time: "$scriptActionDuration.Minutes "Minute(s), " $scriptActionDuration.seconds "Seconds and " $scriptActionDuration.Milliseconds "Milleseconds"

#Install FSLogix
$scriptActionStartTime = get-date
Write-host ('*** STEP 1 : Install FSLogix Apps [ '+(get-date) + ' ]')
Expand-Archive -Path 'C:\temp\apps\fslogix.zip' -DestinationPath 'C:\temp\apps\fslogix\'  -Force
Start-Sleep -Seconds 10
Start-Process -FilePath 'C:\temp\apps\fslogix\x64\Release\FSLogixAppsSetup.exe' -ArgumentList '/install /quiet /norestart' -Wait
$scriptActionDuration = (get-date) - $scriptActionStartTime
Write-Host "*** FSLogix Install time: "$scriptActionDuration.Minutes "Minute(s), " $scriptActionDuration.seconds "Seconds and " $scriptActionDuration.Milliseconds "Milleseconds"


#Configure FSLogix
New-ItemProperty -Path $registryPath -Name "Enabled" -Value 1 -PropertyType DWORD -Force | Out-Null
New-ItemProperty -Path $registryPath -Name "DeleteLocalProfileWhenVHDShouldApply" -Value 1 -PropertyType DWORD -Force | Out-Null
New-ItemProperty -Path $registryPath -Name "PreventLoginWithFailure" -Value 1 -PropertyType DWORD -Force | Out-Null

# MULTIPLY SESSIONS
New-ItemProperty -Path $registryPath -Name "ConcurrentUserSessions" -Value 1 -PropertyType DWORD -Force | Out-Null
New-ItemProperty -Path "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\FSLogix\ODFC" -Name "ConcurrentUserSessions" -Value 1 -PropertyType DWORD -Force | Out-Null

New-ItemProperty -Path $registryPath -Name "VHDLocations" -Value "\\churchwvd.file.core.windows.net\fslogixpe" -PropertyType MultiString -Force | Out-Null
 ######################
# Install DUO Login https://help.duo.com/s/article/1090?language=en_US

$DouIKEY = "DIXXXXXXXXXXXXXXXXXXXX"
$DuoSKEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
$DouHostAPI = "api-xxxxxxxx.duosecurity.com"
$DouArguments = @('/S', '/V"', '/qn', "IKEY=$DouIKEY", "SKEY=$DuoSKEY", "HOST=$DouHostAPI", 'AUTOPUSH="#1"', 'FAILOPEN="#1"', 'SMARTCARD="#0"', 'RDPONLY="#0"', 'UAC_PROTECTMODE=#2')

echo $DouArguments

$scriptActionStartTime = get-date
Write-host ('*** STEP 1 : Install DOU Apps [ '+(get-date) + ' ]')
Start-Process -FilePath 'c:\temp\apps\duo-win-login-latest.exe' -ArgumentList $DouArguments -Wait
$scriptActionDuration = (get-date) - $scriptActionStartTime
Write-Host "*** DOU Install time: "$scriptActionDuration.Minutes "Minute(s), " $scriptActionDuration.seconds "Seconds and " $scriptActionDuration.Milliseconds "Milleseconds"


reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 43200000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 43200000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 600000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f

reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v NoWarningNoElevationOnInstall /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v UpdatePromptSettings /t REG_DWORD /d 1 /f

```
Read logs: %ProgramData%\FSLogix\Logs

---
### REFERENCES
[Original Microsoft Policies TerminalServer-Server](https://admx.help/?Category=Windows_8.1_2012R2&Policy=Microsoft.Policies.TerminalServer-Server::TS_SESSIONS_RemoteApp_End_Timeout)


