---
description: In diesem Artikel werden verschiedene Möglichkeiten vorgestellt, eine gepackte Desktop-App mit dem Datei-Explorer unter Verwendung von Paketerweiterungen zu integrieren.
title: Integrieren einer gepackten Desktop-App in den Datei-Explorer
ms.date: 02/08/2021
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3e7aaffc86a152530c933291321c30c2e7c507d
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335201"
---
# <a name="integrate-a-packaged-desktop-app-with-file-explorer"></a>Integrieren einer gepackten Desktop-App in den Datei-Explorer

Einige Windows-Apps definieren Erweiterungen für den Datei-Explorer, die Einträge zum Kontextmenü hinzufügen, mit denen Kunden Optionen im Zusammenhang mit der App ausführen können. Ältere Windows-App-Bereitstellungstechnologien wie MSI und ClickOnce definieren Datei-Explorer-Erweiterungen über die Registrierung. Die Registrierung enthält eine Reihe von Hives, die die Datei-Explorer-Erweiterungen und andere Arten von Shell-Erweiterungen steuern. Diese Installationsprogramme erstellen in der Regel eine Reihe von Registrierungsschlüsseln, um die verschiedenen Elemente zu konfigurieren, die in das Kontextmenü aufgenommen werden sollen.

Wenn Sie Ihre Windows-App mithilfe von [MSIX](/windows/msix/) packen, wird die Registrierung virtualisiert, und Ihre App kann die Datei-Explorer-Erweiterungen daher nicht über die Registrierung registrieren. Stattdessen müssen Sie Ihre Datei-Explorer-Erweiterungen über Paketerweiterungen definieren, die Sie im Paketmanifest festlegen. In diesem Artikel werden verschiedene Vorgehensweisen hierfür beschrieben.

Den kompletten in diesem Artikel verwendeten Beispielcode finden Sie [auf GitHub](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample).

## <a name="add-a-context-menu-entry-that-supports-startup-parameters"></a>Hinzufügen eines Kontextmenüeintrags, der Startparameter unterstützt

Eine der einfachsten Möglichkeiten zur Integration in den Datei-Explorer besteht darin, eine Paketerweiterung zu definieren, die Ihre App zur Liste der verfügbaren Apps im Kontextmenü hinzufügt, wenn ein Benutzer mit der rechten Maustaste auf einen bestimmten Dateityp im Datei-Explorer klickt. Wenn der Benutzer Ihre App öffnet, kann Ihre Erweiterung Parameter an die App übergeben.

Dieses Szenario weist mehrere Einschränkungen auf:

* Dies funktioniert nur in Kombination mit dem [Feature „Dateitypzuordnung“](/windows/uwp/launch-resume/handle-file-activation). Sie können zusätzliche Optionen im Kontextmenü nur für Dateitypen anzeigen, die mit der Haupt-App verbunden sind (z. B. unterstützt Ihre App das Öffnen einer Datei durch Doppelklick im Datei-Explorer).
* Die Optionen im Kontextmenü werden nur angezeigt, wenn Ihre App als Standard für diesen Dateityp eingestellt ist.
* Die einzige unterstützte Aktion ist das Starten der ausführbaren Hauptdatei der App (d. h. die gleiche ausführbare Datei, die mit dem Startmenüeintrag verbunden ist). Jede Aktion kann jedoch verschiedene Parameter angeben, die Sie beim Starten der Apps verwenden können, um zu verstehen, welche Aktion die Ausführung ausgelöst hat, und um verschiedene Aufgaben durchzuführen.

Trotz dieser Einschränkungen ist dieser Ansatz für viele Szenarien ausreichend. Wenn Sie z. B. einen Bild-Editor erstellen, können Sie im Kontextmenü einfach einen Eintrag zum Ändern der Bildgröße hinzufügen, der den Bild-Editor direkt mit einem Assistenten startet, um den Größenänderungsprozess zu starten.

### <a name="implement-the-context-menu-entry"></a>Implementieren des Kontextmenüeintrags

Um dieses Szenario zu unterstützen, fügen Sie ein Element [Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) mit der Kategorie `windows.fileTypeAssociation` zu Ihrem Paketmanifest hinzu. Dieses Element muss als untergeordnetes Element des Elements [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) unter dem Element [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) hinzugefügt werden.

