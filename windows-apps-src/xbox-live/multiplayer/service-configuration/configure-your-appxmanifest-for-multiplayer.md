---
title: Konfigurieren von Ihrem AppXManifest für Multiplayer-Spiele
description: Informationen Sie zum Konfigurieren Ihrer UWP-AppXManifest um Multiplayer-Einladungen Xbox Live zu aktivieren.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, protokollaktivierung, Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646025"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>Konfigurieren von Ihrem AppXManifest für Multiplayer-Spiele

Sie müssen einige Updates in die appxmanifest-Datei in Visual Studio-Projekt zu machen, wenn die folgenden Bedingungen erfüllt sind:
- Entwickeln Sie eine UWP
- Sie möchten die Möglichkeit, dass der Spieler, um andere Benutzer einladen, Ihre Titel implementieren

Wenn Sie diesen Schritt nicht tun, klicken Sie dann erhalten Titel des keine Protokoll aktiviert, wenn ein Player Empfänger eine Einladung zur Wiedergabe akzeptiert.

## <a name="open-your-packageappxmanifest"></a>Öffnen Sie Ihr "Package.appxmanifest"

Die Datei "Package.appxmanifest" befindet sich in der Regel im gleichen Verzeichnis wie die Visual Studio-Projekt-Projektmappendatei.  Oder Sie finden sie im Projektmappen-Explorer.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>Neuen Eintrag hinzufügen

Sie benötigen Folgendes zum Hinzufügen der ```<Extensions>``` Element unter ```<Applications>``` in der Datei "Package.appxmanifest"

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Beispiel:

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

Speichern und dem Titel des neu zu erstellen.  Um zu erfahren, wie Sie den Multiplayer-Manager verwenden, um die Möglichkeit, laden Sie die Spieler, um den Titel zu implementieren, finden Sie unter [Multiplayer-Spiele mit Freunden spielen](../multiplayer-manager/play-multiplayer-with-friends.md)
