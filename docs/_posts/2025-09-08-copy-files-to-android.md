---
layout: post
title:  "Jak skopiować pliki z MacOS-a na Androida"
excerpt: Jak skopiować pliki z MacOS-a na Androida
image: assets/images/file-transfer.gif
---

![File transfer](/assets/images/file-transfer.gif)

Przyjemnie i bezproblemowo można postawić serwer FTP na Androidzie z użyciem [Foobar2000](https://www.foobar2000.org/android) (w Google Play'u: [Foobar2000](https://play.google.com/store/apps/details?id=com.foobar2000.foobar2000)).  Postawienie go jest trywialne: `ikonka ustawień ⚙` -> `FTP Server` -> `Server running` i działa.  

![Foobar2000 FTP Server](/assets/images/foobar2000-ftp.webp)

Z kolei na MacOS-ie wystarczająco dobrym zamiennikiem dla Total Commandera jest [Nimble Commander](https://magnumbytes.com/) (w App Storze: [Nimble Commander](https://apps.apple.com/us/app/nimble-commander/id905202937)).  W menu `Go` -> `Connect to` -> `FTP Server`, albo w menu `Help`, albo pod skrótem `⌘+K` starczy wpisać `IP`, `port`, `użytkownika` i `hasło` - naprawdę minimalny zestaw akcji.

![Nimble Commander FTP connection](/assets/images/nimble-commander.webp)

---

Co nie działa dla mnie.

[Total Commander](https://www.ghisler.com) z wtyczką [WiFi](https://www.ghisler.com/androidplugins/wifi) (w Google Play'u: [Total Commander](https://play.google.com/store/apps/details?id=com.ghisler.android.TotalCommander) i [wtyczka WiFi](https://play.google.com/store/apps/details?id=com.ghisler.tcplugins.wifitransfer)) wyglądają jak właściwe narzędzie, ale trudno się je konfiguruje i ja nie potrafiłem w kilka minut tego zrobić.

![Total Commander](/assets/images/total-commander.webp)


Tak samo z wtyczką [LAN](https://play.google.com/store/apps/details?id=com.ghisler.tcplugins.LAN) do Total Commandera, nie umiem go poprawnie postawić, aby potem użyć na PC-cie [mount_smbfs](https://man.freebsd.org/cgi/man.cgi?mount_smbfs).

Wydawałoby się, że [OpenMTP](https://openmtp.ganeshrvel.com) robi robotę, ale wymaga zaskakująco dużo kroków, aby zaczął działać - i dla mnie kiedyś był łaskawy, ale kilka innych razy raczej zawodził.  No i wymaga kabla.

![OpenMTP](/assets/images/openmtp.webp)
![OpenMTP error](/assets/images/openmtp-error.webp)

Da się zaprząc do akcji [adb](developer.android.com/tools/adb), ale jest to narzędzie programistyczne, więc zanim się go użyje, należy na Androidzie wykonać magiczne akcje UI-owe, a na kompie zainstalować gigabajty SDK-ów.  No i wymaga kabla, po parowanie po WiFi to kolejny bieg z przeszkodami.

![Android Debug Bridge](/assets/images/adb.webp)

