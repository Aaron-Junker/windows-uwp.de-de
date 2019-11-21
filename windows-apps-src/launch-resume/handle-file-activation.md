---
title: Behandeln der Dateiaktivierung
description: Eine App kann als Standardhandler für einen bestimmten Dateityp registriert werden.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 079746d3c1619fe940ba243410f0247b7b850ed9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259461"
---
# <a name="handle-file-activation"></a>Behandeln der Dateiaktivierung

**Wichtige APIs**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

Your app can register to become the default handler for a certain file type. Sowohl Windows-Desktopanwendungen als auch UWP-Apps (Universelle Windows-Plattform) können als Standarddateihandler registriert werden. Wenn Ihre App vom Benutzer als Standardhandler für einen bestimmten Dateityp ausgewählt wird, wird sie dann aktiviert, wenn der Dateityp gestartet wird.

Wir empfehlen, die Registrierung für einen Dateityp nur durchzuführen, wenn davon auszugehen ist, dass alle entsprechenden Vorgänge für einen bestimmten Dateityp verarbeitet werden. Wenn der Dateityp von Ihrer App nur intern verwendet wird, ist die Registrierung eines Standardhandlers nicht erforderlich. Wenn Sie sich entscheiden, die Registrierung für eine bestimmten Dateityp durchzuführen, müssen Sie die entsprechenden Funktionen bereitstellen, die vom Benutzer bei der Aktivierung für diesen Dateityp erwartet werden. Beispielsweise kann eine Bildanzeige-App für das Anzeigen des Dateityps JPG registriert werden. Weitere Informationen zu Dateizuordnungen finden Sie unter [Richtlinien für Dateitypen und URIs](https://docs.microsoft.com/windows/uwp/files/index).

Die folgenden Schritte zeigen, wie Sie den benutzerdefinierten Dateityp „.alsdk“ registrieren und Ihre App aktivieren, wenn der Benutzer eine ALSDK-Datei startet.

> **Note**  In UWP apps, certain URIs and file extensions are reserved for use by built-in apps and the operating system. Versuche, die App mit einem reservierten URI oder einer reservierten Dateierweiterung zu registrieren, werden ignoriert. Weitere Informationen finden Sie unter [Reservierte Datei- und URI-Schemanamen](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Schritt 1: Angeben des Erweiterungspunkts im Paketmanifest

Die App empfängt nur für die im Paketmanifest angegebenen Dateierweiterungen Aktivierungsereignisse. Im Folgenden wird beschrieben, wie Sie festlegen, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf „package.appxmanifest“, um den Manifest-Designer zu öffnen. Wählen Sie die Registerkarte **Deklarationen** und dann in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie dann auf **Hinzufügen**. Ausführliche Informationen zu Bezeichnern, die von Dateizuordnungen verwendet werden, finden Sie unter [ProgIDs](https://docs.microsoft.com/windows/desktop/shell/fa-progids).

    Es folgt eine kurze Beschreibung der Felder, die Sie im Manifest-Designer ausfüllen können:

| Feld | Beschreibung |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Display Name** | Geben Sie den Anzeigenamen für eine Gruppe von Dateitypen an. Anhand des Anzeigenamens wird in der **Systemsteuerung** unter [Standardprogramme festlegen](https://docs.microsoft.com/windows/desktop/shell/default-programs) der Dateityp identifiziert. |
| **Logo** | Geben Sie das Logo zur Identifikation des Dateityps auf dem Desktop in der **Systemsteuerung** unter [Standardprogramme festlegen](https://docs.microsoft.com/windows/desktop/shell/default-programs) an. Wenn kein Logo angegeben wird, wird das kleine Logo der Anwendung verwendet. |
| **Info Tip** | Geben Sie den [Infotipp](https://docs.microsoft.com/windows/desktop/shell/fa-progids) für eine Gruppe von Dateitypen an. Diese QuickInfo wird angezeigt, wenn Benutzer mit der Maus auf das Symbol für eine Datei dieses Typs zeigen. |
| **Name** | Wählen Sie einen Namen für eine Gruppe von Dateitypen aus, die identische Anzeigenamen, Logos, Infotipps und Bearbeitungsflags verwenden. Wählen Sie einen Gruppennamen aus, der auch nach App-Updates unverändert bleiben kann. **Hinweis**  Der Name darf nur aus Kleinbuchstaben bestehen. |
| **Content Type** | Geben Sie den MIME-Inhaltstyp, z. B. **image/jpeg**, für einen bestimmten Dateityp an. **Wichtiger Hinweis zu zulässigen Inhaltstypen:** Dies ist eine alphabetische Liste der MIME-Inhaltstypen, die Sie nicht in das Paketmanifest eingeben können, da sie entweder reserviert oder unzulässig sind: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **File type** | Geben Sie den Dateityp, für den die Registrierung durchgeführt werden soll, mit vorangestelltem Punkt an, z. B. „.jpeg“. **Reservierte und unzulässige Dateitypen:** Eine alphabetische Liste der Dateitypen für vorinstallierte Apps, die Sie nicht für Ihre UWP-Apps registrieren können, da diese entweder reserviert oder unzulässig sind, finden Sie unter [Reservierte URI-Schemanamen und Dateitypen](reserved-uri-scheme-names.md). |

2.  Geben Sie `alsdk` in das Feld **Name** ein.
3.  Geben Sie `.alsdk` als **Dateityp** ein.
4.  Enter “images\\Icon.png” as the Logo.
5.  Drücken Sie STRG+S, um die an „package.appxmanifest“ vorgenommenen Änderungen zu speichern.

Die obigen Schritte fügen dem Paketmanifest ein [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension)-Element wie das unten dargestellte hinzu. Die **windows.fileTypeAssociation**-Kategorie gibt an, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

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

## <a name="step-2-add-the-proper-icons"></a>Schritt 2: Hinzufügen der geeigneten Symbole

Die Symbole von Apps, die für einen Dateityp zum Standard werden, werden an verschiedenen Stellen innerhalb des Systems angezeigt. Diese Symbole werden z. B. an folgenden Stellen angezeigt:

-   Anzeige der Elemente, Kontextmenüs, Menüband in Windows Explorer
-   Standardprogramme in der Systemsteuerung
-   Dateiauswahl
-   Suchergebnisse auf dem Startbildschirm

Fügen Sie ein 44 x 44-Symbol in das Projekt ein, damit Ihr Logo an diesen Positionen angezeigt werden kann. Stimmen Sie das Erscheinungsbild des Logos der App-Kachel ab, und verwenden Sie die Hintergrundfarbe der App, anstatt das Symbol transparent darzustellen. Erweitern Sie das Logo bis zum Rand, ohne eine Auffüllung vorzunehmen. Testen Sie Ihre Symbole auf weißem Hintergrund. Weitere Einzelheiten zu den Symbolen finden Sie unter [Richtlinien für Kacheln und Symbole](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="step-3-handle-the-activated-event"></a>Schritt 3: Behandeln des activated-Ereignisses

Der [**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)-Ereignishandler empfängt alle Dateiaktivierungsereignisse.

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
> When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

We recommend that you create a new XAML **Frame** for each activation event that opens a new page. That way, the navigation backstack for the new XAML Frame doesn't contain any previous content that the app might have on the current window when suspended. If you decide to use a single XAML **Frame** for Launch and for File Contracts, then you should clear the pages in the **Frame**'s navigation journal before navigating to a new page.

When your app is launched via File activation, you should consider including UI that allows the user to go back to the top page of the app.

## <a name="remarks"></a>Hinweise

Die empfangenen Dateien stammen unter Umständen aus einer nicht vertrauenswürdigen Quelle. Wir empfehlen, den Inhalt einer Datei zu überprüfen, bevor Sie sie weiter verarbeiten.

## <a name="related-topics"></a>Verwandte Themen

### <a name="complete-example"></a>Vollständiges Beispiel

* [Association launching sample](https://code.msdn.microsoft.com/windowsapps/Association-Launching-535d2cec)

### <a name="concepts"></a>Konzepte

* [Default Programs](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [File Type and Protocol Associations Model](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>Aufgaben

* [Starten der Standard-App für eine Datei](launch-the-default-app-for-a-file.md)
* [Behandeln der URI-Aktivierung](handle-uri-activation.md)

### <a name="guidelines"></a>Richtlinien

* [Guidelines for file types and URIs](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Referenz
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
