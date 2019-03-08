---
title: Schema „ms-tonepicker“
description: In diesem Thema wird das URI-Schema „ms-tonepicker“ beschrieben und wie Sie dieses verwenden können, um eine Tonauswahl anzuzeigen, Töne auszuwählen und zu speichern sowie den Anzeigenamen für Töne abzurufen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 293c755ecaf81ce80fab148a8aca92a7e3a8fa48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618585"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>Wählen und Speichern von Tönen mithilfe des URI-Schemas „ms-tonepicker“

In diesem Thema wird beschrieben, wie Sie mit der **ms-Tonepicker:** URI-Schema. Dieses URI-Schema kann verwendet werden, um:
- Zu ermitteln, ob die Tonauswahl auf dem Gerät verfügbar ist;
- Die Tonauswahl anzuzeigen, um die verfügbaren Klingeltöne, Systemtöne, SMS-Klingeltöne und Alarmtöne aufzulisten, und ein Tontoken zu erhalten, das den vom Benutzer ausgewählten Ton darstellt;
- Das Tool zum von Tönen anzuzeigen, das ein Sounddateitoken als Eingabe erhält und es auf dem Gerät speichert. Gespeicherte Töne stehen anschließend über die Tonauswahl zur Verfügung. Benutzer können dem Ton einen Anzeigenamen zuweisen.
- Konvertieren Sie ein Tontoken in dessen Anzeigenamen.

## <a name="ms-tonepicker-uri-scheme-reference"></a>ms-tonepicker: URI-Schema-Verweis

Dieses URI-Schema übergibt keine Argumente über die URI-Schema-Zeichenfolge, sondern über einen [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx). Alle Zeichenfolgen beachten die Groß- und Kleinschreibung.

In den folgenden Abschnitten wird angegeben, welche Argumente übergeben werden müssen, um die angegebene Aufgabe auszuführen.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>Aufgabe: Bestimmen Sie, ob die Auswahl der Ton auf dem Gerät verfügbar ist
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>Aufgabe: Anzeigen der Ton-Auswahl

Die Argumente, die Sie zum Anzeigen der Tonauswahl übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Aktion | string | Ja | "PickRingtone" | Öffnet die Tonauswahl. |
| CurrentToneFilePath | string | Nein | Ein vorhandenes Tontoken. | Der Ton, der in der Tonauswahl als aktueller Ton angezeigt werden soll. Wenn dieser Wert nicht festgelegt ist, ist der erste Ton in der Liste standardmäßig aktiviert.<br>Streng genommen, ist dies kein Dateipfad. Sie können einen geeigneten Wert für `CurrenttoneFilePath` aus dem Wert `ToneToken` erhalten, der von der Tonauswahl zurückgegeben wird.  |
| TypeFilter | string | Nein | "Ringtones", "Notifications", "Alarms", "None" | Wählt die Töne aus, die der Auswahl hinzugefügt werden sollen. Wenn kein Filter angegeben ist, werden alle Töne angezeigt. |

Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewerte | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Erfolg. <br>1 – Abgebrochen. <br>7 – Ungültige Parameter. <br>8 – Es gibt keine Töne, die den Filterkriterien entsprechen. <br>255 – Die angegebene Aktion ist nicht implementiert. | Das Ergebnis des Auswahlvorgangs. |
| ToneToken | string | Das Token des ausgewählten Tons. <br>Die Zeichenfolge ist leer, wenn der Benutzer in der Auswahl **Standard** auswählt. | Dieses Token kann in einer Popupbenachrichtigungs-Nutzlast verwendet werden oder einem Kontakt als Klingelton oder SMS-Klingelton zugewiesen werden. Der Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |
| DisplayName | string | Der Anzeigename des angegebenen Tons. | Eine Zeichenfolge, die dem Benutzer angezeigt werden kann, um den ausgewählten Ton darzustellen. Der Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |


**Beispiel: Öffnen Sie die Auswahl der Ton, damit der Benutzer einen Ton auswählen können**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>Aufgabe: Anzeigen der Ton Bildschirmschoner

Die Argumente, die Sie zum Anzeigen des Tools für das Speichern von Tönen übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Aktion | string | Ja | "SaveRingtone" | Öffnet die Auswahl, um einen Klingelton zu speichern. |
| ToneFileSharingToken | string | Ja | [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx)-Dateifreigabetoken für die Klingeltondatei, die gespeichert werden soll. | Speichert eine bestimmte Audiodatei als Klingelton. Die unterstützten Inhaltstypen für die Datei sind „mpeg audio“ und „x-ms-wma audio“. |
| DisplayName | string | Nein | Der Anzeigename des angegebenen Tons. | Legt den Anzeigenamen fest, der beim Speichern des angegebenen Klingeltons verwendet werden soll. |

Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewerte | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Erfolg.<br>1 – Vom Benutzer abgebrochen.<br>2 – Ungültige Datei.<br>3 – Ungültiger Inhaltstyp.<br>4 – Datei überschreitet die maximal zulässige Größe für Klingeltöne (1 MB in Windows 10).<br>5 – Datei überschreitet die Begrenzung auf 40 Sekunden.<br>6 – Datei wird durch Digital Rights Management geschützt.<br>7 – Ungültige Parameter. | Das Ergebnis des Auswahlvorgangs. |

**Beispiel: Speichern einer lokalen Musikdatei als einen Klingelton**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>Aufgabe: Konvertieren Sie ein Token Ton in seinen Anzeigenamen

Die Argumente, die Sie zum Abrufen des Anzeigenamens eines Tons übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Aktion | string | Ja | "GetToneName" | Gibt an, dass Sie den Anzeigenamen eines Tons abrufen möchten. |
| ToneToken | string | Ja | Das Tontoken. | Das Tontoken, aus dem ein Anzeigename abgerufen werden soll. |

Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewert | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Der Auswahlvorgang war erfolgreich.<br>7 – Falscher Parameter (wenn beispielsweise kein Tontoken angegeben wurde).<br>9 – Fehler beim Lesen des Namens für das angegebene Token.<br>10 – Das angegebene Tontoken kann nicht gefunden werden. | Das Ergebnis des Auswahlvorgangs.
| DisplayName | string | Der Anzeigename des Tons. | Gibt den Anzeigenamen des ausgewählten Tons zurück. Dieser Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |

**Beispiel: Abrufen eines Tokens für die Ton aus Contact.RingToneToken und seinen Anzeigenamen in die Kontaktkarte anzuzeigen.**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```
