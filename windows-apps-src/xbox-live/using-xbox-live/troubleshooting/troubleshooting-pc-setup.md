---
title: Problembehandlung beim Xbox Live-Setup auf Windows-PC
description: Erfahren Sie, wie Ihre Xbox Live-Entwicklungsumgebung auf einem Windows-PC zu beheben.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647735"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Problembehandlung beim Xbox Live-Setup auf Windows-PC

Auf Windows 10-PC können Sie sicherstellen, dass Ihr Computer eingerichtet, ordnungsgemäß mit den folgenden Schritten ist:

1. Ändern Sie Ihren Computer mit der Sandbox XDKS.1 verweisen, in denen Beispiele zum Ausführen dienen.  Führen Sie dies durch Ausführen dieses Skripts:

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. Extrahieren Sie den Inhalt der Zip-Datei, die "SourcesAndSamples.zip" innerhalb des SDK gefunden.
1. Öffnen Sie eine Beispielprojektmappe:
    1. Für C++-API: {*SDK Quellstamm*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. Für WinRT-API mit C#: {*SDK Quellstamm*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. Für WinRT-API mit C++ / CX: {*SDK Quellstamm*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. Ändern Sie die buildzielplattform "Win32" oder "X64".
1. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und erstellen Sie alles, was neu zu.
1. Starten Sie die app im Debugger.
1. Melden Sie sich mit dem Development-Konto, das Sie erstellt die [Xbox-Entwicklerportal](https://xdp.xboxlive.com), oder mit einem Retail Entwicklerkonto anzumelden, die Autorisierung in [Partner Center](https://partner.microsoft.com/dashboard).
1. Gewähren Sie der app die Berechtigung den Zugriff auf Ihre Xbox Live-Informationen.
1. Stellen Sie sicher, dass die app Sie Ihre Informationen abrufen können und Sie können Ihr Gamertag finden Sie unter.