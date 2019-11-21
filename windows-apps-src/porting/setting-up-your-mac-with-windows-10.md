---
description: Verwenden Sie Ihren aktuellen Mac-Computer zum Entwickeln von Apps für Windows.
title: Einrichten von Windows 10 auf Mac
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259118"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Einrichten von Windows 10 auf Mac


Verwenden Sie Ihren aktuellen Mac-Computer zum Entwickeln von Apps für Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Ausführen von Windows auf einem Mac und Verwenden von Visual Studio

Sie sind bereit zum Entwickeln universeller Windows-Apps, haben aber keinen PC zur Hand? Das ist kein Problem – Sie können auch Ihren Mac verwenden! With popular third-party solutions like Apple Boot Camp, Oracle VirtualBox, VMware Fusion, and Parallels Desktop, you can install Windows 10 and Microsoft Visual Studio on your Apple computer.

**Note**  You will need a Windows 10 bootable image on disk or USB flash drive. Wenn Sie MSDN-Abonnent sind, können Sie das Image aus dem Center mit Downloads für MSDN-Abonnenten herunterladen. If you aren't a subscriber, the installer can be purchased from the [Microsoft Store](https://www.microsoft.com/store/apps). Sie können es auch [hier](https://www.microsoft.com/software-download/windows10) herunterladen. Das ist hilfreich, wenn Sie Windows bereits verwenden und ein Upgrade ausführen möchten.

Once you have Windows running, you can then install the latest release of Visual Studio from [Developer downloads for Windows 10](https://developer.microsoft.com/en-us/windows/downloads) and start writing apps!

**Note**  If you plan to use the Visual Studio device emulators, you **must** install a 64-bit (x64) version of Windows 10 Pro or better. Leider unterstützen einige ältere Macs 64-Bit-Versionen von Windows nicht. Auf dieser[Supportseite von Apple](https://support.apple.com/kb/HT5634)können Sie nachsehen, ob Ihre Hardware kompatibel ist.

## <a name="apple-boot-camp"></a>Apple Boot Camp

The Boot Camp Assistant app is pre-installed on every recent Mac, and launching it will walk you through the process of installing Windows 10. Sie benötigen nur eine Kopie von Windows (aus den oben aufgeführten Quellen) und mindestens 30 GB freien Speicherplatz. Nach der Installation können Sie mit Mac OSX oder Windows 10 starten. Weitere Informationen finden Sie auf der [Seite mit Anweisungen zu Boot Camp](https://support.apple.com/HT201468) von Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Mit Parallels Desktop 11 können Sie Windows-Apps (einschließlich Visual Studio und Cortana) nebeneinander mit vorhandenen Mac-Anwendungen ausführen. Eine Pro-Version mit zusätzlichen Features für Entwickler, einschließlich verbesserten Debuggingfunktionen, und Unterstützung für Docker und Jenkins. Weitere Informationen und eine kostenlose Testversion finden Sie unter [Parallels Desktop](https://www.parallels.com/download/desktop/).

## <a name="vmware-fusion"></a>VMWare Fusion

Mit Fusion 8 von VMWare können Sie Visual Studio direkt auf Ihrem Mac-Desktop ausführen. Hiervon steht eine Pro-Version für Entwickler mit erweiterten Features wie etwa vSphere-Unterstützung zur Verfügung. Weitere Informationen und eine kostenlose Testversion finden Sie unter [VMware Fusion](http://www.vmware.com/products/fusion/).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox ist eine kostenlose Anwendung für die Ausführung virtueller Computer auf Ihrem Computer, die die Ausführung von Windows auf einem Mac unterstützt. Dies ist eine einfache Option zu einem ansprechenden Preis. Weitere Informationen finden Sie unter [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

