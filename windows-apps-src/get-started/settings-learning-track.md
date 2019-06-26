---
title: Speichern und Laden von Einstellungen in einer UWP-App
description: Erfahren Sie, wie Sie App-Einstellungen in Apps für die universelle Windows-Plattform speichern und laden.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Einstellungen, Einstellungen speichern, Einstellungen laden
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 490dd8f0f3841fae089626ec9c283d54cc0d8cd9
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66370496"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Speichern und Laden von Einstellungen in einer UWP-App

In diesem Thema erfahren Sie, was Sie wissen müssen, um mit dem Laden und Speichern von Einstellungen in einer App für die universelle Windows-Plattform (UWP) zu beginnen. Es werden die wichtigsten APIs vorgestellt, und Sie erhalten Links zu weiteren Informationen.

Verwenden Sie Einstellungen, um die vom Benutzer anpassbaren Aspekte Ihrer App zu speichern. Beispielsweise könnte ein Benutzer in einem Newsreader mithilfe von App-Einstellungen speichern, welche Nachrichtenquellen angezeigt werden sollen und welche Schriftart zum Lesen von Artikeln verwendet werden soll.

Wir betrachten Code zum Speichern und Laden von App-Einstellungen, einschließlich lokaler und Roamingeinstellungen.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Verwenden Sie App-Einstellungen, um Konfigurationsdaten wie Benutzereinstellungen und den App-Status zu speichern.  Gerätespezifische Einstellungen werden lokal gespeichert. Einstellungen für das jeweilige Gerät, auf dem Ihre App installiert ist, werden im Roaming-Datenspeicher gespeichert. Die Einstellungen werden zwischen Geräten synchronisiert, auf denen der Benutzer mit demselben Microsoft-Konto angemeldet ist und auf denen dieselbe Version der App installiert ist.

Die folgenden Datentypen können für Einstellungen verwendet werden: Integer, Double, Float, Char, String, Point, DateTime u. a. Sie können auch Instanzen der [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)-Klasse speichern, was nützlich ist, wenn mehrere Einstellungen als Einheit behandelt werden sollen. Beispielsweise sollten der Schriftartname und Schriftgrad für den Text im Lesebereich der App als eine Einheit gespeichert/wiederhergestellt werden. Dadurch wird verhindert, dass Einstellungen nicht mehr synchron sind, falls Einstellungen beim Roaming unterschiedlich schnell übermittelt werden.

Hier die wichtigsten APIs, die Sie zum Speichern oder Laden von App-Einstellungen benötigen:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) ruft den Container für Anwendungseinstellungen aus dem lokalen App-Datenspeicher ab. Speichern Sie darin Einstellungen, die nicht für das Roaming zwischen Geräten geeignet sind, da sie einen gerätespezifischen Status darstellen oder zu groß sind.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) ruft den Container für Anwendungseinstellungen aus dem Roaming-App-Datenspeicher ab. Diese Daten werden zwischen Geräten ausgetauscht.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) ist ein Container, der App-Einstellungen als Schlüssel-Wert-Paare darstellt. Verwenden Sie diese Klasse zum Erstellen und Abrufen von Einstellungswerten.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) stellt mehrere App-Einstellungen dar, die als Einheit serialisiert werden sollten. Dies ist nützlich, wenn eine Einstellung nicht unabhängig von einer anderen aktualisiert werden soll.

## <a name="save-app-settings"></a>Speichern von App-Einstellungen

In dieser Einführung konzentrieren wir uns auf zwei einfache Szenarien: lokales Speichern und Laden einer App-Einstellung und Roaming einer aus Schriftart und Schriftgrad zusammengesetzten Einstellung zwischen Geräten.

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

Speichern Sie eine Einstellung auf dem lokalen Gerät, indem Sie zuerst mit `Windows.Storage.ApplicationData.Current.LocalSettings` einen **ApplicationDataContainer** für den lokalen Einstellungsdatenspeicher abrufen. Schlüssel-Wert-Wörterbuchpaare, die Sie dieser Instanz zuweisen, werden im lokalen Datenspeicher für Geräteeinstellungen gespeichert.

Roamingeinstellungen werden nach einem ähnlichen Muster gespeichert. Rufen Sie zunächst mit `Windows.Storage.ApplicationData.Current.RoamingSettings` einen **ApplicationDataContainer** für den Roaming-Einstellungsdatenspeicher ab. Weisen Sie dieser Instanz dann Schlüssel-Wert-Paare zu.  Die Schlüssel-Wert-Paare werden automatisch zwischen Geräten übertragen (Roaming).

Im obigen Codeausschnitt werden in **ApplicationDataCompositeValue** mehrere Schlüssel-Wert-Paare gespeichert. Zusammengesetzte Werte sind nützlich, um sicherzustellen, dass mehrere zusammengehörende Einstellungen synchron bleiben. Beim Speichern von **ApplicationDataCompositeValue** werden die Werte als Einheit bzw. unteilbar gespeichert und geladen. Auf diese Weise bleiben zusammengehörende Einstellungen synchron, da sie beim Roaming als Einheit und nicht einzeln übertragen werden.

## <a name="load-app-settings"></a>Laden von App-Einstellungen

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

Laden Sie eine Einstellung vom lokalen Gerät, indem Sie zuerst mit `Windows.Storage.ApplicationData.Current.LocalSettings` eine **ApplicationDataContainer**-Instanz für den lokalen Einstellungsdatenspeicher abrufen. Verwenden Sie diese dann zum Abrufen von Schlüssel-Wert-Paaren.

Roamingeinstellungen werden nach einem ähnlichen Muster geladen. Rufen Sie zunächst mit `Windows.Storage.ApplicationData.Current.RoamingSettings` eine **ApplicationDataContainer**-Instanz aus dem Roaming-Einstellungsdatenspeicher ab. Greifen Sie auf Schlüssel-Wert-Paare dieser Instanz zu. Wenn die Daten noch nicht per Roaming auf das Gerät übertragen wurden, auf dem Sie auf die Einstellungen zugreifen, erhalten Sie einen **ApplicationDataContainer** mit dem Wert „null”. Daher enthält der obige Beispielcode die Überprüfung `if (composite != null)`.

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung der APIs und anderer nützlicher Dokumentation, die Sie beim Speichern und Laden von App-Einstellungen unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [ApplicationData.LocalSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) | Ruft den Container der Anwendungseinstellungen aus dem lokalen App-Datenspeicher ab. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Ruft den Container der Anwendungseinstellungen aus dem Roaming-App-Datenspeicher ab. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Ein Container für App-Einstellungen, der das Erstellen, Löschen, Aufzählen und Durchlaufen der Containerhierarchie unterstützt. |
| [Windows.UI.ApplicationSettings Namespace](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Stellt Klassen bereit, mit denen Sie die App-Einstellungen definieren, die im Einstellungsbereich der Windows-Shell angezeigt werden. |

### <a name="useful-docs"></a>Nützliche Dokumentation

| Thema | Beschreibung |
|-------|----------------|
| [Richtlinien für App-Einstellungen](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Erläutert bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen. |
| [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Exemplarische Vorgehensweise zum Speichern und Abrufen von Einstellungen, einschließlich Roamingeinstellungen. |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Beispiel für Anwendungsdaten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | Die Szenarien 2–4 legen den Schwerpunkt auf Einstellungen. |
