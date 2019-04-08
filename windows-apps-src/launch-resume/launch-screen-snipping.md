---
title: Ausschnitt & Skizze starten
description: In diesem Thema wird beschrieben, die ms-Screenclip und ms-Screensketch URI-Schemas. Ihre app kann diese URI-Schemas verwenden, die folgende & Sketch-app zu starten oder einen neuen Ausschnitt zu öffnen.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10 "," Uwp "," Uri "," Snip "," Skizze
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595385"
---
# <a name="launch-screen-snipping"></a>Ausschnitt & Skizze starten

Die **ms-Screenclip:** und **ms-Screensketch:** URI-Schemas ermöglicht Ihnen, snipping oder Bearbeiten von Screenshots zu initiieren.

## <a name="open-a-new-snip-from-your-app"></a>Öffnen Sie einen neuen Ausschnitt aus Ihrer app

Die **ms-Screenclip:** -URI kann Ihre app automatisch starten und einen neuen Ausschnitt an. Die resultierende Ausschnitt in die Zwischenablage des Benutzers kopiert wird, aber wird nicht automatisch wieder an das Öffnen der app übergeben.

**MS-Screenclip:** enthält die folgenden Parameter:

| Parameter | Typ | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| Quelle | string | Nein | Eine Freihandform-Zeichenfolge an der Quelle, die den URI gestartet. |
| "delayinseconds" | int | Nein | Ein ganzzahliger Wert zwischen 1 und 30. Gibt die Verzögerung in vollständige Sekunden zwischen dem URI und wenn snipping beginnt. |
| callbackformat | string | Nein | Dieser Parameter ist nicht verfügbar. |

## <a name="launching-the-snip--sketch-app"></a>Starten die folgende & Sketch-App

Die **ms-Screensketch:** URI können Sie programmgesteuert starten Sie die folgende & Sketch-app, und öffnen ein bestimmtes Image, in der app für die Anmerkung.

**MS-Screensketch:** enthält die folgenden Parameter:

| Parameter | Typ | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| sharedAccessToken | string | Nein | Ein Token, identifizieren die Datei, in der folgende & Sketch-app geöffnet. Abgerufen aus [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Wenn dieser Parameter ausgelassen wird, wird die app ohne einer geöffneten Datei gestartet werden. |
| secondarySharedAccessToken | string | Nein | Eine Zeichenfolge, die eine JSON-Datei mit Metadaten über den Ausschnitt identifizieren. Die Metadaten enthalten möglicherweise eine **ClipPoints** Feld mit einem Array von X, y-Koordinaten, und/oder einen [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| Quelle | string | Nein | Eine Freihandform-Zeichenfolge an der Quelle, die den URI gestartet. |
| IsTemporary | bool | Nein | Wenn auf True festgelegt, Bildschirm Sketch versucht, die nach dem Öffnen sie die Datei zu löschen. |

Im folgenden Beispiel wird die [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) Methode, um ein Bild an folgende & Sketch aus der app des Benutzers zu senden.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

Das folgende Beispiel zeigt, welche eine Datei, die gemäß der **SecondarySharedAccessToken** Parameter **ms-Screensketch** enthalten kann:

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
