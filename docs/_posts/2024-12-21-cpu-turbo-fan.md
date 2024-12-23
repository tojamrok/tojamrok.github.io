---
layout: post
title:  "CPU Turbo mode"
excerpt: "When laptop fan gets too loud"
image: assets/images/cpu-turbo-thumbnail.webp
---

![CPU Turbo](/assets/images/cpu-turbo.webp)

## TL;DR:

### Problem: Laptop gets hot and fan is too loud

### Solution: In cmd.exe terminal execute following three commands

Enable the PERFBOOSTMODE option of Windows power plan:

```batchfile
reg.exe ADD "HKLM\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\54533251-82be-4824-96c1-47b60b740d00\be337238-0d82-4146-a960-4f3749d470c7" /v Attributes /t REG_DWORD /d 2
```

Disable the CPU Turbo mode in the currently used Windows power plan in plugged-in state and on battery power:

```batchfile
reg.exe ADD "HKLM\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\54533251-82be-4824-96c1-47b60b740d00\be337238-0d82-4146-a960-4f3749d470c7" /v Attributes /t REG_DWORD /d 2
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

## The narrative

I put my hands on a new laptop, rather top shelf.  End of the year holiday season is a good time to rest so I visited GOG[^1] and returned to the Heroes of Might and Magic 3[^2].  The gaming session was great... for an hour.  After that I woke my spouse up, with the sound of the CPU fan.  Indeed, the game was playing smooth and all but the fan and all the heat and the battery life, all went to crazy levels.  I made a round of typical hardware, firmware and software solutions.

### Vendor

First the manual[^3].  It states that pressing `Fn+F8` can alleviate load on the CPU.  There's even such funny statement: 

> Note: In balanced mode or best-performance mode, avoid keeping your hands, your lap, or any other part of your body in contact with a hot section of the computer for 10 seconds or longer.

Toggling the key result is visible by Windows 11 `Power & battery` `Power Mode` setting.  Alas, the `Best Power Efficiency mode` didn't do much.

![Windows 11, Settings, Power & battery, Power Mode](/assets/images/windows-power-and-battery-power-mode.webp)
*Windows 11, Settings, Power & battery, Power Mode*

However the manual mentions the `Cool and Quiet on lap` BIOS setting as well.  That sounds like exactly the thing that I want to address, the hot and loud on lap laptop.  The Internet wisdom says it can turn 5h battery life into 7h battery life.  It didn't help in my case though.  There's nothing more in the Lenovo BIOS for the Thinkpad regarding power or battery management, it's bare bones BIOS.

I've searched a little in the vendor tools and found a few tools which sounded like something which can fix the problem.  There's `Lenovo Vantage`[^5] boasting with `Optimal Battery Performance` handling, sadly its `Power` settings aren't helpful.  Then there's the tailored to my needs `Lenovo Performance Tuner`[^6],[^7] which is meant to "Get the Most Out of My Workstation".  It has interesting features: can ask the operating system to change CPU priority and CPU affinity of a process, something achievable with the Windows `Task Manager` but more permanent.  CPU priority and CPU affinity didn't help me though.  There's no possibility to do anything else with the Lenovo software, the overclocking features are not available to the "mobile" flavour of CPUs.

![Lenovo Performance Tuner](/assets/images/lenovo-performance-tuner.webp)
*Lenovo Performance Tuner*

### Foundry

Off to the CPU manufacturer tools.  There's `AMD Software: Adrenalin Edition`[^9] with the `AMD Radeon Chill` and other features, but they are either disabled or just unhelpful.  There's `AMD Ryzen Master`[^4] which can help me, but it doesn't support the processor.  I made a detour to an open source alternative `RyzenAdj`[^10] which was created for this specific use case - to support CPUs not supported by Ryzen Master - maintained in a typical open source way with reverse-engineering and gathering information from `AMD BIOS and kernel developer's guide`[^11], with a myriad of UI apps on top, but for now I don't want to try a tool which can brick my device and make the warranty void.