Das folgende Beispiel veranschaulicht eine Registrierung für eine App, die Kontextmenüs für Dateien mit der Erweiterung `.foo` aktiviert. In diesem Beispiel wird die Erweiterung `.foo` angegeben, da es sich um eine fiktive Erweiterung handelt, die bei anderen Apps auf einem bestimmten Computer normalerweise nicht registriert ist. Wenn Sie einen Dateityp verwalten müssen, der möglicherweise bereits belegt ist (z. B. TXT oder JPG), denken Sie daran, dass die Option nur angezeigt wird, wenn Ihre App als Standard für diesen Dateityp festgelegt ist. Dieses Beispiel ist ein Auszug aus der Datei [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) im zugehörigen Beispiel auf GitHub.

```xml
<Extensions>
  <uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="foo" Parameters="&quot;%1&quot;">
      <uap:SupportedFileTypes>
        <uap:FileType>.foo</uap:FileType>
      </uap:SupportedFileTypes>
      <uap2:SupportedVerbs>
        <uap3:Verb Id="Resize" Parameters="&quot;%1&quot; /p">Resize file</uap3:Verb>
      </uap2:SupportedVerbs>
    </uap3:FileTypeAssociation>
  </uap3:Extension>
</Extensions>
```

In diesem Beispiel wird davon ausgegangen, dass die folgenden Namespaces und Aliase im Stammelement `<Package>` des Manifests deklariert sind.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap2 uap3 rescap">
  ...
</Package>
```

Das Element [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) verknüpft Ihre App mit den Dateitypen, die unterstützt werden sollen. Weitere Informationen finden Sie unter [Zuordnen einer gepackten Anwendung zu einer Gruppe von Dateitypen](desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types). Im folgenden finden Sie die wichtigsten Elemente, die mit diesem Element verknüpft sind.

| Attribut oder Element | BESCHREIBUNG |
|----------------------|-------------|
| `Name`-Attribut | Entspricht dem Namen der zu registrierenden Erweiterung ohne den Punkt (im vorherigen Beispiel `foo`). |
| `Parameters`-Attribut | Enthält die Parameter, die Sie an Ihre Anwendung übergeben möchten, wenn der Benutzer auf eine Datei mit dieser Erweiterung doppelklickt. Typischerweise übergeben Sie mindestens `%1`, einen speziellen Parameter, der den Pfad der ausgewählten Datei enthält. Wenn Sie auf eine Datei doppelklicken, erkennt die Anwendung so ihren vollständigen Pfad und kann sie laden. |
| Element [SupportedFileTypes](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-supportedfiletypes) | Gibt die Namen der Erweiterungen an, die Sie registrieren möchten, einschließlich des Punkts (in diesem Beispiel `.foo`). Sie können mehrere `<FileType>`-Einträge angeben, wenn mehr Dateitypen unterstützt werden sollen. |

Zum Definieren der Kontextmenüintegration müssen Sie auch das untergeordnete Element [SupportedVerbs](/uwp/schemas/appxpackage/uapmanifestschema/element-uap2-supportedverbs) hinzufügen. Dieses Element enthält ein oder mehrere [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb)-Elemente, die die Optionen definieren, die aufgelistet werden, wenn ein Benutzer im Datei-Explorer mit der rechten Maustaste auf eine Datei mit der Erweiterung „.foo“ klickt. Weitere Informationen finden Sie unter [Hinzufügen von Optionen zu den Kontextmenüs von Dateien eines bestimmten Dateityps](desktop-to-uwp-extensions.md#add-options-to-the-context-menus-of-files-that-have-a-certain-file-type). Hier finden Sie die wichtigsten Elemente, die mit dem Element [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) zusammenhängen.

| Attribut oder Element | BESCHREIBUNG |
|----------------------|-------------|
| `Id`-Attribut | Gibt den eindeutigen Bezeichner für die Aktion an.|
| `Parameters`-Attribut | Ähnlich wie das Element [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) enthält dieses Attribut für das Element [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) die Parameter, die an Ihre Anwendung übergeben werden, wenn der Benutzer auf den Kontextmenüeintrag klickt. Typischerweise übergeben Sie außer dem speziellen Parameter `%1` zum Abrufen des Pfads der ausgewählten Datei auch einen oder mehrere Parameter, um den Kontext abzurufen. Dadurch kann Ihre App erkennen, dass sie über einen Kontextmenüeintrag geöffnet wurde.  |
| Wert des Elements | Der Wert des Elements [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) enthält die Bezeichnung, die im Kontextmenüeintrag angezeigt werden soll (in diesem Beispiel **Dateigröße ändern**). |

### <a name="access-the-startup-parameters-in-your-app-code"></a>Zugriff auf die Startparameter in Ihrem App-Code

Die Art und Weise, wie Ihre App die Parameter empfängt, hängt vom Typ der App ab, die Sie erstellt haben. Eine WPF-App verarbeitet zum Beispiel typischerweise Startereignisargumente in der Methode `OnStartup` der Klasse `App`. Sie können prüfen, ob es Startparameter gibt, und basierend auf dem Ergebnis die am besten geeignete Aktion durchführen (z. B. anstelle des Hauptfensters ein bestimmtes Fenster der Anwendung öffnen).

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        if (e.Args.Contains("Resize"))
        {
            // Open a specific window of the app.
        }
        else
        {
            MainWindow main = new MainWindow();
            main.Show();
        }
    }
}
```

