---
title: Update Orchestrator API
description: UpdateOrchestrator schedules your automatic software updates with user impact in mind. 
ms.date: 01/29/2020
ms.topic: overview
---

# UpdateOrchestrator API

**UpdateOrchestrator** schedules your automatic software updates with user impact in mind. This API allows you to [specify actions](updateorchestratoractionkind.md), such as downloading or installing, along with their requirements in order to run updates at an optimal time that minimizes user-present impact. These features particularly benefit lower performance systems with limited or slower computing resources.

Windows 20H1 includes a first-generation solution for automatic software update use cases that were adopted by OS updates and Store App updates and exposes an initial ‘Limited Access’ version of this API for a select set of updaters of 'user-mode' apps as described below.

## Features

- Dynamically registers software updaters
 
- Invokes registered software updaters during optimal times, such as during user absence, to update 'user mode apps'.
- Includes the ability to 'keep awake' on AC power to further reduce user-away impact.

## Developer Audience

> [!IMPORTANT]
> The **UpdateOrchestrator** API is part of a Limited Access Feature (see [LimitedAccessFeatures class](/uwp/api/windows.applicationmodel.limitedaccessfeatures)). For more information or to request an unlock token, please use the [LAF Access Token Request Form](https://go.microsoft.com/fwlink/?linkid=2271232&clcid=0x409).

Use UpdateOrchestrator API if you already have background software updaters for Win32 'user mode' applications such as Adobe's updater for Acrobat Reader or Valve's Steam. This interface is not needed for UWP/Store applications as the Microsoft Store already takes advantage of this functionality for software updates.

To provide the best customer experience, this initial API release is scoped to a select set of registered updaters that meet the following criteria:

- Updates for 'user mode' applications only
- Do not involve BIOS/Firmware/Device or Software Drivers
    - Updating BIOS, firmware, or device/software drivers that have not passed a common quality criteria pose a substantial risk, particularly when a user is not present. 
- Participating in the usage of this API entails being able to vouch for all content downloaded and installed by your background software updaters on users systems via audits. 

The initial release of UpdateOrchestrator API as limited access feature is only for updaters that meet above criteria at this time.

Our aim is to improve the functionality of this API and reduce impact from multiple automatic software updaters on Windows. We would appreciate your input through this [**brief survey**](https://aka.ms/UOAPISurvey) to help us understand how UpdateOrchestrator API can better serve your developer needs.

## Expediting OEM Apps via Universal Orchestrator framework

>[!IMPORTANT]
> Universal Orchestrator provides functionality to OEMs to register an application during the imaging process to perform a one-time expedited install / update.  This installation happens within 30 minutes of a user logging into a new device.  Be aware that expediting an application may have a negative performance impact to the out of box experience for new devices. This functionality is only available on Windows versions running the November 2024 Cumulative Non-Security Preview Update.<br><br>
Windows 11 23H2 - KB5046732 (OS Build 22631.4541)<br>
Windows 11 24H2 - KB5046740 (OS Build 26100.2454)

### Requirements
To plug into the expedited app framework, the app must meet the following requirements:
- It must be a Store packaged app in MSIX format
- It must have a valid Product Family Name (PFN)

### Registration

Registration files are ASCII JSON files that contain metadata with information on the desired expedited flow, as well as any custom client-side targeting that needs to be performed.

Expedited apps support two mechanisms to update / acquire an app:
1. Directly from the Microsoft Store using a ProductId _(Recommended)_
2. From a URL that contains an MSIX package or bundle.  This package must contain a Store packaged app with a valid Package Family Name (PFN).  The OEM or App Owner is responsible for maintaining this URL.

Each registration file must contain the following required JSON properties:

| Key | Type | Description | 
| --- | ---- | ----------- |
| PFN | String | The Package Family Name of the app (ex: Microsoft.WindowsStore_8wekyb3d8bbwe) |
| OEMName | String | String to represent the OEM creating this registration |
| UpdaterName | String | Unique name to track this expedited registration |
| RegistrationVersion | Number | The Package Family Name of the app (ex: Microsoft.WindowsStore_8wekyb3d8bbwe) |
| Source | String | Allowed values:<br>Store &#124; CustomURL<br><br>Store – searches for the app directly from the Microsoft Store<br><br>CustomURL – searches for the app from a URL specified in the app registration's "Endpoint" value |
| Scenario | String | Allowed values:<br>Update &#124; Acquisition &#124; StubAcquisition<br><br>Update – (Not supported for CustomURL flows) attempts to update an existing app to its latest available version.  No work is done if the app is not present<br><br>Acquisition – attempts to acquire the latest version of an app.<br><br>StubAcquisition -  attempts to acquire a "stub" of the app (if it is available).  Acquires the full app if the stub is not available. |
| ProductId | String | (Only required for Store scenarios)<br><br>The ProductId of the desired Store app<br>(ex. 9WZDNCRFJBH4) |
| Endpoint | String | (Only required for CustomURL scenarios)<br><br>A string URI pointing to a location hosting an MSIX package. Must be an SSL URI that begins with 'https'. |

<br>
Additionally, the following optional properties can be specified to modify the behavior of the expedited app installation, or to target the expedited flow to occur only under certain conditions.

|Key|Type|Default|Description|
|---|----|-------|-----------|
| AllowedInOobe | Boolean | False | Whether this expedited app should run during user OOBE.<br><br>**Note:** Use caution when setting this to true, since this may create resource constraints on a device during the Out of Box Experience flow and negatively impact user perceived performance. |
| MaxRetryCount | Number | 1 | The number of times this updater is allowed to retry after failure.<br><br>Maximum allowed value is: 5 |
| TimeoutDurationInMinutes | Number | 15 | The duration in minutes to wait for this updater to complete work.<br><br>Maximum allowed value is: 30 |
| Architecture | String | No restriction | Allowed values:<br> "amd64" &#124; "arm64".<br><br>Specifies whether the expedited work should only occur for a specific architecture. |
| MinimumAllowedBuildVersion | Number | No restriction | Minimum Windows build versions where the expedited work is allowed.<br><br>Ex. If this is set to 22631, expedited work will be allowed for Windows 11 23H2 (10.0.22631.x), but blocked for Windows 11 22H2 (10.0.22621.x) |
| HonorDeprovisioning | Boolean | False | (Only applicable for Acquisition scenarios)<br><br> If the app has been previously deprovisioned, do not attempt to acquire it again. |
| SkipIfPresent | Boolean | False | (Only applicable for Acquisition scenarios)<br><br>Do not perform the expedited work if any version of the app is already present. |
| Priority | Number | 100 | A numeric value from 1 – 100 to indicate relative priority of this application update.<br><br>Lower values indicate a higher relative priority to other expedited apps. |
| ExcludedRegions | Array (String) | No restrictions | A JSON array of strings for regions where this app should NOT be expedited.<br>Each entry in the array corresponds to the 2 letter ISO 3166-1 country code of the desired region.<br><br>Ex: `["US", "MX"]` would prevent this flow on devices where the region is United States or Mexico.<br><br>**Note:** This value cannot be used in conjunction with IncludedRegions. |
| IncludedRegions | Array (String) | No restrictions | A JSON array of strings that indicate an allow list of regions where this app should be expedited.<br>Each entry in the array corresponds to the 2 letter ISO 3166-1 country code of the desired region.<br><br>Ex: `["US", "MX"]` would allow this flow only on devices where the region is United States or Mexico.<br><br>**Note:** This value cannot be used in conjunction with ExcludedRegions. |
| IncludedEditions | Array (Number) | No restrictions | A JSON array of numbers that indicate an allow list of Editions where this app should be expedited.<br>Each entry in the array corresponds to the Edition code retrieved by the [GetProductInfo API](https://learn.microsoft.com/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo).<br><br>Ex: `[121, 122]` to include only Education and EducationN Editions<br><br>**Note:** This value cannot be used in conjunction with ExcludedEditions. |
| ExcludedEditions | Array (Number) | No restrictions | A JSON array of numbers for Editions where this app should NOT be expedited.<br>Each entry in the array corresponds to the Edition code retrieved by the [GetProductInfo API](https://learn.microsoft.com/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo).<br><br> Ex: `[121, 122]` to exclude Education and EducationN Editions.<br><br>**Note:** This value cannot be used in conjunction with IncludedEditions. |  
                                            
### Samples
**Store-based stub acquisition, only in US and Mexico, to execute during OOBE**<br>
````
    {  
        "RegistrationVersion":1,  
        "Source": "Store",  
        "Scenario": "StubAcquisition",  
        "PFN": "FakePackageFamilyName",  
        "ProductId": "11111111111",  
        "HonorDeprovisioning": true,  
        "AllowedInOobe": true,  
        "IncludedRegions": ["US", "MX"],  
        "Priority": 50  
    }
````
<br>

**URL-based app acquisition on amd64 devices, excluding Education and EducationN editions, on Windows 11 23H2 only `(not Windows 11 22H2)`** <br>  
````
    {  
        "RegistrationVersion":2,  
        "Source": "CustomURL",  
        "Scenario": "Acquisition",  
        "PFN": "FakePackageFamilyName",  
        "Endpoint": "https://<SSL_URI>",   
        "ExcludedEditions": [121, 122],   
        "Architecture": "amd64",   
        "MinimumAllowedBuildVersion": 22631,  
        "Priority": 60 
    }
````         
<br>                                       
    
### Tools

To facilitate the registration process and provide actionable feedback on the registration metadata, OEMs need to use the **AppOrchestration** powershell scripts from the following location:  
[microsoft/ms-update-universalorchestrator: Scripts and tools to onboard to Universal Orchestrator based update flows](https://github.com/microsoft/ms-update-universalorchestrator)

The scripts perform basic validation and stage the registration to the appropriate location on the device.  On any failures, the scripts throw an exception with the specific failure details.  
<br>
To use the scripts:
1. Download the scripts to your device
2. In a powershell window, run "Import-Module <PathToScripts>\AppOrchestration.psd1"

**Note:** These scripts require the user to have administrative privileges on the device, and must be executed from an elevated console.

There are 4 main cmdlets used for the registration flow

**Test-UpdaterRegistration <PathToRegistrationFile>**  
Purpose:  Validate contents of a proposed registration file (without performing registration).  Allows OEM to iterate on the registration file payload without affecting the device

**Add-UpdaterRegistration <PathToRegistrationFile>**  
Purpose:  Validate and Stage the contents of a registration file to the appropriate location, to onboard to the expedited app flow

**Get-UpdaterRegistration `[OEMName]` `[UpdaterName]`**  
Purpose:  If OEMName and UpdaterName are provided, returns a summary of an existing registration that matches those values.  If those inputs are omitted, returns a summary of all the current registrations present on the device.

**Remove-UpdaterRegistration <OEMName> <UpdaterName>**  
Purpose:  Unstages any registration that matches the OEMName and UpdaterName values.
<br><br>

### Execution
The Universal Orchestrator framework automatically invokes each of the registered apps, in sequence based on relative priority, within the first 30 minutes of a user reaching the Desktop on a new device (or during User OOBE if AllowedInOobe is set to true).  Each registered application added by the OEM registration process will be attempted until either:  
- It is successfully installed  
- It surpasses the maximum amount of failures specified in MaxRetryCount.  After each failure, the app will enter a cooldown period of 30 minutes before it is attempted again.

The Universal Orchestrator framework will not perform expedited attempts if any of the following conditions are true: 
- Device does not have internet access  
- Device is on a metered network  
- Device is on battery, and battery saver is enabled  
- Device is configured with a [Windows Update Restricted Network Traffic policy](https://learn.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services#bkmk-wu)  
- Device is configured with a [CTA policy](https://learn.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-deviceaccess-setregionspecificprivacyaccesspolicy) that is not set for AutoApprove  
<br>
In each of these cases, the Universal Orchestrator framework will keep the registrations in place until the device configuration allows expedited attempts to proceed.
<br>
If the app registration contains optional values that block the expedited flow (ex. due to edition type), the Universal Orchestrator framework will consider this registration request fulfilled and will not attempt it again, even if those conditions may later change on a device.
<br><br>

> [!IMPORTANT]
> Exercise caution when opting to expedite apps via this framework, as the update operations occur when the device may be in use and can cause a negative performance impact of the user experience on a new device.
 







 