![AMD Ryzen Master error](/assets/images/amd-ryzen-master-error.webp)
*AMD Ryzen Master error*

So back to the CPU documentation.  On page 657 and 659 of the `AMD64 Architecture Programmer's Manual, Volumes 1-5`[^12], chapter 17 `Hardware Performance Monitoring and Control`, there's the passage:

> The AMD64 architecture provides several mechanisms by which software can monitor and control processor performance to optimize power use. [...] Core performance boost (CPB) dynamically increases core clock rate beyond that defined for the P0 power state to achieve higher performance while maintaining power consumption below a preset level. [...] CPB ensures that average power consumption over a thermally significant time period remains at or below the defined power limit.

Hm.  Sounds like the thing.

### Platform

A little bit of googling around and the server-related `Power and performance tuning`[^13] sheds more light on the `Power plans`, `powercfg.exe` and the UI version of it.  It turns out that there's no Windows 11, 10 or 8 UI for the power plans but the good old `Control Panel` with Windows 7-like UI contains the `Control Panel\Hardware and Sound\Power Options\Edit Plan Settings` editable power plans.  Playing with the `Processor power management`'s `Maximum processor state` and `Minimum processor state` and even setting it to ridiculously low values doesn't help with my issue, however I can see it does change the way the power consumption behaves.  But nothing about turbo.  And then the knowledge of the Internet reveals that Microsoft wanted to save unsuspecting users from breaking their machines and thus hid the Turbo setting - and it backfired.  There's a need to unhide and enable the setting by Windows registry, as described in the [TL;DR: section](#tldr) above.  Once that's done the following `Processor performance boost mode` option with a set of values appears[^14]:

![Windows 11, Control Panel, Hardware and Sound, Power Options, Edit Plan Settings](/assets/images/control-panel-power-plan-boost-mode.webp)
*Windows 11, Control Panel, Hardware and Sound, Power Options, Edit Plan Settings*

And it did work.  Disabling it made the computer use 3.5 Watts instead of 18 Watts.

Let that sink in:  3.5 W instead of 18 W.

In the knowledge gathering and tuning process I also used a number of old performance monitoring friends: CPU-Z[^15], HWiNFO[^16].  Below you can see the Wattage and CPU temperature (no fan controller available so no sound dB graph) when running HoMM3 on CPU, first with Turbo on battery and later plugged in;  then on CPU without boost first plugged in and later on battery:

![HWiNFO, running HoMM3, CPU with boost](/assets/images/hwinfo-cpu-with-boost.webp)
*HWiNFO, running Heroes of Might and Magic III, CPU with boost*

![HWiNFO, running HoMM3, CPU without boost](/assets/images/hwinfo-cpu-without-boost.webp)
*HWiNFO, running Heroes of Might and Magic III, CPU without boost*

### Mods

And that would be it.  But then I made another birds eye view and asked, how does the HoMM3 community fare with the issue.  The answer was modding with HD+ mod[^17].  It has the `Sys.CPU.ReduceUsage=2` option which fixes my issue.  But the mod is closed source, it's homepage looks fishy, it's a very heavy mod while - given the description of the mod's configuration option - the fix is lightweight.  Fortunately there's also the lightweight `HeroesGL` mod which ports Heroes of Might and Magic 1, 2, 3 and 4 from `DirectDraw` to `OpenGL` and as a side fix it also hooks into the `DDraw.dll` and adds this tiny elegant fix[^18]:

```cpp
BOOL __stdcall PeekMessageHook(LPMSG lpMsg, HWND hWnd, UINT wMsgFilterMin, UINT wMsgFilterMax, UINT wRemoveMsg)
{
  if (PeekMessage(lpMsg, hWnd, wMsgFilterMin, wMsgFilterMax, wRemoveMsg))
    return TRUE;

  if (config.coldCPU)
    Sleep(1);

  return FALSE;
}
```

I've tried it on the GOG version of HoMM3 but it didn't work due to the recent 2024-11-13 change which adds another level of indirection to the `DDraw.dll` with the `DDrawCompat`[^19].  Thankfully the GOG distributes the game with two binaries: `Heroes3.exe` which uses the `DDrawCompat` and `H3Blade.exe` which uses original `DDraw.dll`.  And the above code of the `HeroesGL` does fix the game run with the `H3Blade.exe` executable:

![HWiNFO, running HoMM3 with HeroesGL fix, CPU regardless of boost](/assets/images/hwinfo-heroesgl.webp)
*HWiNFO, running Heroes of Might and Magic III with HeroesGL fix, CPU regardless of boost*

## Non-closing words

I really like the way the `HeroesGL` works, the source code is available on GitHub, with MIT license and it's clean.  I'd like to contact and express my gratitude to the author of the mod, Oleksiy Ryabchun.  And here the real world kicks in.  The binary version of the mod which I got from someone's Google Drive[^20],[^21] (VirusTotal is kind of OK with it[^22]) was created 2022-01-05.  Soon after the personal webpage of the author was collected by the `Wayback Machine` on 2022-01-20 and it archived the content, but on 2022-03-02 the page was empty[^23] and the author is silent since then.  Since the invasion.

And it struck me, wondering how to redeem the non ideal world.  Maybe paying due respect to works of persons such as [Maria Primachenko](https://en.uartlib.org/books/a-set-of-postcards-by-maria-primachenko/)[^24] and more generally the [Library Of Ukrainian Art](https://en.uartlib.org/exclusive/)[^25] would offset the damage, even if by tiny bit.  Her works fit the HoMM esthetics.

![Heroes of Might and Magic: A Strategic Quest, Castle Ironfist campaign map by Maria Primachenko](/assets/images/maria-primachenko-castle-ironfist.webp)
*Heroes of Might and Magic: A Strategic Quest, Castle Ironfist campaign map by Maria Primachenko*

![Heroes of Might and Magic: A Strategic Quest, Sorceress troops, Phoenix by Maria Primachenko](/assets/images/maria-primachenko-phoenix.webp)
*Heroes of Might and Magic: A Strategic Quest, Sorceress troops, Phoenix by Maria Primachenko*

![Heroes of Might and Magic III, Castle troops, Cavalier by Maria Primachenko](/assets/images/maria-primachenko-castle-cavalier.webp)
*Heroes of Might and Magic III, Castle troops, Cavalier by Maria Primachenko*

[^1]: GOG: [https://www.gog.com/](https://www.gog.com/),
[^2]: GOG, Heroes of Might and Magic 3 Complete Edition: [https://www.gog.com/pl/game/heroes_of_might_and_magic_3_complete_edition](https://www.gog.com/pl/game/heroes_of_might_and_magic_3_complete_edition),
[^3]: ThinkPad P14s Gen 5 and ThinkPad P16s Gen 3 User Guide: [https://download.lenovo.com/manual/thinkpad_p14sg5_p16sg3/manuals/UG_EN.pdf](https://download.lenovo.com/manual/thinkpad_p14sg5_p16sg3/manuals/UG_EN.pdf), page 30,
[^4]: AMD Ryzen Master: [https://www.amd.com/en/products/software/ryzen-master.html](https://www.amd.com/en/products/software/ryzen-master.html), 
[^5]: Lenovo Vangate: [https://www.lenovo.com/us/en/software/vantage](https://www.lenovo.com/us/en/software/vantage), 
[^6]: Lenovo Performance Tuner press release: [https://news.lenovo.com/lenovo-performance-tuner-2-0-get-the-most-out-of-your-workstation/](https://news.lenovo.com/lenovo-performance-tuner-2-0-get-the-most-out-of-your-workstation/), 
[^7]: Lenovo Performance Tuner homepage: [https://www.thinkworkstationsoftware.com/?view=perf](https://www.thinkworkstationsoftware.com/?view=perf), 
[^9]: AMD Software: Adrenalin Edition: [https://www.amd.com/en/products/software/adrenalin.html](https://www.amd.com/en/products/software/adrenalin.html), 
[^10]: RyzenAdj: [https://github.com/FlyGoat/RyzenAdj](https://github.com/FlyGoat/RyzenAdj), 
[^11]: AMD Technical Documentation: [https://www.amd.com/en/developer.html#documentation](https://www.amd.com/en/developer.html#documentation), 
[^12]: AMD64 Architecture Programmer's Manual, Volumes 1-5: [https://www.amd.com/content/dam/amd/en/documents/processor-tech-docs/programmer-references/40332.pdf](https://www.amd.com/content/dam/amd/en/documents/processor-tech-docs/programmer-references/40332.pdf), 
[^13]: Microsoft Windows Server, Power and performance tuning: [https://learn.microsoft.com/en-us/windows-server/administration/performance-tuning/hardware/power/power-performance-tuning](https://learn.microsoft.com/en-us/windows-server/administration/performance-tuning/hardware/power/power-performance-tuning), 
[^14]: Windows Power Settings, Processor Power Management Options, PerfBoostMode: [https://learn.microsoft.com/en-us/windows-hardware/customize/power-settings/options-for-perf-state-engine-perfboostmode](https://learn.microsoft.com/en-us/windows-hardware/customize/power-settings/options-for-perf-state-engine-perfboostmode), 
[^15]: CPU-Z: [https://cpuid.com/softwares/cpu-z.html](https://cpuid.com/softwares/cpu-z.html), 
[^16]: HWiNFO: [https://www.hwinfo.com](https://www.hwinfo.com), 
[^17]: HeroesIII HD+ mod changelog: [https://sites.google.com/site/heroes3hd/eng/changelog](https://sites.google.com/site/heroes3hd/eng/changelog), 
[^18]: HeroesGL mod, coldCPU fix: [https://github.com/vEhwaz/HeroesGL/blob/b0d6d32c19999465a4d0345636ece171287c8ea7/src/Heroes3GL/Hooks.cpp#L891](https://github.com/vEhwaz/HeroesGL/blob/b0d6d32c19999465a4d0345636ece171287c8ea7/src/Heroes3GL/Hooks.cpp#L891), 
[^19]: DDrawCompat library: [https://github.com/narzoul/DDrawCompat](https://github.com/narzoul/DDrawCompat), 
[^20]: Celestial Heavens, The Heroes Round Table forum, Verok's OpenGL Wrappers for Heroes are gone thread: [https://www.celestialheavens.com/forum/topic/17477](https://www.celestialheavens.com/forum/topic/17477), 
[^21]: HeroesGL binaries snapshot: [https://drive.google.com/file/d/1qomaln7wErXksw0hUdrMLgf-c73sqbQ_/view?usp=share_link](https://drive.google.com/file/d/1qomaln7wErXksw0hUdrMLgf-c73sqbQ_/view?usp=share_link), 
[^22]: VirusTotal: [https://www.virustotal.com/gui/home/upload](https://www.virustotal.com/gui/home/upload), 
[^23]: Wayback Machine archive, Verok's official blog, Best patches for favorite games: [https://web.archive.org/web/20220701000000\*/https://verokster.blogspot.com/2018/10/heroes-of-might-and-magic-i-iv-gl.html](https://web.archive.org/web/20220701000000*/https://verokster.blogspot.com/2018/10/heroes-of-might-and-magic-i-iv-gl.html), 
[^24]: Ukrainian art library, Maria Primachenko postcards: [https://en.uartlib.org/books/a-set-of-postcards-by-maria-primachenko/](https://en.uartlib.org/books/a-set-of-postcards-by-maria-primachenko/), 
[^25]: Ukrainian art library, exclusive content: [https://en.uartlib.org/exclusive/](https://en.uartlib.org/exclusive/), 