Der folgende Screenshot veranschaulicht den im vorherigen Beispiel erstellten Kontextmenüeintrag **Dateigröße ändern**.

![Screenshot des Befehls „Dateigröße ändern“ im Kontextmenü](images/file-explorer/resize-file.png)

## <a name="support-generic-files-or-folders-and-perform-complex-tasks"></a>Unterstützen generischer Dateien oder Ordner und Durchführen komplexer Aufgaben

Obwohl die Verwendung der [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)-Erweiterung im Paketmanifest, wie im vorigen Abschnitt beschrieben, für viele Szenarien ausreichend ist, kann dies für Sie einschränkend sein. Die zwei größten Herausforderungen sind:

* Sie können nur Dateitypen bearbeiten, die Ihnen zugeordnet sind. Sie können zum Beispiel keinen generischen Ordner behandeln.
* Sie können die App nur mit einer Reihe von Parametern starten. Sie können keine erweiterten Operationen durchführen, wie z. B. das Starten einer anderen ausführbaren Datei oder das Ausführen einer Aufgabe, ohne die Haupt-App zu öffnen.

Um diese Ziele zu erreichen, müssen Sie eine [Shellerweiterung ](/windows/win32/shell/shell-exts)erstellen, die leistungsfähigere Möglichkeiten zur Integration in den Datei-Explorer bietet. In diesem Szenario erstellen Sie eine DLL, die alles enthält, was zur Verwaltung des Dateikontextmenüs erforderlich ist, einschließlich Beschriftung, Symbol, Status und auszuführende Aufgaben. Da diese Funktionalität in einer DLL implementiert ist, können Sie fast alle Aufgaben ausführen, die auch mit einer normalen App möglich sind. Nachdem Sie die DLL implementiert haben, müssen Sie sie über Erweiterungen registrieren, die Sie in Ihrem Paketmanifest definieren.

> [!NOTE]
> Für das in diesem Abschnitt beschriebene Verfahren gilt eine Einschränkung. Nachdem das MSIX-Paket, das die Erweiterung enthält, auf einem Zielcomputer installiert wurde, muss der Datei-Explorer neu gestartet werden, bevor die Shellerweiterung geladen werden kann. Um dies zu erreichen, kann der Benutzer den Computer oder den **explorer.exe**-Prozess mit dem **Task-Manager** neu starten.

### <a name="implement-the-shell-extension"></a>Implementieren der Shellerweiterung

Shellerweiterungen basieren auf dem [Komponentenobjektmodell (COM, Component Object Model)](/windows/win32/com/component-object-model--com--portal). Die DLL macht ein oder mehrere COM-Objekte verfügbar, die in der Systemregistrierung registriert werden. Windows erkennt diese COM-Objekte und bindet Ihre Erweiterung in den Datei-Explorer ein. Da Sie Ihren Code in die Windows-Shell integrieren, sind Leistung und Speicherbedarf wichtig. Daher werden diese Arten von Erweiterungen typischerweise mit C++ erstellt.

Beispielcode, der veranschaulicht, wie Shellerweiterungen implementiert werden, finden Sie im Projekt [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) im zugehörigen Beispiel auf GitHub. Dieses Projekt basiert auf [diesem Beispiel](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) in den Beispielen für Windows-Desktop und wurde mehrfach überarbeitet, um seine Verwendung mit den neuesten Versionen von Visual Studio zu erleichtern.

Dieses Projekt enthält eine Menge Bausteincode für verschiedene Aufgaben, z. B. dynamische oder statische Menüs und manuelle Registrierung der DLL. Der größte Teil dieses Codes wird nicht benötigt, wenn Sie Ihre App mit MSIX packen, da die Paketunterstützung diese Aufgaben für Sie übernimmt. Die Datei [ExplorerCommandVerb.cpp](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/ExplorerCommandVerb.cpp) enthält die Implementierung des Kontextmenüs, und dies ist die Hauptcodedatei, die für diese exemplarische Vorgehensweise von Interesse ist.

