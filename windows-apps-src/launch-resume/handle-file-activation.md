---
title: Behandeln der Dateiaktivierung
description: Eine App kann als Standardhandler für einen bestimmten Dateityp registriert werden.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: eaf6118720ab77931decf93113a13341ab4f51d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642175"
---
# <a name="handle-file-activation"></a>Behandeln der Dateiaktivierung

**Wichtige APIs**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Ihre app kann registrieren, um der Standardhandler für einen bestimmten Dateityp zu werden. Sowohl Windows-Desktopanwendungen als auch UWP-Apps (Universelle Windows-Plattform) können als Standarddateihandler registriert werden. Wenn Ihre App vom Benutzer als Standardhandler für einen bestimmten Dateityp ausgewählt wird, wird sie dann aktiviert, wenn der Dateityp gestartet wird.

Wir empfehlen, die Registrierung für einen Dateityp nur durchzuführen, wenn davon auszugehen ist, dass alle entsprechenden Vorgänge für einen bestimmten Dateityp verarbeitet werden. Wenn der Dateityp von Ihrer App nur intern verwendet wird, ist die Registrierung eines Standardhandlers nicht erforderlich. Wenn Sie sich entscheiden, die Registrierung für eine bestimmten Dateityp durchzuführen, müssen Sie die entsprechenden Funktionen bereitstellen, die vom Benutzer bei der Aktivierung für diesen Dateityp erwartet werden. Beispielsweise kann eine Bildanzeige-App für das Anzeigen des Dateityps JPG registriert werden. Weitere Informationen zu Dateizuordnungen finden Sie unter [Richtlinien für Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Die folgenden Schritte zeigen, wie Sie den benutzerdefinierten Dateityp „.alsdk“ registrieren und Ihre App aktivieren, wenn der Benutzer eine ALSDK-Datei startet.

> **Beachten Sie**  In UWP-apps, bestimmte URIs und Dateierweiterungen für die Verwendung von integrierten apps und das Betriebssystem reserviert sind. Versuche, die App mit einem reservierten URI oder einer reservierten Dateierweiterung zu registrieren, werden ignoriert. Weitere Informationen finden Sie unter [Reservierte Datei- und URI-Schemanamen](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Schritt 1: Geben Sie den Erweiterungspunkt im Paketmanifest

Die App empfängt nur für die im Paketmanifest angegebenen Dateierweiterungen Aktivierungsereignisse. Im Folgenden wird beschrieben, wie Sie festlegen, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf „package.appxmanifest“, um den Manifest-Designer zu öffnen. Wählen Sie die Registerkarte **Deklarationen** und dann in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie dann auf **Hinzufügen**. Ausführliche Informationen zu Bezeichnern, die von Dateizuordnungen verwendet werden, finden Sie unter [ProgIDs](https://msdn.microsoft.com/library/windows/desktop/cc144152).

    Es folgt eine kurze Beschreibung der Felder, die Sie im Manifest-Designer ausfüllen können:

| Feld | Beschreibung |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Anzeigename** | Geben Sie den Anzeigenamen für eine Gruppe von Dateitypen an. Anhand des Anzeigenamens wird in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) der Dateityp identifiziert. |
| **Logo** | Geben Sie das Logo zur Identifikation des Dateityps auf dem Desktop in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) an. Wenn kein Logo angegeben wird, wird das kleine Logo der Anwendung verwendet. |
| **Infotipp** | Geben Sie den [Infotipp](https://msdn.microsoft.com/library/windows/desktop/cc144152) für eine Gruppe von Dateitypen an. Diese QuickInfo wird angezeigt, wenn Benutzer mit der Maus auf das Symbol für eine Datei dieses Typs zeigen. |
| **Name** | Wählen Sie einen Namen für eine Gruppe von Dateitypen aus, die identische Anzeigenamen, Logos, Infotipps und Bearbeitungsflags verwenden. Wählen Sie einen Gruppennamen aus, der auch nach App-Updates unverändert bleiben kann. **Hinweis**  Der Name darf nur aus Kleinbuchstaben bestehen. |
| **Inhaltstyp** | Geben Sie den MIME-Inhaltstyp, z. B. **image/jpeg**, für einen bestimmten Dateityp an. **Wichtiger Hinweis zu zulässigen Inhaltstypen:** Hier wird eine alphabetische Liste der MIME-Inhaltstypen, die Sie in das Paketmanifest eingeben können nicht, da sie entweder reserviert bzw. unzulässigen: **Anwendung/Force-Download**, **Application/Octet-Stream**, **Anwendung/unbekannt**, **Application/X-Msdownload**. |
| **Dateityp** | Geben Sie den Dateityp, für den die Registrierung durchgeführt werden soll, mit vorangestelltem Punkt an, z. B. „.jpeg“. **Reservierte und unzulässige Dateitypen:** Finden Sie unter [reservierte URI-Schema-Namen und Dateitypen](reserved-uri-scheme-names.md) für eine alphabetische Liste der Dateitypen für integrierte apps, die Sie für Ihre UWP-apps registrieren können, da diese reserviert nicht zulässig sind oder sind. |

2.  Geben Sie `alsdk` in das Feld **Name** ein.
3.  Geben Sie `.alsdk` als **Dateityp** ein.
4.  Geben Sie "Images\\Icon.png" als das Logo.
5.  Drücken Sie STRG+S, um die an „package.appxmanifest“ vorgenommenen Änderungen zu speichern.

Die obigen Schritte fügen dem Paketmanifest ein [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400)-Element wie das unten dargestellte hinzu. Die **windows.fileTypeAssociation**-Kategorie gibt an, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>Schritt 2: Fügen Sie die richtigen Symbole

Die Symbole von Apps, die für einen Dateityp zum Standard werden, werden an verschiedenen Stellen innerhalb des Systems angezeigt. Diese Symbole werden z. B. an folgenden Stellen angezeigt:

-   Anzeige der Elemente, Kontextmenüs, Menüband in Windows Explorer
-   Standardprogramme in der Systemsteuerung
-   Dateiauswahl
-   Suchergebnisse auf dem Startbildschirm

Fügen Sie ein 44 x 44-Symbol in das Projekt ein, damit Ihr Logo an diesen Positionen angezeigt werden kann. Stimmen Sie das Erscheinungsbild des Logos der App-Kachel ab, und verwenden Sie die Hintergrundfarbe der App, anstatt das Symbol transparent darzustellen. Erweitern Sie das Logo bis zum Rand, ohne eine Auffüllung vorzunehmen. Testen Sie Ihre Symbole auf weißem Hintergrund. Weitere Einzelheiten zu den Symbolen finden Sie unter [Richtlinien für Kacheln und Symbole](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="step-3-handle-the-activated-event"></a>Schritt 3: Das aktivierte Ereignis behandeln

Der [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)-Ereignishandler empfängt alle Dateiaktivierungsereignisse.

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> Wenn über den Vertrag der Datei gestartet wird, stellen Sie sicher, dass es sich bei, Schaltfläche "zurück" wird der Benutzer wieder auf den Bildschirm, der die app gestartet und nicht auf vorherigen Inhalt der app an.

Es wird empfohlen, die Erstellung einer neuen XAML **Frame** für jede Aktivierungsereignis, das eine neue Seite geöffnet wird. Auf diese Weise die Navigation Backstack-für den neuen XAML-Rahmen keine vorherigen Inhalte enthalten, die die app möglicherweise auf das aktuelle Fenster angehalten haben. Wenn Sie eine einzelne XAML verwenden **Frame** für den Start und für Datei-Verträge, dann sollten Sie löschen, die Seiten in der **Frame**des Journalnavigation, bevor Sie zu einer neuen Seite navigiert.

Wenn Ihre app über den Datei-Aktivierung gestartet wird, sollten Sie erwägen, einschließlich der Benutzeroberfläche, die dem Benutzer ermöglicht, auf die oberste Seite der app zurückkehren.

## <a name="remarks"></a>Hinweise

Die empfangenen Dateien stammen unter Umständen aus einer nicht vertrauenswürdigen Quelle. Wir empfehlen, den Inhalt einer Datei zu überprüfen, bevor Sie sie weiter verarbeiten. Weitere Informationen zur Eingabeüberprüfung finden Sie unter [Schreiben von sicherem Code](https://go.microsoft.com/fwlink/p/?LinkID=142053).

## <a name="related-topics"></a>Verwandte Themen

### <a name="complete-example"></a>Vollständiges Beispiel

* [Starten des Association-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=231484)

### <a name="concepts"></a>Konzepte

* [Standardprogramme](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Dateityp und Protokollmodell von Zuordnungen](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>Aufgaben

* [Starten der Standard-App für eine Datei](launch-the-default-app-for-a-file.md)
* [Behandeln der URI-Aktivierung](handle-uri-activation.md)

### <a name="guidelines"></a>Richtlinien

* [Richtlinien für die Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Verweis
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224716)
* [Windows.UI.Xaml.Application.OnFileActivated](https://msdn.microsoft.com/library/windows/apps/br242331)
