---
description: Du kannst Erweiterungen nutzen, um deine gepackte Desktop-App in vordefinierter Weise in Windows 10 zu integrieren.
title: Modernisieren vorhandener Desktop-Apps mit Desktop-Brücke
ms.date: 08/25/2020
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: fb1daddeb743909417d6483223d5386e64ca5241
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942780"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>Integrieren deiner Desktop-App in Windows 10 und UWP

Wenn deine Desktop-App eine [Paketidentität](modernize-packaged-apps.md) umfasst, kannst du deine App mithilfe von Erweiterungen in Windows 10 integrieren, indem du vordefinierte Erweiterungen im [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) nutzt.

Verwende beispielsweise eine Erweiterung, um eine Firewallausnahme festzulegen, lege deine App als Standard-App für einen Dateityp fest, oder verweise mit Startkacheln auf deine App. Um eine Erweiterung zu verwenden, fügst du einfach etwas XML zur Paketmanifestdatei deiner App hinzu. Es ist kein Code erforderlich.

In diesem Thema werden diese Erweiterungen und die Aufgaben beschrieben, die du mithilfe dieser Erweiterungen ausführen kannst.

> [!NOTE]
> Für die im vorliegenden Artikel beschriebenen Features muss deine Desktop-App eine [Paketidentität](modernize-packaged-apps.md) aufweisen, entweder durch das [Packen deiner Desktop-App in einem MSIX-Paket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) oder durch das [Gewähren einer App-Identität durch Verwendung eines Pakets mit geringer Datendichte](grant-identity-to-nonpackaged-apps.md).

## <a name="transition-users-to-your-app"></a>Benutzerumstellung auf deine App

Unterstütze die Benutzer bei der Umstellung auf deine gepackte App.