Die Schlüsselfunktion lautet `CExplorerCommandVerb::Invoke`. Dies ist die Funktion, die aufgerufen wird, wenn ein Benutzer auf den Eintrag im Kontextmenü klickt. Im Beispiel wird die Operation auf einem anderen Thread ausgeführt, um die Auswirkungen auf die Leistung zu minimieren. Die tatsächliche Implementierung finden Sie also in `CExplorerCommandVerb::_ThreadProc`.

```cpp
DWORD CExplorerCommandVerb::_ThreadProc()
{
    IShellItemArray* psia;
    HRESULT hr = CoGetInterfaceAndReleaseStream(_pstmShellItemArray, IID_PPV_ARGS(&psia));
    _pstmShellItemArray = NULL;
    if (SUCCEEDED(hr))
    {
        DWORD count;
        psia->GetCount(&count);

        IShellItem2* psi;
        HRESULT hr = GetItemAt(psia, 0, IID_PPV_ARGS(&psi));
        if (SUCCEEDED(hr))
        {
            PWSTR pszName;
            hr = psi->GetDisplayName(SIGDN_DESKTOPABSOLUTEPARSING, &pszName);
            if (SUCCEEDED(hr))
            {
                WCHAR szMsg[128];
                StringCchPrintf(szMsg, ARRAYSIZE(szMsg), L"%d item(s), first item is named %s", count, pszName);

                MessageBox(_hwnd, szMsg, L"ExplorerCommand Sample Verb", MB_OK);

                CoTaskMemFree(pszName);
            }

            psi->Release();
        }
        psia->Release();
    }

    return 0;
}
```

Wenn ein Benutzer mit der rechten Maustaste auf eine Datei oder einen Ordner klickt, zeigt diese Funktion ein Meldungsfeld mit dem vollständigen Pfad der ausgewählten Datei bzw. des ausgewählten Ordners an. Wenn Sie die Shellerweiterung auf andere Weise anpassen möchten, können Sie die folgenden Funktionen im Beispiel erweitern:

- Ändern Sie die Funktion [GetTitle](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettitle), um die Bezeichnung des Eintrags im Kontextmenü anzupassen.
- Ändern Sie die Funktion [GetIcon](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-geticon), um das Symbol, das neben dem Eintrag im Kontextmenü angezeigt wird, anzupassen.
- Ändern Sie die Funktion [GetToolTip](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettooltip), um die QuickInfo anzupassen, die angezeigt wird, wenn Sie im Kontextmenü mit dem Mauszeiger auf den Eintrag zeigen.

### <a name="register-the-shell-extension"></a>Registrieren der Shellerweiterung

Da die Shellerweiterung auf COM basiert, muss die Implementierungs-DLL als COM-Server verfügbar gemacht werden, damit Windows sie in den Datei-Explorer integrieren kann. Typischerweise geschieht dies, indem dem COM-Server eine eindeutige ID (eine so genannte CLSID) zugewiesen wird und er in einem bestimmten Hive der Systemregistrierung registriert wird. Im [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb)-Projekt ist die CLSID für die Erweiterung `CExplorerCommandVerb` in der Datei [Dll.h](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/Dll.h) definiert.

```cpp
class __declspec(uuid("CC19E147-7757-483C-B27F-3D81BCEB38FE")) CExplorerCommandVerb;
```

Wenn Sie eine Shellerweiterungs-DLL in ein MSIX-Paket packen, folgen Sie einem ähnlichen Ansatz. Allerdings muss die GUID, wie [hier](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)erläutert, im Paketmanifest anstelle der Registrierung registriert werden.

Beginnen Sie in Ihrem Paketmanifest, indem Sie die folgenden Namespaces zu Ihrem **Package**-Element hinzufügen.

```xml
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:desktop5="http://schemas.microsoft.com/appx/manifest/desktop/windows10/5"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10" 
  IgnorableNamespaces="desktop desktop4 desktop5 com">
    
    ...
</Package>
```

Um die CLSID zu registrieren, fügen Sie ein [com.Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver)-Element mit der Kategorie `windows.comServer` zu Ihrem Paketmanifest hinzu. Dieses Element muss als untergeordnetes Element des Elements [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) unter dem Element [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) hinzugefügt werden. Dieses Beispiel ist ein Auszug aus der Datei [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) im zugehörigen Beispiel auf GitHub.

