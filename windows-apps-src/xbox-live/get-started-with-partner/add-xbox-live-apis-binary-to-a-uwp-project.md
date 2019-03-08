---
title: Xbox Live-APIs binäre zu einem UWP-Projekt hinzufügen
description: Erfahren Sie, wie Sie NuGet verwenden, um das Xbox Live-APIs binäre Paket Ihr UWP-Projekt hinzufügen.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593945"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>Hinzufügen von binärpaket für Xbox Live-APIs zu Ihr UWP-Projekt

## <a name="requirements"></a>Anforderungen

2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**. UWP-apps können mit Visual Studio 2015 Update 3 oder höher erstellt werden. Es wird empfohlen, dass Sie die neueste Version von Visual Studio für Entwickler und Sicherheitsupdates verwenden.
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** oder höher.

## <a name="add-the-binary-package-via-nuget"></a>Hinzufügen des binären über NuGet-Pakets

Um die Xbox Live-API aus Ihrem Projekt zu verwenden, können Sie Verweise auf die Binärdateien entweder durch Verwendung von NuGet-Paketen oder Hinzufügen der API-Quelle hinzufügen. Das Hinzufügen von NuGet-Paketen kann die Kompilierung erleichtern, während das Hinzufügen der Quelle das Debuggen vereinfacht. Dieser Artikel führt durch die Verwendung von NuGet-Pakete. Wenn Sie die Quelle verwenden möchten, sehen Sie [kompilieren das Xbox Live-APIs In Ihre UWP Quellprojekt](add-xbox-live-apis-source-to-a-uwp-project.md).

Die Xbox-API gehen Varianten für UWP und xdk-Version und für C++ und WinRT und ein Namespace als strukturiert **Microsoft.Xbox.Live.SDK.*. UWP** und **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** ist für Entwickler, die erstellen ein UWP-Spiels, die auf einem PC die Xbox One oder Windows Phone ausgeführt werden können.
2. **XboxOneXDK** ist für ID@Xbox und Entwickler, die die Xbox One XDK-verwaltet.
3. Das C++ SDK kann für C++ Spiele-Engines verwendet werden, während das WinRT-SDK ist für die Spiele-Engines, die mit C++, C#, oder JavaScript.
4. Wenn Sie WinRT mit einem C++-Modul verwenden, verwenden Sie C++ / CX Hüten (^) verwendet. C++ ist die empfohlene API für C++-Spiele-Engines verwendet wird.  

> [!TIP]
> Erfahren Sie mehr über das Ausführen von UWP für Xbox One an [erste Schritte mit UWP-app-Entwicklung auf der Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Sie können die Xbox Live SDK-NuGet-Pakets hinzufügen:

1. Wechseln Sie in Visual Studio zu **Tools** > **NuGet Package Manager** > **NuGet-Pakete für Projektmappe verwalten...** .
2. Klicken Sie in der NuGet-Paket-Manager auf **Durchsuchen** , und geben Sie **Xbox.Live.SDK** in das Suchfeld.
3. Wählen Sie die Version des Xbox Live SDK, das Sie aus der Liste auf der linken Seite verwenden möchten.
3. Klicken Sie auf der rechten Seite des Fensters, aktivieren Sie das Kontrollkästchen neben Ihrem Projekt, und klicken Sie auf **installieren**.

> [!NOTE]
> Xbox Live Creators-Programm-Entwickler müssen eine der UWP-Versionen des Xbox Live SDK verwenden, da xdk-Version nicht unterstützt wird.

![XBL über NuGet hinzufügen](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> Für `Microsoft.Xbox.Live.SDK.Cpp.*` -basierte Projekte, achten Sie darauf, den Header `#include <xsapi\services.h>` in Ihrem Projekt die Quelle.