* [Verweis auf die gepackte App mit vorhandenen Startkacheln und Taskleistenschaltflächen](#point)
* [Festlegen deiner gepackten Anwendung zum Öffnen von Dateien anstelle deiner Desktop-App](#make)
* [Zuordnen einer gepackten Anwendung zu einer Gruppe von Dateitypen](#associate)
* [Hinzufügen von Optionen zu den Kontextmenüs von Dateien eines bestimmten Dateityps](#add)
* [Öffnen bestimmter Dateitypen direkt über eine URL](#open)

<a id="point"></a>

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Verweis auf die gepackte App mit vorhandenen Startkacheln und Taskleistenschaltflächen

Deine Benutzer haben möglicherweise deine Desktop-Anwendung an die Taskleiste oder das Startmenü angeheftet. Du kannst in diesen Verknüpfungen auf deine neue gepackte App verweisen.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Name | Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.desktopAppMigration``.
|AumID |Die Anwendungsbenutzermodell-ID deiner gepackten App. |
|ShortcutPath |Der Pfad zu den INK-Dateien, mit denen die Desktopversion deiner App gestartet wird. |

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

#### <a name="related-sample"></a>Verwandtes Beispiel

[WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make"></a>

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Festlegen deiner gepackten Anwendung zum Öffnen von Dateien anstelle deiner Desktop-App

Du kannst sicherstellen, dass die Benutzer zum Öffnen bestimmter Dateitypen standardmäßig deine neue gepackte App anstelle der Desktopversion deiner App verwenden.

Dazu gibst du den [programmatischen Bezeichner (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) jeder Anwendung an, von der du Dateizuordnungen übernehmen möchtest.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|MigrationProgId |Der [programmatische Bezeichner (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids), der die Anwendung, die Komponente und die Version der Desktopanwendung beschreibt, aus der die Dateizuordnungen übernommen werden sollen.|

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

#### <a name="related-sample"></a>Verwandtes Beispiel

[WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate"></a>

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Zuordnen einer gepackten Anwendung zu einer Gruppe von Dateitypen

Du kannst deine gepackte App bestimmten Dateityperweiterungen zuordnen. Wenn ein Benutzer mit der rechten Maustaste auf eine Datei klickt und **Öffnen mit** auswählt, wird deine App in der Vorschlagsliste angezeigt.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name | Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen.   |
|FileType |Die Dateierweiterung, die von deiner App unterstützt wird. |

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
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Verwandtes Beispiel

[WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add"></a>

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Hinzufügen von Optionen zu den Kontextmenüs von Dateien eines bestimmten Dateityps

In den meisten Fällen doppelklicken Benutzer auf Dateien, um sie zu öffnen. Wenn Benutzer mit der rechten Maustaste auf eine Datei klicken, werden verschiedene Optionen angezeigt.

Du kannst diesem Menü Optionen hinzufügen. Diese Optionen geben Benutzern weitere Möglichkeiten zur Interaktion mit der Datei, beispielsweise durch Optionen zum Drucken, Bearbeiten oder zum Anzeigen einer Vorschau.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie | Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|Verb |Der Name, der im Kontextmenü des Datei-Explorers angezeigt wird. Diese Zeichenfolge kann mithilfe von ```ms-resource``` lokalisiert werden.|
|Id |Die eindeutige ID des Verbs. Bei UWP-Apps wird sie im Rahmen der Aktivierungsereignisargumente übergeben, um eine ordnungsgemäße Verarbeitung der Benutzerauswahl zu ermöglichen. Bei gepackten, als vollständig vertrauenswürdig eingestuften Apps hingegen werden Parameter übergeben (siehe nächster Punkt in der Auflistung). |
|Parameter |Die Liste mit Argumentparametern und -werten für das Verb. Wenn deine Anwendung eine als vertrauenswürdig eingestufte gepackte App ist, werden diese Parameter bei der Aktivierung der Anwendung als Ereignisargumente an die Anwendung übergeben. Du kannst das Verhalten deiner App auf Basis anderer Aktivierungsverben anpassen. Wenn eine Variable einen Dateipfad enthalten kann, schließe den Parameterwert in Anführungszeichen ein. So werden Probleme vermieden, die bei Pfaden mit Leerzeichen auftreten können. Wenn deine App eine UWP-App ist, kannst du keine Parameter übergeben. Die App empfängt stattdessen die ID (siehe vorheriger Punkt in der Auflistung).|
|Erweitert |Gibt an, dass das Verb nur angezeigt werden soll, wenn der Benutzer zum Anzeigen des Kontextmenüs die **UMSCHALTTASTE** gedrückt hält, bevor er mit der rechten Maustaste auf die Datei klickt. Dieses Attribut ist optional und standardmäßig auf den Wert **FALSE** (Verb soll immer angezeigt werden) festgelegt. Dieses Verhalten muss für jedes Verb einzeln angegeben werden – mit Ausnahme von „Öffnen“: Bei diesem Verb ist der Wert immer **False**.|

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

#### <a name="related-sample"></a>Verwandtes Beispiel

[WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open"></a>

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Öffnen bestimmter Dateitypen direkt über eine URL

Du kannst sicherstellen, dass die Benutzer zum Öffnen bestimmter Dateitypen standardmäßig deine neue gepackte App anstelle der Desktopversion deiner App verwenden.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|UseUrl |Gibt an, ob Dateien direkt über ein URL-Ziel geöffnet werden sollen. Wenn du diesen Wert nicht festlegst, lädt das System die Datei zunächst lokal herunter, wenn deine App versucht, die Datei über eine URL zu öffnen. |
|Parameter | Optionale Parameter. |
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
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
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

## <a name="perform-setup-tasks"></a>Ausführen von Einrichtungsaufgaben

* [Erstellen von Firewallausnahmen für deine App](#rules)
* [Platzieren deiner DLL-Dateien in einem beliebigen Ordner des Pakets](#load-paths)

<a id="rules"></a>

### <a name="create-firewall-exception-for-your-app"></a>Erstellen von Firewallausnahmen für deine App

Wenn deine App über einen Port kommunizieren muss, kannst du deine App zur Liste der Firewallausnahmen hinzufügen.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

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

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Name |Beschreibung |
|-------|-------------|
|Kategorie |Immer ``windows.firewallRules``.|
|Ausführbare Datei |Der Name der ausführbaren Datei, die zur Liste der Firewallausnahmen hinzugefügt werden soll. |
|Direction |Gibt an, ob es sich um eine Regel für eingehenden oder ausgehenden Datenverkehr handelt. |
|IPProtocol |Das Kommunikationsprotokoll. |
|LocalPortMin |Die niedrigste Portnummer in einem Bereich lokaler Ports. |
|LocalPortMax |Die höchste Portnummer in einem Bereich lokaler Ports. |
|RemotePortMax |Die niedrigste Portnummer in einem Bereich von Remoteports. |
|RemotePortMax |Die höchste Portnummer in einem Bereich von Remoteports. |
|Profil |Der Netzwerktyp. |

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

<a id="load-paths"></a>

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Platzieren deiner DLL-Dateien in einem beliebigen Ordner des Pakets

Verwenden Sie die Erweiterung [uap6:LoaderSearchPathOverride](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride), um bis zu fünf Ordnerpfade im App-Paket relativ zum Stammpfad des App-Pakets zu deklarieren, die im Suchpfad des Ladeprogramms für die Prozesse der App verwendet werden sollen.

Die [DLL-Suchreihenfolge](https://docs.microsoft.com/windows/win32/dlls/dynamic-link-library-search-order) für Windows-Apps enthält Pakete im Paketabhängigkeitsdiagramm, wenn die Pakete über Ausführungsrechte verfügen. Standardmäßig umfasst dies Haupt-, optionale und Frameworkpakete, obwohl dies durch das [uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)-Element im Paketmanifest überschrieben werden kann.

Ein Paket, das in der DLL-Suchreihenfolge enthalten ist, enthält standardmäßig seinen *effektiven Pfad*. Weitere Informationen zu effektiven Pfaden finden Sie unter der [EffectivePath](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.effectivepath)-Eigenschaft (WinRT) und der [PackagePathType](https://docs.microsoft.com/windows/win32/api/appmodel/ne-appmodel-packagepathtype)-Enumeration (Win32).

Wenn ein Paket [uap6:LoaderSearchPathOverride](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) angibt, werden diese Informationen anstelle des effektiven Pfads des Pakets verwendet.

Jedes Paket kann nur eine [uap6:LoaderSearchPathOverride](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride)-Erweiterung enthalten. Das bedeutet, du kannst deinem Hauptpaket und dann jedem deiner [optionalen Pakete und zugehörigen Sätzen](/windows/msix/package/optional-packages) eine Erweiterung hinzufügen.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

Deklariere diese Erweiterung auf der Paketebene deines App-Manifests.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Name | BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.loaderSearchPathOverride``.
|FolderPath | Der Pfad des Ordners, der Ihre DLL-Dateien enthält. Gib einen Pfad relativ zum Stammordner des Pakets an. Du kannst bis zu fünf Pfade in einer Erweiterung angeben. Wenn das System nach Dateien im Stammordner des Pakets suchen soll, verwende eine leere Zeichenfolge für einen dieser Pfade. Fügen Sie keine doppelten Pfade ein, und stellen Sie sicher, dass die Pfade keine voran- bzw. nachgestellten Schrägstriche oder umgekehrten Schrägstriche enthalten. <br><br> Das System durchsucht keine Unterordner. Stelle deshalb sicher, dass du jeden Ordner mit DLL-Dateien, die das System laden soll, explizit auflistest.|

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

## <a name="integrate-with-file-explorer"></a>Integration in den Datei-Explorer

Unterstütze Benutzer beim Organisieren deiner Dateien und interagiere auf vertraute Weise mit ihnen.

* [Definieren des Anwendungsverhaltens, wenn Benutzer mehrere Dateien gleichzeitig auswählen und öffnen](#define)
* [Anzeigen von Dateiinhalten in einem Miniaturbild im Datei-Explorer](#show)
* [Anzeigen von Dateiinhalten im Vorschaubereich des Datei-Explorers](#preview)
* [Ermöglichen der Gruppierung von Dateien mithilfe der Spalte „Art“ im Datei-Explorer](#enable)
* [Bereitstellen von Dateieigenschaften für Suche, Index, Eigenschaftendialogfelder und Detailbereich](#make-file-properties)
* [Angeben eines Kontextmenühandlers für einen Dateityp](#context-menu)
* [Anzeigen von Dateien aus deinem Clouddienst im Datei-Explorer](#cloud-files)

<a id="define"></a>

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definieren des Anwendungsverhaltens, wenn Benutzer mehrere Dateien gleichzeitig auswählen und öffnen

Gib an, wie sich die App verhält, wenn ein Benutzer mehrere Dateien gleichzeitig öffnet.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|MultiSelectModel |Siehe unten |
|FileType |Die relevanten Dateierweiterungen. |

**MultiSelectModel**

Bei gepackten Desktop-Apps stehen die gleichen drei Optionen zur Verfügung wie bei regulären Desktop-Apps.

* ``Player``: Deine Anwendung wird einmalig aktiviert. Alle der ausgewählten Dateien werden als Argumentparameter an deine Anwendung übergeben.
* ``Single``: Deine App wird einmalig für die erste ausgewählte Datei aktiviert. Andere Dateien werden ignoriert.
* ``Document``: Für jede ausgewählte Dateien wird jeweils eine neue (separate) Instanz deiner Anwendung aktiviert.

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
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
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

Wenn der Benutzer 15 oder weniger Dateien öffnet, lautet die Standardauswahl des **MultiSelectModel**-Attributs *Player*. Andernfalls lautet der Standardwert *Document*. UWP-Apps werden immer als *Player* gestartet.

<a id="show"></a>

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Anzeigen von Dateiinhalten in einem Miniaturbild im Datei-Explorer

Ermögliche es Benutzern, eine Miniaturansicht des Dateiinhalts anzuzeigen, wenn für das Dateisymbol „Mittelgroße Symbole“, „Große Symbole“ oder „Extra große Symbole“ festgelegt ist.

#### <a name="xml-namespace"></a>XML-Namespace

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid   |Die Klassen-ID deiner App. |

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="preview"></a>

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Anzeigen von Dateiinhalten im Vorschaubereich des Datei-Explorers

Ermögliche es Benutzern, die Inhalte einer Datei im Vorschaubereich des Datei-Explorers anzuzeigen.

#### <a name="xml-namespace"></a>XML-Namespace

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid   |Die Klassen-ID deiner App. |

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="enable"></a>

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Ermöglichen der Gruppierung von Dateien mithilfe der Spalte „Art“ im Datei-Explorer

Du kannst einen oder mehrere vordefinierte Werte für die Dateitypen mit dem **Kind**-Feld zuordnen.

Im Datei-Explorer können Benutzer diese Dateien mithilfe dieses Felds gruppieren. Systemkomponenten verwenden dieses Feld auch für andere Zwecke, beispielsweise für die Indizierung.

Weitere Informationen zum **Kind**-Feld und den Werten, die du für dieses Feld verwenden kannst, findest du unter [Verwenden von Kind-Namen](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|FileType |Die relevanten Dateierweiterungen. |
|value |Ein gültiger [Kind-Wert](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) |

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
           <uap:FileTypeAssociation Name="mediafiles">
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

<a id="make-file-properties"></a>

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Bereitstellen von Dateieigenschaften für Suche, Index, Eigenschaftendialogfelder und Detailbereich

#### <a name="xml-namespace"></a>XML-Namespace

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fileTypeAssociation``.
|Name |Der Name der Dateitypzuordnung. Mit diesem Namen kannst du Dateitypen organisieren und gruppieren. Der Name darf nur Kleinbuchstaben und keine Leerzeichen umfassen. |
|FileType |Die relevanten Dateierweiterungen. |
|Clsid  |Die Klassen-ID deiner App. |

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="context-menu"></a>

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Angeben eines Kontextmenühandlers für einen Dateityp

Wenn deine Desktopanwendung einen [Kontextmenühandler](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers) definiert, verwende diese Erweiterung zum Registrieren des Menühandlers.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

Die vollständige Schemareferenz findest du hier: [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) und [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instructions

Folge diesen Anweisungen, um deinen Kontextmenühandler zu registrieren.

1. Implementiere in deiner Desktopanwendung einen [Kontextmenühandler](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers), indem du die Schnittstelle [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) oder [IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate) implementierst. Siehe hierzu das Codebeispiel [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb). Stelle sicher, dass du eine Klassen-GUID für jedes deiner Implementierungsobjekte definierst. Beispielsweise definiert der folgende Code eine Klassen-ID für eine Implementierung von [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. Gib in deinem Paketmanifest eine [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver)-Anwendungserweiterung an, die einen COM-Ersatzserver mit der Klassen-ID der Implementierung deines Kontextmenühandlers registriert.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. Gib in deinem Paketmanifest eine [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)-Anwendungserweiterung an, die die Implementierung deines Kontextmenühandlers registriert.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>Beispiel

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files"></a>

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Anzeigen von Dateien aus deinem Clouddienst im Datei-Explorer

Registriere die Handler, die du in deiner Anwendung implementierst. Du kannst außerdem Kontextmenüoptionen hinzufügen, die angezeigt werden, wenn deine Benutzer im Datei-Explorer mit der rechten Maustaste auf die cloudbasierten Dateien klicken.

#### <a name="xml-namespace"></a>XML-Namespace

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.cloudfiles``.
|iconResource |Das Symbol, das deinen Clouddateianbieter-Dienst repräsentiert. Dieses Symbol wird im Navigationsbereich des Datei-Explorers angezeigt.  Benutzer wählen dieses Symbol aus, um Dateien aus deinem Clouddienst anzuzeigen. |
|CustomStateHandler Clsid |Die Klassen-ID der App, die den CustomStateHandler implementiert. Das System verwendet diese Klassen-ID, um benutzerdefinierte Statuswerte und Spalten für Clouddateien anzufordern. |
|ThumbnailProviderHandler Clsid |Die Klassen-ID der App, die den ThumbnailProviderHandler implementiert. Das System verwendet diese Klassen-ID, um Miniaturansichten für Clouddateien anzufordern. |
|ExtendedPropertyHandler Clsid |Die Klassen-ID der App, die den ExtendedPropertyHandler implementiert.  Das System verwendet diese Klassen-ID, um erweiterte Eigenschaften für eine Clouddatei anzufordern. |
|Verb |Der Name, der im Kontextmenü des Datei-Explorers für Dateien angezeigt wird, die von deinem Clouddienst bereitgestellt werden. |
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

<a id="start"></a>

## <a name="start-your-application-in-different-ways"></a>Starten deiner Anwendung auf unterschiedlicher Weise

* [Starten deiner Anwendung über ein Protokoll](#protocol)
* [Starten deiner Anwendung über einen Alias](#alias)
* [Starten einer ausführbaren Datei, wenn sich Benutzer bei Windows anmelden](#executable)
* [Ermöglichen des Anwendungsstarts beim Verbinden eines Geräts mit dem Benutzer-PC](#autoplay)
* [Automatischer Neustart nach dem Empfang eines Updates aus dem Microsoft Store](#updates)

<a id="protocol"></a>

### <a name="start-your-application-by-using-a-protocol"></a>Starten deiner Anwendung über ein Protokoll

Protokollzuordnungen ermöglichen es anderen Programmen und Systemkomponenten, mit deiner gepackten App zu interagieren. Wenn deine gepackte App über ein Protokoll gestartet wird, kannst du bestimmte Parameter zur Übergabe an die Aktivierungsereignisargumente angeben, um ein entsprechendes Verhalten zu erreichen. Parameter werden nur für gepackte, als vertrauenswürdig eingestufte Apps unterstützt. UWP-Apps können keine Parameter verwenden.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.protocol``.
|Name |Der Name des Protokolls. |
|Parameter |Die Liste der Parameter und Werte, die bei der Aktivierung deiner App als Ereignisargumente an deine App übergeben werden sollen. Wenn eine Variable einen Dateipfad enthalten kann, schließe den Parameterwert in Anführungszeichen ein. So werden Probleme vermieden, die bei Pfaden mit Leerzeichen auftreten können. |

### <a name="example"></a>Beispiel

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
```

<a id="alias"></a>

### <a name="start-your-application-by-using-an-alias"></a>Starten deiner Anwendung über einen Alias

Benutzer und andere Prozesse können einen Alias verwenden, um deine App zu starten, ohne den vollständigen Pfad zu deiner App angeben zu müssen. Du kannst diesen Aliasnamen angeben.

#### <a name="xml-namespaces"></a>XML-Namespaces

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.appExecutionAlias``.
|Ausführbare Datei |Der relative Pfad zur ausführbaren Datei, die beim Aufrufen des Alias gestartet wird. |
|Alias |Der Kurzname für deine App. Er muss immer mit der Erweiterung „.exe“ enden. Für die einzelnen Anwendungen im Paket kann immer nur einzelner App-Ausführungsalias angegeben werden. Wenn sich mehrere Apps mit dem gleichen Alias registrieren, ruft das System die zuletzt registrierte App auf. Wählen Sie daher einen eindeutigen Alias, um die Wahrscheinlichkeit einer Überschreibung durch andere Apps möglichst gering zu halten.
|

#### <a name="example"></a>Beispiel

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

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable"></a>

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Starten einer ausführbaren Datei, wenn sich Benutzer bei Windows anmelden

Startaufgaben ermöglichen deiner App das automatische Ausführen einer ausführbaren Datei, wenn sich ein Benutzer anmeldet.

> [!NOTE]
> Der Benutzer muss deine App mindestens einmal starten, um diese Startaufgabe zu registrieren.

Deine App kann mehrere Startaufgaben deklarieren. Die Aufgaben werden unabhängig voneinander gestartet. Alle Startaufgaben werden im Task-Manager auf der Registerkarte **Autostart** mit dem Namen aus deinem App-Manifest und dem Symbol deiner App angezeigt. Der Task-Manager analysiert automatisch die Startauswirkungen Ihrer Aufgaben.

Benutzer können die Startaufgabe Ihrer App manuell mithilfe des Task-Managers deaktivieren. Wenn ein Benutzer eine Aufgabe deaktiviert, kannst du sie nicht programmgesteuert reaktivieren.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.startupTask``.|
|Ausführbare Datei |Der relative Pfad der ausführbaren Datei, die gestartet werden soll. |
|TaskId |Ein eindeutiger Bezeichner für deine Aufgabe. Mit diesem Bezeichner kann deine App die APIs in der [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask)-Klasse aufrufen, um eine Startaufgabe programmgesteuert zu aktivieren oder zu deaktivieren. |
|Aktiviert |Gibt an, ob die Aufgabe zunächst aktiviert oder deaktiviert wird. Aktivierte Aufgaben werden bei der nächsten Anmeldung des Benutzers ausgeführt (es sei denn, der Benutzer deaktiviert sie). |
|DisplayName |Der Name der Aufgabe, die im Task-Manager angezeigt wird. Du kannst diese Zeichenfolge mit ```ms-resource``` lokalisieren. |

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

<a id="autoplay"></a>

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Ermöglichen des Anwendungsstarts beim Verbinden eines Geräts mit dem Benutzer-PC

Du kannst deine Anwendung als Option für die automatische Wiedergabe anzeigen, wenn ein Benutzer ein Gerät an seinen PC anschließt.

#### <a name="xml-namespace"></a>XML-Namespace

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

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

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.autoPlayHandler``.
|ActionDisplayName |Diese Zeichenfolge repräsentiert die Aktion, die Benutzer für ein Gerät ausführen können, das sie an einen PC anschließen (beispielsweise „Dateien importieren“ oder „Video wiedergeben“). |
|ProviderDisplayName | Eine Zeichenfolge, die deine App oder deinen Dienst repräsentiert (beispielsweise „Contoso-Videoplayer“). |
|ContentEvent |Der Name eines Inhaltsereignisses, durch das Benutzer eine Aufforderung zu deinem ``ActionDisplayName`` und ``ProviderDisplayName`` erhalten. Ein Inhaltsereignis wird ausgelöst, wenn ein Volumegerät wie etwa die Speicherkarte einer Kamera, eine DVD oder ein USB-Stick in den PC eingelegt bzw. daran angeschlossen wird. Die vollständige Liste dieser Ereignisse findest du [hier](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |Die Einstellung „Verb“ dient zum Angeben eines Werts, der für die ausgewählte Option an deine App übergeben wird. Sie können mehrere Startaktionen für Ereignisse der automatischen Wiedergabe angeben und mit der Einstellung Verb ermitteln, welche Option ein Benutzer für Ihre App ausgewählt hat. Für welche Option sich der Benutzer entschieden hat, erfahren Sie durch Überprüfen der verb-Eigenschaft der an die App übergebenen Startereignisargumente. Für die Einstellung Verb können Sie einen beliebigen Wert verwenden. Einzige Ausnahme ist open: Dieser Wert ist reserviert. |
|DropTargetHandler |Die Klassen-ID der Anwendung, die die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Schnittstelle implementiert. Dateien von Wechselmedien werden an die [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__)-Methode deiner [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Implementierung übergeben.  |
|Parameter |Du musst die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Schnittstelle nicht für alle Inhaltsereignisse implementieren. Für jedes der Inhaltsereignisse kannst du Befehlszeilenparameter bereitstellen, anstatt die [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)-Schnittstelle zu implementieren. Bei diesen Ereignissen verwendet die automatische Wiedergabe diese Befehlszeilenparameter, um deine App zu starten. Du kannst durch eine Analyse dieser Parameter im Initialisierungscode deiner App festzustellen, ob sie von der automatischen Wiedergabe gestartet wurde, und dann deine benutzerdefinierte Implementierung bereitstellen. |
|DeviceEvent |Der Name eines Geräteereignisses, bei dem Benutzer eine Aufforderung zu deinem ``ActionDisplayName`` und ``ProviderDisplayName`` erhalten. Ein Geräteereignis wird ausgelöst, wenn ein Gerät an den PC angeschlossen wird. Geräteereignisse beginnen mit der Zeichenfolge ``WPD`` und sind [hier](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference) aufgelistet. |
|HWEventHandler |Die Klassen-ID der App, die die [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler)-Schnittstelle implementiert. |
|InitCmdLine |Der Zeichenfolgenparameter, den du an die [Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize)-Methode der [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler)-Schnittstelle übergeben möchtest. |

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

<a id="updates"></a>

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Automatischer Neustart nach dem Empfang eines Updates aus dem Microsoft Store

Wenn deine Anwendung geöffnet ist, wenn Benutzer ein Update installieren, wird die Anwendung geschlossen.

Wenn du möchtest, dass die Anwendung nach Abschluss des Updates neu gestartet wird, rufe die [RegisterApplicationRestart ist](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart)-Funktion in jedem Prozess auf, der neu gestartet werden soll.

Jedes aktive Fenster in deiner Anwendung empfängt eine [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession)-Nachricht. An diesem Punkt kann deine Anwendung erneut die [RegisterApplicationRestart ist](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart)-Funktion aufrufen, um die Befehlszeile bei Bedarf zu aktualisieren.

Wenn jedes aktive Fenster in deiner Anwendung die [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession)-Nachricht empfängt, sollte die Anwendung die Daten speichern und herunterfahren.

>[!NOTE]
> Die aktiven Fenster empfangen außerdem die [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close)-Nachricht, falls die Anwendung die [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession)-Nachricht nicht verarbeitet.

An diesem Punkt hat deine Anwendung 30 Sekunden Zeit, um die eigenen Prozesse zu schließen. Andernfalls wird deren Beendigung durch die Plattform erzwungen.

Nachdem das Update abgeschlossen ist, wird deine App neu gestartet.

## <a name="work-with-other-applications"></a>Zusammenarbeit mit anderen Anwendungen

Führe eine Integration in andere Apps durch, starte weitere Prozesse, oder teile Informationen.

* [Anzeigen deiner Anwendung als Druckziel in Anwendungen mit Druckunterstützung](#printing)
* [Freigeben von Schriftarten für andere Windows-Anwendungen](#fonts)
* [Starten eines Win32-Prozesses aus einer UWP-App](#win32-process)

<a id="printing"></a>

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Anzeigen deiner Anwendung als Druckziel in Anwendungen mit Druckunterstützung

Wenn Benutzer Daten aus einer anderen Anwendung wie z. B. Editor drucken möchten, kannst du deine App als Druckziel in der Liste der verfügbaren Druckziele der App anzeigen lassen.

Du musst deine Anwendung so einrichten, dass sie Daten im XPS-Format (XML Paper Specification) empfängt.

#### <a name="xml-namespaces"></a>XML-Namespaces

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Die vollständige Schemareferenz findest du [hier](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.appPrinter``.
|DisplayName |Der Name, der in der Liste der Druckziele für eine App angezeigt werden soll. |
|Parameter |Alle Parameter, die deine App zur ordnungsgemäßen Verarbeitung der Anforderung benötigt. |

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

Ein Beispiel zur Verwendung dieser Erweiterung findest du [hier](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF).

<a id="fonts"></a>

### <a name="share-fonts-with-other-windows-applications"></a>Freigeben von Schriftarten für andere Windows-Anwendungen

Gibt deine benutzerdefinierten Schriftarten für andere Windows-Anwendungen frei.

#### <a name="xml-namespaces"></a>XML-Namespaces

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Die vollständige Schemareferenz findest du [hier](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.sharedFonts``.
|Datei |Die Datei mit der Schriftart, die du freigeben möchtest. |

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

<a id="win32-process"></a>

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Starten eines Win32-Prozesses aus einer UWP-App

Starte einen Win32-Prozess, der als vollständig vertrauenswürdig eingestuft wird.

#### <a name="xml-namespaces"></a>XML-Namespaces

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elemente und Attribute dieser Erweiterung

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Name |BESCHREIBUNG |
|-------|-------------|
|Category |Immer ``windows.fullTrustProcess``.
|GroupID |Eine Zeichenfolge zum Identifizieren einer Reihe von Parametern, die du an die ausführbare Datei übergeben möchtest. |
|Parameter |Parameter, die du an die ausführbare Datei übergeben möchtest. |

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

Diese Erweiterung kann nützlich sein, wenn du eine UWP-Benutzerschnittstelle erstellen möchtest, die auf allen Geräten ausgeführt werden kann, die Komponenten deiner Win32-Anwendung aber weiterhin als vollständig vertrauenswürdig eingestuft werden sollen.

Erstelle einfach ein Windows-App-Paket für deine Win32-App. Füge diese Erweiterung dann der Paketdatei deiner UWP-App hinzu. Diese Erweiterungen gibt an, dass du eine ausführbare Datei im Windows-App-Paket starten möchtest.  Wenn du eine Kommunikation zwischen deiner UWP-App und deiner Win32-App ermöglichen möchtest, kannst du hierzu einen oder mehrere [App-Dienste](/windows/uwp/launch-resume/app-services) festlegen. Weitere Informationen zu diesem Szenario findest du [hier](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Nächste Schritte

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.
