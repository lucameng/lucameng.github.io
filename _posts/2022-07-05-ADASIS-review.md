---
layout: post
title: ADASIS v2 review
description: "ADAS--高级驾驶辅助系统"
tags: [自动驾驶]
image:
  path: /images/abstract-5.jpg
  feature: abstract-5.jpg
---

## Introduction

> These day I have been reading *ADASIS_v2_Specification*. As a matter of fact, *ADASIS_v3* was already out there. So why do I have to *read ADAS_v2* ? Only my mentor Roger knows.  
> Below is my general review towards *ADASIS_v2_Specification*, just as a note for myself.

___

## main entities of ADAS applications

- ADAS Horizon Provider
- ADAS Protocol 传输协议
- ADAS Application  
 - ADAS Resconstructor 数据重构（ADAS Horizon ->  Client side）

___

## ADASIS Concept

### Digital Map Database

road networks contain:
- Links (only care those ahead of the vehicle)
- Nodes (define connectivity between links)

### Path

Paths are built from databasse links and their connectivity. (single entity)

Paths contain characteristics, such as crossings, road attributes and so on.

### Optimized Path Representation

Main Path -> First-Level Sub-Path -> Second-Level Sub-Path

___

## Blocks

Paths are identify by `path identifier` (6 bits available)

`Stub` marks the start of a (sub-)path and it is located on another (parent) path

`Profile` refers to characteristics of paths

Stubs and profiles are defined by path identifier and an `offset` along that path.

`offset` is the distance between an entity and the start of a path

`Vehicle status`: position in respect to the Av2 Horizon and other characteristics (such as speed and heading)

___

### Offsets

the position of map entities and the vehical position is defined by the `path identifier` and the `distance from the start of path`--`Offset`

- 13 bits available


### Path Profiles

- Profile Type
- Profile Interpolation Type 插值
- Profile offset
- Profile Spot

### Profile

Characteristic of a path that has a defined value.

Typical examples:

- Road curvature  
- road heading

### Stub

- Value 5: used for systems with very low bandwidth or storage capacity...  
- Value 6: provides information for the continuation of the main path such as TURN ANGLE and RELATIVE PROBABILITY.  
- Value 8 to 63: Normal STUB message  
  
### Segment

___

## Guidelines and Recommendations

### Maximum horizon trailing length

behind the current vehicle position

    
### ADASIS v2 Mini

Only a SEGMENT message with a PATH INDEX of 4 describes the attributes of the link where the vehicle is currently located is sent.

### Message Priority

POSITION message can have a higher CAN id than the STUB message, so as to achieve better consistency of positioning.





___


## QUESTIONS to be addressed

1. CAN bus: multyplexing (polymorphism)  
2. Figure  
3. Interpolation

