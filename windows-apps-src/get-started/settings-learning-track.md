---
title: Speichern und Laden von Einstellungen in einer UWP-App
description: Erfahren Sie, wie Sie App-Einstellungen in Apps für die Universelle Windows-Plattform speichern und laden.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Einstellungen, Einstellungen speichern, Einstellungen laden
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4aa56bf24d2dfa1fd4ae1947a5b0edf7f312ea2f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614635"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Speichern und Laden von Einstellungen in einer UWP-App

In diesem Thema erfahren Sie, was Sie wissen müssen, um mit dem Laden und Speichern von Einstellungen in einer App für die Universelle Windows-Plattform (UWP) zu beginnen. Es werden die wichtigsten APIs vorgestellt, und Sie erhalten Links zu weiteren Informationen.

Verwenden Sie Einstellungen, um die vom Benutzer anpassbaren Aspekte Ihrer App zu realisieren. Beispielsweise könnten Einstellungen in einer Nachrichten-App festlegen, welche Nachrichtenquellen angezeigt werden und welche Schriftart die App zum Lesen von Artikeln verwenden soll.

Betrachten wir Code zum Speichern und Laden von Appeinstellungen, einschließlich der lokalen und Roamingeinstellungen.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Verwenden Sie App-Einstellungen, um Konfigurationsdaten wie Benutzereinstellungen und den App-Status zu speichern.  Gerätespezifische Einstellungen werden lokal gespeichert. Einstellungen für das jeweilige Gerät, auf dem Ihre App installiert ist, werden im Roaming-Datenspeicher gespeichert. Die Einstellungen werden zwischen Geräten synchronisiert, auf denen der Benutzer mit demselben Microsoft-Konto angemeldet ist und auf denen dieselbe Version der App installiert ist.

Die folgenden Datentypen können für Einstellungen verwendet werden: Integer, Double, Float, Chars, Strings, Points, DateTimes u. a. Sie können auch Instanzen der Klasse [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) speichern, was nützlich ist, wenn mehrere Einstellungen vorhanden sind, die als Einheit behandelt werden sollen. Beispielsweise sollten Schriftname und Schriftgröße für den Text im Lesebereich der App als Einheit gespeichert/abgerufen werden. Dadurch wird verhindert, dass Einstellungen nicht mehr miteinander synchron sind, falls Einstellungen beim Roaming unterschiedlich schnell übermittelt werden.

Hier die wichtigsten APIs, die Sie zum Speichern oder Laden von App-Einstellungen benötigen:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) ruft den Container für Anwendungseinstellungen aus dem lokalen App-Datenspeicher ab. Speichern Sie darin Einstellungen, die nicht für das Roaming zwischen Geräten geeignet sind, da sie gerätespezifisch oder zu groß sind.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) ruft den Container für Anwendungseinstellungen aus dem Roaming-App-Datenspeicher ab. Diese Daten werden zwischen Geräten ausgetauscht.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) ist ein Container, der App-Einstellungen als Schlüssel/Wert-Paare darstellt. Verwenden Sie diese Klasse zum Erstellen und Abrufen von Werten.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) stellt mehrere App-Einstellungen dar, die als Einheit serialisiert werden sollten. Dies ist nützlich, wenn eine Einstellung nicht unabhängig von einer anderen aktualisiert werden soll.

## <a name="save-app-settings"></a>Speichern von App-Einstellungen

In dieser Einführung werden wir uns auf zwei einfache Szenarien konzentrieren: Speichern und Laden einer App-Einstellung lokal, und Übertragen einer aus Schrift und Schriftgröße zusammengesetzten Einstellung zwischen Geräten.

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

Speichern Sie eine Einstellung auf dem lokalen Gerät, indem Sie zuerst mit `Windows.Storage.ApplicationData.Current.LocalSettings` einen **ApplicationDataContainer** für den lokalen Einstellungsdatenspeicher abrufen. Schlüssel/Wert-Paare, die Sie dieser Instanz zuweisen, werden im lokalen Datenspeicher für Geräteeinstellungen gespeichert.

Eine Roamingeinstellung speichern Sie nach einem ähnlichen Muster. Rufen Sie zunächst mit `Windows.Storage.ApplicationData.Current.RoamingSettings` einen **ApplicationDataContainer** für den Datenspeicher für Roamingeinstellungen ab. Dann weisen Sie dieser Instanz Schlüssel/Wert-Paare zu.  Die Schlüssel/Wert-Paare werden automatisch zwischen Geräten übertragen (Roaming).

Im obigen Codeausschnitt speichert ein **ApplicationDataCompositeValue** mehrere Schlüssel/Wert-Paare. Zusammengesetzte Werte sind nützlich, um sicherzustellen, dass mehrere zusammengehörende Einstellungen synchronisiert bleiben. Beim Speichern eines **ApplicationDataCompositeValue** werden die Werte als Einheit (atomar) gespeichert und können auch so geladen werden. Auf diese Weise bleiben zusammengehörende Einstellungen synchron, da sie beim Roaming als Einheit und nicht einzeln übertragen werden.

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

Laden Sie eine Einstellung aus dem lokalen Gerät, indem Sie zuerst mit `Windows.Storage.ApplicationData.Current.LocalSettings` eine **ApplicationDataContainer**-Instanz für den lokalen Einstellungsdatenspeicher abrufen. Verwenden Sie diese dann zum Abrufen von Schlüssel/Wert-Paaren.

Eine Roamingeinstellung laden Sie nach einem ähnlichen Muster. Rufen Sie zunächst mit `Windows.Storage.ApplicationData.Current.RoamingSettings` eine **ApplicationDataContainer**-Instanz für den Datenspeicher für Roamingeinstellungen ab. Greifen Sie auf Schlüssel/Wert-Paare dieser Instanz zu. Wenn die Daten noch nicht zu dem Gerät gelangt sind, von dem aus Sie auf die Einstellungen zugreifen, erhalten Sie einen **ApplicationDataContainer** mit dem Wert „null”. Daher enthält der obige Beispielcode die Prüfung `if (composite != null)`.

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung der APIs und anderer nützlicher Dokumentation, die Sie beim Speichern und Laden von App-Einstellungen unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | Ruft den Container der Anwendungseinstellungen aus dem lokalen App-Datenspeicher ab. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Ruft den Container der Anwendungseinstellungen aus dem Roaming-App-Datenspeicher ab. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Ein Container für App-Einstellungen, der das Erstellen, Löschen, Aufzählen und Durchlaufen der Containerhierarchie unterstützt. |
| [Windows.UI.ApplicationSettings Namespace](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Stellt Klassen bereit, mit denen Sie die App-Einstellungen definieren, die im Einstellungsbereich der Windows-Shell angezeigt werden. |

### <a name="useful-docs"></a>Nützliche Dokumente

| Thema | Beschreibung |
|-------|----------------|
| [Richtlinien zum app-Einstellungen](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Erläutert bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen. |
| [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Exemplarische Vorgehensweise zum Speichern und Abrufen von Einstellungen, einschließlich der Roamingeinstellungen. |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Anwendungsbeispiel für Daten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | Szenarien 2-4 mit Fokus auf Einstellungen |
