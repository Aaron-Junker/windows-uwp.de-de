---
title: Starten von Ausschnitt und Skizze
description: In diesem Thema werden die URI-Schemas MS-screenclip und MS-screensketch beschrieben. Ihre APP kann diese URI-Schemas verwenden, um den Snip-& Sketch-APP zu starten oder einen neuen Snip zu öffnen.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP, Uri, Snip, Sketch
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d9469dd6efd3598ab7abd9791a976385f4dfce49
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684665"
---
# <a name="launch-screen-snipping"></a>Starten von Ausschnitt und Skizze

Mit den Schemas " **MS-screenclip:** " und " **MS-screensketch:** URI" können Sie Screenshots auslösen oder Screenshots bearbeiten.

## <a name="open-a-new-snip-from-your-app"></a>Öffnen eines neuen Ausschnitt in Ihrer APP

Der **MS-screenclip:** URI ermöglicht der APP das automatische Öffnen und Starten eines neuen Snip. Das resultierende Ausschnitt wird in die Zwischenablage des Benutzers kopiert, aber nicht automatisch an die öffnende App zurückgegeben.

**MS-screenclip:** übernimmt die folgenden Parameter:

| Parameter | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| Quelle | String | Nein | Eine frei Form Zeichenfolge, die die Quelle angibt, die den URI gestartet hat. |
| delayInSeconds | int | Nein | Ein ganzzahliger Wert zwischen 1 und 30. Gibt die Verzögerung zwischen dem URI-Befehl und dem Beginn der Ermittlung in vollen Sekunden an. |
| callbackformat | String | Nein | Dieser Parameter ist nicht verfügbar. |

## <a name="launching-the-snip--sketch-app"></a>Starten der Snip-& Sketch-App

Der **MS-screensketch:** -URI ermöglicht das programmgesteuerte Starten der Snip-& Sketch-APP und das Öffnen eines bestimmten Bilds in der APP für die Anmerkung.

**MS-screensketch:** übernimmt die folgenden Parameter:

| Parameter | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| sharedaccesstoken | String | Nein | Ein Token, das die Datei identifiziert, die in der Snip-& Sketch-app geöffnet werden soll. Aus " [sharedstorageaccessmanager. AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)" abgerufen. Wenn dieser Parameter ausgelassen wird, wird die APP gestartet, ohne dass eine Datei geöffnet ist. |
| secondarysharedaccesstoken | String | Nein | Eine Zeichenfolge, die eine JSON-Datei mit Metadaten zum Snip identifiziert. Die Metadaten können ein **clippoints** -Feld mit einem Array von x, y-Koordinaten und/oder einer [useractivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)enthalten. |
| Quelle | String | Nein | Eine frei Form Zeichenfolge, die die Quelle angibt, die den URI gestartet hat. |
| IsTemporary | bool | Nein | Wenn diese Einstellung auf "true" festgelegt ist, versucht die Bildschirm Skizze, die Datei nach dem Öffnen zu löschen. |

Im folgenden Beispiel wird die [launchuriasync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) -Methode aufgerufen, um ein Bild an den Snip-& Sketch aus der APP des Benutzers zu senden.

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
