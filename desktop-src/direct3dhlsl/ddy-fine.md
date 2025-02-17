---
title: ddy_fine function
description: Computes a high precision partial derivative with respect to the screen-space y-coordinate.
ms.assetid: 29fcdbc9-470b-4b5b-b18c-f75dd2c87920
keywords:
- ddy_fine function HLSL
topic_type:
- apiref
api_name:
- ddy_fine
api_type:
- NA
ms.topic: reference
ms.date: 05/31/2018
api_location: 
---

# ddy\_fine function

Computes a high precision partial derivative with respect to the screen-space y-coordinate.

## Syntax

``` syntax
float ddy_fine(
  in float value
);
```

## Parameters

<dl> <dt>

*value* \[in\]
</dt> <dd>

Type: **float**

The input value.

</dd> </dl>

## Return value

Type: **float**

The high precision partial derivative of *value*.

## Remarks

The following overloaded versions are also available:

``` syntax
float2 ddy_fine(float2 value);
float3 ddy_fine(float3 value);
float4 ddy_fine(float4 value);  
```

### Minimum Shader Model

This function is supported in the following shader models.



| Shader Model                                                                | Supported |
|-----------------------------------------------------------------------------|-----------|
| [Shader Model 5](d3d11-graphics-reference-sm5.md) and higher shader models | yes       |



 

This function is supported in the following types of shaders:



| Vertex | Hull | Domain | Geometry | Pixel | Compute |
|--------|------|--------|----------|-------|---------|
|        |      |        |          | x     |         |



 

## See also

<dl> <dt>

[Intrinsic Functions](dx-graphics-hlsl-intrinsic-functions.md)
</dt> <dt>

[Shader Model 5](d3d11-graphics-reference-sm5.md)
</dt> </dl>

 

 




