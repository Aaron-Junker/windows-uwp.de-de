---
title: Starten von Ausschnitt und Skizze
description: In diesem Thema werden die URI-Schemas MS-screenclip und MS-screensketch beschrieben. Ihre APP kann diese URI-Schemas verwenden, um den Snip-& Sketch-APP zu starten oder einen neuen Snip zu öffnen.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP, Uri, Snip, Sketch
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2d7471f414922eb1e4923079082ee6abfd8418bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167814"
---
# <a name="launch-screen-snipping"></a>Starten von Ausschnitt und Skizze

Mit den Schemas " **MS-screenclip:** " und " **MS-screensketch:** URI" können Sie Screenshots auslösen oder Screenshots bearbeiten.

## <a name="open-a-new-snip-from-your-app"></a>Öffnen eines neuen Ausschnitt in Ihrer APP

Der **MS-screenclip:** URI ermöglicht der APP das automatische Öffnen und Starten eines neuen Snip. Das resultierende Ausschnitt wird in die Zwischenablage des Benutzers kopiert, aber nicht automatisch an die öffnende App zurückgegeben.

**MS-screenclip:** übernimmt die folgenden Parameter:

| Parameter | Typ | Erforderlich | BESCHREIBUNG |
| --- | --- | --- | --- |
| source | Zeichenfolge | nein | Eine frei Form Zeichenfolge, die die Quelle angibt, die den URI gestartet hat. |
| delayInSeconds | INT | Nein | Ein ganzzahliger Wert zwischen 1 und 30. Gibt die Verzögerung zwischen dem URI-Befehl und dem Beginn der Ermittlung in vollen Sekunden an. |
| callbackformat | Zeichenfolge | nein | Dieser Parameter ist nicht verfügbar. |

## <a name="launching-the-snip--sketch-app"></a>Starten der Snip-& Sketch-App

Der **MS-screensketch:** -URI ermöglicht das programmgesteuerte Starten der Snip-& Sketch-APP und das Öffnen eines bestimmten Bilds in der APP für die Anmerkung.

**MS-screensketch:** übernimmt die folgenden Parameter:

| Parameter | Typ | Erforderlich | BESCHREIBUNG |
| --- | --- | --- | --- |
| sharedaccesstoken | Zeichenfolge | nein | Ein Token, das die Datei identifiziert, die in der Snip-& Sketch-app geöffnet werden soll. Aus " [sharedstorageaccessmanager. AddFile](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)" abgerufen. Wenn dieser Parameter ausgelassen wird, wird die APP gestartet, ohne dass eine Datei geöffnet ist. |
| secondarysharedaccesstoken | Zeichenfolge | nein | Eine Zeichenfolge, die eine JSON-Datei mit Metadaten zum Snip identifiziert. Die Metadaten können ein **clippoints** -Feld mit einem Array von x, y-Koordinaten und/oder einer [useractivity](/uwp/api/windows.applicationmodel.useractivities.useractivity)enthalten. |
| source | Zeichenfolge | nein | Eine frei Form Zeichenfolge, die die Quelle angibt, die den URI gestartet hat. |
| IsTemporary | bool | Nein | Wenn diese Einstellung auf "true" festgelegt ist, versucht die Bildschirm Skizze, die Datei nach dem Öffnen zu löschen. |

Im folgenden Beispiel wird die [launchuriasync](/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) -Methode aufgerufen, um ein Bild an den Snip-& Sketch aus der APP des Benutzers zu senden.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

Im folgenden Beispiel wird veranschaulicht, wie eine durch den **secondarysharedaccesstoken** -Parameter von **MS-screensketch** angegebene Datei Folgendes enthalten kann:

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```