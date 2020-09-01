---
title: Starten der Microsoft Store-App
description: In diesem Thema wird das URI-Schema „ms-windows-store“ beschrieben. Ihre APP kann dieses URI-Schema verwenden, um die Microsoft Store-App auf bestimmten Seiten im Speicher zu starten.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a31c762c002e711a87e99e2f97de6c26e2c8b48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172984"
---
# <a name="launch-the-microsoft-store-app"></a>Starten der Microsoft Store-App



In diesem Thema wird das **ms-windows-store:**-URI-Schema beschrieben. Ihre APP kann dieses URI-Schema verwenden, um die Microsoft Store-App auf bestimmten Seiten im Speicher mit der [**launchuriasync**](/uwp/api/windows.system.launcher.launchuriasync) -Methode zu starten.

Dieses Beispiel zeigt, wie der Store auf der Seite Games geöffnet wird:

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-windows-store: URI-Schemaverweis

<table>
<tr><th>BESCHREIBUNG</th><th></th><th>URI-Schema</th></tr>
<tr><td>Startet die Startseite des Store.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Startet eine der obersten Ebenen im Store.<p>Hinweis: Nicht alle Benutzer haben Zugriff auf allen Ebenen.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Startet die Seite mit Produktdetails für ein Produkt. <p>Store-ID wird für Kunden mit Windows 10 empfohlen und funktioniert für alle Betriebssystemversionen. Die früheren Verfahren hierfür (Beispiel: PFN) werden jedoch weiterhin unterstützt.</p>
<p>Diese Werte finden Sie im <a href="https://partner.microsoft.com/dashboard">Partner Center</a> auf der Seite <a href="https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details">App-Identität</a> im Abschnitt App-Verwaltung für jede APP.</p>
</td>
<td>
Store-ID <p>(Empfohlen)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>Paketfamilienname (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Produkt-ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>Produkt-ID (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Startet das Schreiben einer Bewertung für ein Produkt.</td>
<td>Store-ID <p>(Empfohlen)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>Paketfamilienname (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Produkt-ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>Produkt-ID (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die einer Dateierweiterung zugeordnet sind. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die einem Protokoll zugeordnet sind.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die mindestens einem Tag zugeordnet sind. Tags müssen durch Kommas getrennt werden.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
Startet eine Suche für die angegebene Abfrage. Leerzeichen in der Abfrage sind zulässig.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten in einer Kategorie.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten eines angegebenen Herausgebers. Leerzeichen in der Abfrage sind zulässig.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>Startet die Downloads- und Updateseite.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>Startet die Store-Einstellungsseite.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 