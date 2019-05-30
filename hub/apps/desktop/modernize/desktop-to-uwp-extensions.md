---
Description: Sie können Erweiterungen verwenden, um Ihre verpackte Desktop-App in Windows 10 auf vordefinierter Weise zu integrieren.
title: Integrieren Sie Ihre App-Pakete Desktopanwendung mit Windows 10 und UWP (Desktop-Brücke)
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 291c16d14428f8c6476b12fbadf00f84c26a4235
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359481"
---
# <a name="integrate-your-packaged-desktop-app-with-windows-10-and-uwp"></a>Integrieren Sie Ihre desktop-app-Paket mit Windows 10 und UWP

Wenn Sie [Packen Sie die desktop-Anwendung in einem Container MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), können Sie Erweiterungen in Windows 10 die gepackte desktop-Anwendung integrieren, mithilfe von vordefinierten Erweiterungen in der [fürapp-Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Verwenden Sie z. B. eine Erweiterung, um eine Firewallausnahme zu erstellen, stellen Ihre Anwendung als standardanwendung für einen Dateityp oder zeigen Sie die Start-Kacheln auf der verpackten Version Ihrer app. Um eine Erweiterung zu verwenden, fügen Sie einfach XML-Codes zur Paketmanifestdatei Ihrer App hinzu. Es ist kein Code erforderlich.

Dieser Artikel beschreibt diese Erweiterungen und die Aufgaben, die Sie ausführen können, verwenden Sie sie.

> [!NOTE]
> Die in diesem Artikel beschriebenen Funktionen erfordern, dass Sie ein Windows-app-Paket für die desktop-Anwendung erstellen. Wenn Sie noch dies nicht getan haben, finden Sie unter [Paket desktopanwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root).

## <a name="transition-users-to-your-app"></a>Den Übergang Ihrer Benutzer auf Ihre App bereitstellen

Helfen Sie Ihren Benutzern, auf Ihre verpackte App umzustellen.

* [Zeigen Sie vorhandene Start-Kacheln und Schaltflächen der Taskleiste auf Ihre app-Paket](#point)
* [Die gepackte Anwendung anstelle Ihrer desktop-app-Dateien öffnen](#make)
* [Zuordnen der gepackten Anwendung mit einem Satz von Dateitypen](#associate)
* [Hinzufügen von Optionen zu den Kontextmenüs von Dateien, die einen bestimmten Dateityp haben.](#add)
* [Öffnen Sie bestimmte Arten von Dateien direkt mithilfe einer URL](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Verweisen Sie mit vorhandenen Startkacheln Taskleistenschaltflächen auf die verpackte App.

Ihre Benutzer haben möglicherweise Ihre Desktop-Anwendung an die Taskleiste oder das Startmenü angeheftet. Sie mit diesen Verknüpfungen auf Ihre neu verpackte App verweisen.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Name | Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.desktopAppMigration``.
|AumID |Die Anwendungsbenutzermodell-ID der verpackten App. |
|Verknüpfungspfad |Der Pfad zu den Ink-Dateien, die die Desktop-Version Ihrer App starten. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Verwandte Beispiele

[WPF-Bildanzeigeprogramms Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Die gepackte Anwendung anstelle Ihrer desktop-app-Dateien öffnen

Sie können sicherstellen, dass Benutzer Ihre neue Anwendung für die App-Pakete, die standardmäßig für bestimmte Typen von Dateien, anstatt Sie zu öffnen die Desktopversion von Ihrer app geöffnet.

Dazu müssen Sie den [programmgesteuerten Bezeichner (Programm-ID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) jeder Anwendung angeben, aus der Sie Dateizuordnungen übernehmen möchten.

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. Diese ID wird intern verwendet, um einen [programmgesteuerte Bezeichner (Programm-ID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) mit Hash zu generieren, die mit Ihrer Dateitypzuordnung verknüpft ist. Mithilfe dieser ID können Sie Änderungen in zukünftigen Versionen Ihrer App verwalten. |
|MigrationProgId |Die [Programmbezeichner (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) , beschreibt die Anwendung, Komponente und Version der Desktopanwendung, die von dem Sie die dateizuordnungen erben möchten.|

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Verwandte Beispiele

[WPF-Bildanzeigeprogramms Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Zuordnen der gepackten Anwendung mit einem Satz von Dateitypen

Sie können die gepackte Anwendung mit dateityperweiterungen verknüpft. Wenn ein Benutzer eine Datei mit der rechten Maustaste, und dann wählt die **Öffnen mit** option, Ihre Anwendung in der Liste mit Vorschlägen angezeigt wird.

#### <a name="xml-namespace"></a>XML-Namespace

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. Diese ID wird intern verwendet, um einen [programmgesteuerte Bezeichner (Programm-ID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) mit Hash zu generieren, die mit Ihrer Dateitypzuordnung verknüpft ist. Mithilfe dieser ID können Sie Änderungen in zukünftigen Versionen Ihrer App verwalten.   |
|FileType |Die Erweiterung, die von Ihrer App unterstützt wird. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Verwandte Beispiele

[WPF-Bildanzeigeprogramms Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Fügen Sie den Kontextmenüs der Dateien, die einen bestimmten Dateityp haben, Optionen hinzu.

In den meisten Fällen klicken Benutzer doppelt auf Dateien, um sie zu öffnen. Wenn Benutzer mit der rechten Maustaste auf eine Datei klicken, werden verschiedene Optionen angezeigt.

Sie können diesem Menü Optionen hinzufügen. Diese Optionen geben Benutzern weitere Möglichkeiten zur Interaktion mit der Datei, wie etwa Drucken, Bearbeiten oder das Anzeigen einer Vorschau.

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie | Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|Verb |Der Name, der im Kontextmenü des Datei-Explorers angezeigt wird. Diese Zeichenfolge kann mithilfe von ```ms-resource``` lokalisiert werden.|
|Id |Die eindeutige ID des Verbs. Wenn Ihre Anwendung eine UWP-app ist, wird dies zu Ihrer app im Rahmen der Aktivierung-Ereignis-Args übergeben, damit sie die Auswahl des Benutzers entsprechend verarbeiten kann. Wenn Ihre Anwendung ein voll vertrauenswürdige app-Paket ist, erhält er Parameter stattdessen (siehe nächsten Punkt). |
|Parameter |Die Liste mit Argumentparametern und -werten für das Verb. Wenn Ihre Anwendung ein voll vertrauenswürdige app-Paket ist, werden diese Parameter an die Anwendung als Ereignis-Args übergeben, wenn die Anwendung aktiviert ist. Sie können das Verhalten Ihrer Anwendung basierend auf verschiedenen Aktivierung Verben anpassen. Wenn eine Variable einen Dateipfad enthalten kann, schließen Sie den Parameterwert in Anführungszeichen. Dadurch werden jegliche Probleme vermieden, die in Fällen auftreten, bei denen der Pfad Leerzeichen enthält. Wenn Ihre Anwendung eine UWP-app ist, übergeben keine Parameter. Die App empfängt stattdessen die ID (siehe das vorherige Aufzählungszeichen).|
|Erweitert |Gibt an, dass das Verb nur angezeigt werden soll, wenn der Benutzer zum Anzeigen des Kontextmenüs **UMSCHALT** gedrückt hält, bevor er mit der rechten Maustaste auf die Datei klickt. Dieses Attribut ist optional und standardmäßig auf den Wert von **False** (Verb soll immer angezeigt werden) festgelegt. Dieses Verhalten muss für jedes Verb einzeln angegeben werden – mit Ausnahme von „Öffnen“: Bei diesem Verb ist der Wert immer **False**.|

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Verwandte Beispiele

[WPF-Bildanzeigeprogramms Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Öffnen Sie bestimmte Dateitypen direkt anhand einer URL

Sie können sicherstellen, dass Benutzer Ihre neue Anwendung für die App-Pakete, die standardmäßig für bestimmte Typen von Dateien, anstatt Sie zu öffnen die Desktopversion von Ihrer app geöffnet.

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|UseUrl |Gibt an, ob Dateien direkt über eine URL-Ziel geöffnet werden sollen. Wenn Sie diesen Wert nicht festlegen, versucht von Ihrer Anwendung zum Öffnen einer Datei mit einer Ursache für die URL der ersten Download die Datei lokal. |
|Parameter |Optionale Parameter. |
|FileType |Die relevanten Dateierweiterungen. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>Setup-Aufgaben ausführen

* [Firewallausnahme für Ihre app erstellen](#rules)
* [Speichern Sie die DLL-Dateien in einen beliebigen Ordner des Pakets](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Erstellen von Firewallausnahmen für Ihre App

Wenn Ihre Anwendung die Kommunikation über einen Port erforderlich ist, können Sie Ihre Anwendung in die Liste der Firewallausnahmen hinzufügen.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.firewallRules``|
|Ausführbare Datei |Der Name der ausführbaren Datei, die Sie der Liste der Firewallausnahmen hinzufügen möchten. |
|Richtung |Gibt an, ob die Regel eine ein- oder ausgehende Regel ist. |
|IPProtocol |Das Kommunikationsprotokoll |
|LocalPortMin |Die untere Portnummer in einer Auswahl von lokalen Portnummern. |
|LocalPortMax |Die höchste Portnummer in einer Auswahl von lokalen Portnummern. |
|RemotePortMax |Die niedrigere Portnummer in einer Auswahl von Remoteportnummern. |
|RemotePortMax |Die höchste Portnummer in einer Auswahl Remoteportnummern. |
|Profil |Der Netzwerktyp |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Speichern Sie die DLL-Dateien in einem beliebigen Ordner des Pakets

Verwenden Sie eine Erweiterung, um diese Ordner zu identifizieren. Auf diese Weise kann das System die Dateien finden und laden, die Sie darin abgelegt haben. Stellen Sie sich diese Erweiterung als Ersatz für die Umgebungsvariable _%PATH%_ vor.

Wenn Sie diese Erweiterung nicht verwenden, durchsucht das System das Paketabhängigkeitsdiagramm des Prozesses, den Paketstammordner und anschließend das Systemverzeichnis ( _%SystemRoot%\system32_) in dieser Reihenfolge. Weitere Informationen hierzu finden Sie unter [Suchreihenfolge von Windows-Apps](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order).

Jedes Paket kann nur eine dieser Erweiterungen enthalten. Das bedeutet, Sie können Ihrem Hauptpaket, und dann jedem Ihrer [optionalen Pakete und zugehörigen Sets](https://docs.microsoft.com/windows/uwp/packaging/optional-packages) eine hinzufügen.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

Deklarieren Sie diese Erweiterung auf der Paketebene des App-Manifests.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Name | Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.loaderSearchPathOverride``.
|FolderPath | Der Pfad des Ordners, der die DLL-Dateien enthält. Geben Sie einen Pfad relativ zum Stammordner des Pakets an. Sie können bis zu fünf Pfade in einer Erweiterung angeben. Wenn das System nach Dateien im Stammordner des Pakets suchen soll, verwenden Sie eine leere Zeichenfolge für einen dieser Pfade. Fügen Sie keine doppelten Pfade ein und stellen Sie sicher, dass die Pfade keine voran- bzw. nachgestellten Schrägstriche oder umgekehrten Schrägstriche enthalten. <br><br> Das System durchsucht keine Unterordner. Stellen Sie daher sicher, dass Sie jeden Ordner mit DLL-Dateien, die das System laden soll, explizit auflisten.|

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>Integration mit dem Datei-Explorer

Helfen Sie Benutzern beim Organisieren von Dateien und interagieren Sie mit Ihnen auf vertrauter Weise.

* [Definieren Sie das Verhalten Ihrer Anwendung bei der Benutzer auswählen und mehrere Dateien gleichzeitig öffnen](#define)
* [Inhalt der Datei in eine Miniaturansicht im Datei-Explorer anzeigen](#show)
* [Inhalt der Datei in einer Vorschau der Datei-Explorer anzeigen](#preview)
* [Aktivieren Sie Benutzer aus, um Dateien zu gruppieren, indem Sie die Kind-Spalte im Datei-Explorer](#enable)
* [Stellen Sie Eigenschaften der Datei suchen, Index, Dialogfelder für Ereigniseigenschaften und im Detailbereich zur Verfügung](#make-file-properties)
* [Stellen Sie Dateien aus Ihrem Clouddienst im Datei-Explorer angezeigt werden.](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definieren Sie das Verhalten Ihrer Anwendung bei der Benutzer auswählen und mehrere Dateien gleichzeitig öffnen

Geben Sie an, wie Ihre Anwendung sich verhält, wenn ein Benutzer mehrere Dateien gleichzeitig öffnen.

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|MultiSelectModel |Weitere Informationen finden Sie unter unten. |
|FileType |Die relevanten Dateierweiterungen. |

**MultSelectModel**

Bei verpackten Desktop-Apps stehen die gleichen drei Optionen zur Verfügung wie bei regulären Desktop-Apps.

* ``Player``: Die Anwendung wird nur einmal aktiviert. Alle ausgewählten Dateien werden als Argument-Parameter an die Anwendung übergeben.
* ``Single``: Die Anwendung wird einmal für die ersten ausgewählten Datei aktiviert. Andere Dateien werden ignoriert.
* ``Document``: Eine neue, eigenständige Instanz Ihrer Anwendung wird für jede ausgewählte Datei aktiviert.

 Für unterschiedliche Dateitypen und Aktionen können unterschiedliche Einstellungen festgelegt werden. So können beispielsweise *Dokumente* im Modus *Document* und *Bilder* im Modus *Player* geöffnet werden.

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Wenn der Benutzer 15 oder weniger Dateien öffnet, ist die Standardauswahl des **MultiSelectModel**-Attributs *Player*. Andernfalls ist die Standardauswahl *Document*. UWP-Apps werden immer als *Player* gestartet.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Zeigen Sie Dateiinhalte in einem Miniaturbild im Datei-Explorer an.

Ermöglichen Sie es Benutzern, eine Miniaturansicht des Dateiinhalts anzuzeigen, wenn das Symbol der Datei in den Größen Mittel, Groß, oder extra Groß angezeigt wird.

#### <a name="xml-namespace"></a>XML-Namespace

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid   |Die Klassen-ID Ihrer App. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Zeigen Sie Dateiinhalte in der Vorschau des Datei-Explorers an.

Ermöglichen Sie es Benutzern, den Inhalt einer Datei in Vorschaubereich des Datei-Explorers anzuzeigen.

#### <a name="xml-namespace"></a>XML-Namespace

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid   |Die Klassen-ID Ihrer App. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Ermöglichen Sie es Benutzern, Dateien mithilfe der Spalte „Art” im Datei-Explorer zu gruppieren.

Sie können einen oder mehrere vordefinierte Werte für die Dateitypen mit dem **Art**-Feld zuordnen.

Im Datei-Explorer können Benutzer diese Dateien mithilfe dieses Felds gruppieren. Systemkomponenten verwenden auch dieses Feld für verschiedene Zwecke, wie etwa für die Indizierung.

Für weitere Informationen zum **Art**-Feld und die Werte, die Sie für dieses Feld verwenden können, finden Sie unter [Art Namen verwenden](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|FileType |Die relevanten Dateierweiterungen. |
|Wert |Ein gültiger [Art-Wert](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Stellen Sie den Bereichen Suchen, Index, Eigenschaftendialogfelder und Details Dateieigenschaften zur Verfügung.

#### <a name="xml-namespace"></a>XML-Namespace

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Eine eindeutige ID für Ihre App. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid  |Die Klassen-ID Ihrer App. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Zeigen Sie Dateien von Ihrem Clouddienst im Datei-Explorer an

Registrieren Sie die Handler, die Sie in Ihrer Anwendung implementieren. Sie können auch Kontextmenüoptionen hinzufügen, die angezeigt werden, wenn Ihre Benutzer im Datei-Explorer mit der rechten Maustaste auf die cloudbasierten Dateien klicken.

#### <a name="xml-namespace"></a>XML-Namespace

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.cloudfiles``.
|iconResource |Das Symbol, das den Dienst Ihres Cloud-Datei-Anbieters darstellt. Dieses Symbol wird im Navigationsbereich des Datei-Explorers angezeigt.  Benutzer wählen dieses Symbol, um Dateien von Ihrem Clouddienst anzuzeigen. |
|CustomStateHandler Clsid |Die Klassen-ID der Anwendung, die die CustomStateHandler implementiert werden soll. Das System verwendet diese Klassen-ID, um benutzerdefinierte Zustände und Spalten für Cloud-Dateien anzufordern. |
|ThumbnailProviderHandler Clsid |Die Klassen-ID der Anwendung, die die ThumbnailProviderHandler implementiert werden soll. Das System verwendet diese Klassen-ID, um Miniaturansichten für Cloud-Dateien anzufordern. |
|ExtendedPropertyHandler Clsid |Die Klassen-ID der Anwendung, die die ExtendedPropertyHandler implementiert werden soll.  Das System verwendet diese Klassen-ID, um erweiterte Eigenschaften für eine Cloud-Datei anzufordern. |
|Verb |Der Name, der im Kontextmenü des Datei-Explorers für Dateien angezeigt wird, die von Ihrem Clouddienst bereitgestellt werden. |
|Id |Die eindeutige ID des Verbs. |

#### <a name="example"></a>Beispiel

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>Die Anwendung auf unterschiedliche Weise starten

* [Starten Sie Ihre Anwendung unter Verwendung des Protokolls](#protocol)
* [Starten Sie Ihre Anwendung mit einem alias](#alias)
* [Starten Sie eine ausführbare Datei aus, wenn Benutzer sich bei Windows anmelden](#executable)
* [Aktivieren von Benutzern für Ihre Anwendung zu starten, wenn sie ein Gerät mit ihrem PC verbinden](#autoplay)
* [Nach dem Empfang eines Updates aus dem Microsoft Store starten Sie automatisch neu](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Starten Sie Ihre Anwendung unter Verwendung des Protokolls

Protokollzuordnungen ermöglichen es anderen Programmen und Systemkomponenten, mit Ihrer verpackten App zu interagieren. Wenn die gepackte Anwendung über ein Protokoll gestartet wird, können Sie bestimmte Parameter an seine Aktivierung Ereignisargumente übergeben werden soll, damit es sich entsprechend Verhalten kann angeben. Parameter werden nur für verpackte, vertrauenswürdige Apps unterstützt. UWP-Apps können die Parameter nicht verwenden.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.protocol``.
|Name |Der Name des Protokolls. |
|Parameter |Die Liste der Parameter und Werte als Argumente an die Anwendung übergeben werden, wenn die Anwendung aktiviert ist. Wenn eine Variable einen Dateipfad enthalten kann, schließen Sie den Parameterwert in Anführungszeichen. Dadurch werden jegliche Probleme vermieden, die in Fällen auftreten, bei denen der Pfad Leerzeichen enthält. |

### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Starten Sie Ihre Anwendung mit einem alias

Benutzer und andere Prozesse können einen Alias verwenden, um Ihre Anwendung zu starten, ohne den vollständigen Pfad zu Ihrer app anzugeben. Sie können diesen Aliasnamen angeben.

#### <a name="xml-namespaces"></a>XML-Namespaces

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.appExecutionAlias``.
|Ausführbare Datei |Der relative Pfad zur ausführbaren Datei, die beim Aufrufen des Alias gestartet wird. |
|Alias |Der kurze Name für Ihre App. Er muss immer mit der Erweiterung „.exe“ enden. Für die einzelnen Anwendungen im Paket kann immer nur einzelner App-Ausführungsalias angegeben werden. Wenn sich mehrere Apps mit dem gleichen Alias registrieren, ruft das System die zuletzt registrierte App auf. Wählen Sie daher einen eindeutigen Alias, um die Wahrscheinlichkeit einer Überschreibung durch andere Apps möglichst gering zu halten.
|

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
 
...
</Package>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Starten Sie eine ausführbare Datei, wenn Benutzer sich bei Windows anmelden.

Mit Startaufgaben können Ihre Anwendung eine ausführbare Datei automatisch ausgeführt, wenn ein Benutzer anmeldet.

> [!NOTE]
> Der Benutzer hat auf Ihre Anwendung mindestens ein Mal zu starten, um diese Startaufgabe zu registrieren.

Ihre Anwendung kann mehrere Startaufgaben deklarieren. Die Aufgaben beginnen unabhängig voneinander. Alle Startaufgaben werden im Task-Manager auf der Registerkarte **Autostart** mit dem Namen aus Ihrem App-Manifest und dem Symbol Ihrer App angezeigt. Der Task-Manager analysiert automatisch die Startauswirkungen Ihrer Aufgaben.

Benutzer können die Startaufgabe Ihrer App manuell mithilfe des Task-Managers deaktivieren. Wenn ein Benutzer eine Aufgabe deaktiviert, können Sie sie nicht erneut programmgesteuert aktivieren.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.startupTask``.|
|Ausführbare Datei |Der relative Pfad der ausführbaren Datei, die gestartet werden soll. |
|TaskId |Ein eindeutiger Bezeichner Ihrer Aufgabe. Verwenden diesen Bezeichner ein, kann Ihre Anwendung die APIs in Aufrufen der [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) -Klasse programmgesteuert aktivieren oder deaktivieren eine Startaufgabe. |
|Enabled |Gibt an, ob die Aufgabe erst aktiviert oder deaktiviert gestartet wird. Aktivierte Aufgaben werden bei der nächsten Anmeldung des Benutzers ausgeführt (es sei denn, der Benutzer deaktiviert sie). |
|DisplayName |Der Name der Aufgabe, die im Task-Manager angezeigt wird. Sie können diese Zeichenfolge mit ```ms-resource``` lokalisieren. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Aktivieren von Benutzern für Ihre Anwendung zu starten, wenn sie ein Gerät mit ihrem PC verbinden

Wenn ein Benutzer ein Gerät mit ihrem PC herstellt, kann die automatische Wiedergabe Ihrer Anwendung als eine Option darstellen.

#### <a name="xml-namespace"></a>XML-Namespace

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.autoPlayHandler``.
|ActionDisplayName |Eine Zeichenfolge, die Aktion darstellt, die Benutzer mit einem Gerät ausführen können, die sie mit einem PC verbunden sind (z. B.: "Importdateien" oder "Video abspielen"). |
|ProviderDisplayName | Eine Zeichenfolge, die Anwendung oder des Diensts darstellt (z. B.: "Contoso video Player"). |
|ContentEvent |Der Name eines Inhaltsereignisses, bei dem Benutzer mit Ihrem ``ActionDisplayName`` und ``ProviderDisplayName`` aufgefordert werden. Ein Inhaltsereignis wird ausgelöst, wenn ein Volumegerät wie etwa die Speicherkarte einer Kamera, eine DVD oder ein USB-Stick in den PC eingelegt bzw. daran angeschlossen wird. Sie finden die vollständige Liste dieser Ereignisse [hier](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |Die Einstellung für das Verb identifiziert einen Wert, der für Ihre Anwendung für die ausgewählte Option übergeben wird. Sie können mehrere Startaktionen für Ereignisse der automatischen Wiedergabe angeben und mit der Einstellung Verb ermitteln, welche Option ein Benutzer für Ihre App ausgewählt hat. Für welche Option sich der Benutzer entschieden hat, erfahren Sie durch Überprüfen der verb-Eigenschaft der an die App übergebenen Startereignisargumente. Für die Einstellung Verb können Sie einen beliebigen Wert verwenden. Einzige Ausnahme ist open: Dieser Wert ist reserviert. |
|DropTargetHandler |Die Klassen-ID der Anwendung, die implementiert die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) Schnittstelle. Dateien aus Wechselmedien werden an die [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__)-Methode Ihrer [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Implementierung übergeben.  |
|Parameter |Sie müssen die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Schnittstelle nicht für alle Inhaltsereignisse implementieren. Für jedes der Inhaltsereignisse können Sie Befehlszeilenparameter bereitstellen, anstatt die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Schnittstelle zu implementieren. Für solche Ereignisse werden automatische Wiedergabe mithilfe dieser Parameter über die Befehlszeile, um Ihre Anwendung starten. Sie können diese Parameter im Initialisierungscode Ihrer App analysieren, um festzustellen, ob sie von der automatischen Wiedergabe gestartet wurde, und dann Ihre benutzerdefinierte Implementierung bereitstellen. |
|DeviceEvent |Der Name eines Geräteereignisses, bei dem Benutzer mit Ihrem ``ActionDisplayName`` und ``ProviderDisplayName`` aufgefordert werden. Ein Geräteereignis wird ausgelöst, wenn ein Gerät an den PC angeschlossen wird. Geräteereignisse beginnen mit der Zeichenfolge ``WPD`` und sind [hier](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference) aufgelistet. |
|HWEventHandler |Die Klassen-ID der Anwendung, die implementiert die [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) Schnittstelle. |
|InitCmdLine |Der Zeichenfolgeparameter, der an die [Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize)-Methode der [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler)-Schnittstelle übergeben werden soll. |

### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Starten Sie nach Erhalt eines Updates vom Microsoft Store automatisch neu

Wenn Ihre Anwendung geöffnet ist, wenn Benutzer ein Update darauf installieren, wird die Anwendung geschlossen.

Wenn Sie möchten, dass die Anwendung neu starten, nachdem das Update abgeschlossen ist, rufen Sie die [RegisterApplicationRestart ist](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) -Funktion in jeder Prozess, der neu gestartet werden soll.

Jeder aktive Fenster in Ihrer Anwendung empfängt eine [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession) Nachricht. An diesem Punkt kann Ihre Anwendung Aufrufen der [RegisterApplicationRestart ist](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) Funktion erneut aus, um die Befehlszeile bei Bedarf aktualisieren.

Zeitpunkt, zu jedem aktiven Fenster in Ihrer Anwendung empfängt die [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) Nachricht, die Ihre Anwendung Daten speichern soll und heruntergefahren.

>[!NOTE]
Erhalten Sie auch Ihre aktiven Fenster der [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close) Meldung für den Fall, dass die Anwendung nicht behandelt die [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) Nachricht.

An diesem Punkt Ihrer Anwendung verfügt über 30 Sekunden zu seinem eigenen Prozesse schließen oder die Plattform beendet diese erzwungen.

Nachdem das Update abgeschlossen ist, startet die Anwendung neu.

## <a name="work-with-other-applications"></a>Zusammenarbeit mit anderen Anwendungen

Mit anderen Apps integrieren, andere Prozesse starten oder Informationen freigeben.

* [Stellen Sie Ihre Anwendung als Ziel in Anwendungen mit Unterstützung für Drucken Drucken](#printing)
* [Freigeben von Schriftarten für andere Windows-Anwendungen](#fonts)
* [Starten Sie einen Win32-Prozess über eine app für die universelle Windows-Plattform (UWP)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Stellen Sie Ihre Anwendung als Ziel in Anwendungen mit Unterstützung für Drucken Drucken

Wenn Benutzer Daten aus einer anderen Anwendung wie z. B. Editor gedruckt werden soll, können Sie Ihre Anwendung als drucken Ziel in der app-Liste der verfügbaren drucken Ziele angezeigt vornehmen.

Sie müssen Ihre Anwendung zu ändern, sodass der Empfang von print-Daten in XML Paper Specification (XPS)-Format.

#### <a name="xml-namespaces"></a>XML-Namespaces

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Die vollständige Schemareferenz finden Sie [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.appPrinter``.
|DisplayName |Der Name, der in der Liste der Druckerziele für eine App angezeigt werden soll. |
|Parameter |Parameter, die Ihre Anwendung benötigt, um die Anforderung ordnungsgemäß zu behandeln. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

Suchen Sie [hier](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF) ein Beispiel, in dem diese Erweiterung verwendet wird.

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>Freigeben von Schriftarten für andere Windows-Anwendungen

Teilen Sie Ihre benutzerdefinierten Schriftarten mit einer anderen Windows-Anwendungen.

#### <a name="xml-namespaces"></a>XML-Namespaces

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Die vollständige Schemareferenz finden Sie [hier](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.sharedFonts``.
|Datei |Die Datei mit der Schriftart, die Sie teilen möchten. |

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Starten Sie einen Win32-Prozess aus einer Universellen Windows-Plattform-App (UWP-App).

Starten Sie einen vertrauenswürdigen Win32-Prozess.

#### <a name="xml-namespaces"></a>XML-Namespaces

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fullTrustProcess``.
|GroupID |Eine Zeichenfolge, die eine Reihe von Parametern identifiziert, die an die ausführbare Datei übergeben werden sollen. |
|Parameter |Parameter, die an die ausführbare Datei übergeben werden sollen. |

#### <a name="example"></a>Beispiel

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Diese Erweiterung ist möglicherweise nützlich, wenn Sie eine universelle Windows Plattform-Benutzeroberfläche zu erstellen, die auf allen Geräten ausgeführt wird, aber Komponenten Ihrer Win32-Anwendung, um den Vorgang fortzusetzen, die in voller Vertrauenswürdigkeit ausgeführt werden sollen.

Erstellen Sie einfach ein Windows-app-Paket für die Win32-app. Fügen Sie dann der Paketdatei Ihrer UWP-App diese Erweiterung hinzu. Diese Erweiterungen gibt an, dass Sie eine ausführbare Datei in das Windows-app-Paket starten möchten.  Wenn Sie zwischen Ihrer UWP-App und Ihrer Win32-App kommunizieren möchten, können Sie einen oder mehrere [App-Dienste](/windows/uwp/launch-resume/app-services.md) dafür festlegen. Weitere Informationen zu diesem Szenario finden Sie [hier](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
