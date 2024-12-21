---
layout: post
title:  "CPU Turbo mode"
excerpt: "When laptop fan gets too loud"
image: assets/images/cpu-turbo-thumbnail.webp
---

![CPU Turbo](/assets/images/cpu-turbo.webp)


TL;DR:

Problem: Laptop gets hot and fan is too loud.

Solution: In cmd.exe terminal execute following three commands.

Enable the PERFBOOSTMODE option of Windows power plan:

```batchfile
reg.exe ADD "HKLM\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\54533251-82be-4824-96c1-47b60b740d00\be337238-0d82-4146-a960-4f3749d470c7" /v Attributes /t REG_DWORD /d 2
```

Disable the CPU Turbo mode in the currently used Windows power plan in plugged-in state and on battery power:

```batchfile
FOR /F "tokens=4" %i IN ('powercfg.exe /setactivescheme') do powercfg.exe /setacvalueindex %i SUB_PROCESSOR PERFBOOSTMODE 0
FOR /F "tokens=4" %i IN ('powercfg.exe /setdctivescheme') do powercfg.exe /setacvalueindex %i SUB_PROCESSOR PERFBOOSTMODE 0
```

The CPU Turbo mode has following names across CPUs:

- AMD PowerNow!
- AMD Turbo Core
- AMD Core Performance Boost (CPB)
- AMD Precision Boost Overdrive
- AMD Precision Boost 2
- Intel Geyserville
- Intel SpeedStep, Enhanced SpeedStep (EIST)
- Intel Speed Shift Technology (SST)
- Intel Turbo Boost
- Intel Turbo Boost Max 3.0 Technology
- dynamic overclocking
- dynamic frequency scaling
- dynamic voltage and frequency scaling (DVFS)

The narrative soon.

