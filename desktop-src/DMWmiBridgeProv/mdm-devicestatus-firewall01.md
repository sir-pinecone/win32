---
title: MDM_DeviceStatus_Firewall01 class
description: The MDM\_DeviceStatus\_Firewall01 class is used by the enterprise to query the state of firewall compliance of devices with their enterprise policies.
ms.assetid: 0f62350c-8c7b-44fb-b163-dedaf4669895
keywords:
- MDM_DeviceStatus_Firewall01 class
- MDM_DeviceStatus_Firewall01 class, described
topic_type:
- apiref
api_name:
- MDM_DeviceStatus_Firewall01
- MDM_DeviceStatus_Firewall01.InstanceID
- MDM_DeviceStatus_Firewall01.ParentID
api_location:
- DMWmiBridgeProv.dll
api_type:
- DllExport
ms.topic: reference
ms.date: 05/31/2018
---

# MDM\_DeviceStatus\_Firewall01 class



The **MDM\_DeviceStatus\_Firewall01** class is used by the enterprise to query the state of firewall compliance of devices with their enterprise policies.

The following syntax is simplified from MOF code and includes all inherited properties.

## Syntax

``` syntax
[InPartition("local-system"), dynamic, provider("DMWmiBridgeProv")]
class MDM_DeviceStatus_Firewall01
{
  string InstanceID;
  string ParentID;
  sint32 Status;
};
```

## Members

The **MDM\_DeviceStatus\_Firewall01** class has these types of members:

-   [Properties](#properties)

### Properties

The **MDM\_DeviceStatus\_Firewall01** class has these properties.

<dl> <dt>

**InstanceID**
</dt> <dd> <dl> <dt>

Data type: **string**
</dt> <dt>

Access type: Read-only
</dt> <dt>

Qualifiers: [**key**](/windows/desktop/WmiSdk/key-qualifier)
</dt> </dl>

Node for the firewall query.

</dd> <dt>

**ParentID**
</dt> <dd> <dl> <dt>

Data type: **string**
</dt> <dt>

Access type: Read-only
</dt> <dt>

Qualifiers: [**key**](/windows/desktop/WmiSdk/key-qualifier)
</dt> </dl>

Describes the full path to the parent node. For this class, the string is "./Vendor/MSFT/DeviceStatus"

</dd> <dt>

[Status](/windows/client-management/mdm/devicestatus-csp#devicestatus-battery-status)
</dt> <dd> <dl> <dt>

Data type: **sint32**
</dt> <dt>

Access type: Read/write
</dt> </dl>

</dd> </dl>

## Requirements



| Requirement | Value |
|-------------------------------------|------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows 10 \[desktop apps only\]<br/>                                                    |
| Minimum supported server<br/> | None supported<br/>                                                                      |
| Namespace<br/>                | Root\\cimv2\\mdm\\dmmap<br/>                                                             |
| MOF<br/>                      | <dl> <dt>DMWmiBridgeProv.mof</dt> </dl> |
| DLL<br/>                      | <dl> <dt>DMWmiBridgeProv.dll</dt> </dl> |



 