```xml
<com:Extension Category="windows.comServer">
  <com:ComServer>
    <com:SurrogateServer DisplayName="ContextMenuSample">
      <com:Class Id="CC19E147-7757-483C-B27F-3D81BCEB38FE" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
    </com:SurrogateServer>
  </com:ComServer>
</com:Extension>
```

Es gibt zwei wichtige Attribute, die im [com:Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver-class)-Element konfiguriert werden müssen.

| attribute | BESCHREIBUNG |
|----------------------|-------------|
| `Id`-Attribut | Muss mit der CLSID des Objekts übereinstimmen, das Sie registrieren möchten. In diesem Beispiel ist dies die CLSID, die in der Datei `CExplorerCommandVerb` deklariert ist, die der Klasse `Dll.h` zugeordnet ist. |
| `Path`-Attribut | Muss den Namen der DLL enthalten, die das COM-Objekt verfügbar macht. In diesem Beispiel ist die DLL im Stammverzeichnis des Pakets enthalten, sodass nur der Name der vom `ExplorerCommandVerb`-Projekt generierten DLL angegeben werden muss. |

Als Nächstes fügen Sie eine weitere Erweiterung zum Registrieren des Dateikontextmenüs hinzu. Fügen Sie dazu ein [desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension)-Element mit der Kategorie `windows.fileExplorerContextMenus` zu Ihrem Paketmanifest hinzu. Dieses Element muss ebenfalls als untergeordnetes Element des Elements [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) unter dem Element [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) hinzugefügt werden.

```xml
<desktop4:Extension Category="windows.fileExplorerContextMenus">
  <desktop4:FileExplorerContextMenus>
    <desktop5:ItemType Type="Directory">
      <desktop5:Verb Id="Command1" Clsid="CC19E147-7757-483C-B27F-3D81BCEB38FE" />
    </desktop5:ItemType>
  </desktop4:FileExplorerContextMenus>
</desktop4:Extension>
```

Es gibt zwei wichtige Attribute, die unter dem [desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension)-Element konfiguriert werden müssen.

| Attribut oder Element | BESCHREIBUNG |
|----------------------|-------------|
| `Type`-Attribut von [desktop5:ItemType](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-itemtype) | Definiert die Art der Elemente, die Sie dem Kontextmenü zuordnen möchten. Es könnte ein Sternchen (`*`) sein, wenn es für alle Dateien angezeigt werden soll, oder eine bestimmte Dateierweiterung (`.foo`), oder es kann für Ordner verfügbar sein (`Directory`). |
| `Clsid`-Attribut von [desktop5:Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-verb) | Muss mit der CLSID übereinstimmen, die Sie zuvor als COM-Server in der Paket Manifestdatei registriert haben. |

### <a name="configure-the-dll-in-the-package"></a>Konfigurieren der DLL im Paket

Fügen Sie die DLL, die die Shellerweiterung implementiert (in diesem Beispiel **ExplorerCommandVerb.dll**) in das Stammverzeichnis des MSIX-Pakets ein. Wenn Sie das [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) verwenden, ist die einfachste Lösung, die DLL zu kopieren und in das Projekt einzufügen und sicherzustellen, dass die Option **In Ausgabeverzeichnis kopieren** für die DLL-Dateieigenschaften auf **Kopieren, wenn neuer** festgelegt ist.

Um sicherzustellen, dass das Paket immer die aktuellste Version der DLL enthält, können Sie dem Shellerweiterungsprojekt ein [Postbuildereignis](/visualstudio/ide/specifying-custom-build-events-in-visual-studio) hinzufügen, damit die DLL bei jedem Build in das Paketerstellungsprojekt für Windows-Anwendungen kopiert wird.

### <a name="restart-file-explorer"></a>Neustarten des Datei-Explorers

Nachdem Sie das Shellerweiterungspaket installiert haben, müssen Sie den Datei-Explorer neu starten, bevor die Shellerweiterung geladen werden kann. Diese Einschränkung gilt für Shellerweiterungen, die über MSIX-Pakete bereitgestellt und registriert werden.

Um die Shellerweiterung zu testen, starten Sie Ihren PC neu, oder starten Sie den Prozess **explorer.exe** über den **Task-Manager** neu. Danach sollte der Eintrag im Kontextmenü angezeigt werden.

![Screenshot des benutzerdefinierten Kontextmenüeintrags](images/file-explorer/folder-context-menu.png)

Wenn Sie darauf klicken, wird die Funktion `CExplorerCommandVerb::_ThreadProc` aufgerufen, um das Meldungsfeld mit dem Pfad des ausgewählten Ordners anzuzeigen.

![Screenshot des benutzerdefinierten Popups](images/file-explorer/popup.png)
