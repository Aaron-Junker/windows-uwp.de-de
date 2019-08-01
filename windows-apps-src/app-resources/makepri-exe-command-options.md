---
Description: MakePri.exe akzeptiert die Befehle createconfig, dump, new, resourcepack und versioned. In diesem Thema wird die Anwendung der Befehle erläutert.
title: Befehlszeilenoptionen für MakePri.exe
template: detail.hbs
ms.date: 04/10/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 24326c19688a0fa6f2300da50e6d13308b79ee2e
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682467"
---
# <a name="makepriexe-command-line-options"></a>Befehlszeilenoptionen für MakePri.exe

[MakePri.exe](compile-resources-manually-with-makepri.md) akzeptiert die Befehle `createconfig`, `dump`, `new`, `resourcepack` und `versioned`. In diesem Thema werden die Befehlszeilenoptionen für deren Verwendung erläutert.

> [!NOTE]
> Makepri. exe wird installiert, wenn Sie bei der Installation des Windows Software Development Kit die Option **Windows SDK für UWP verwaltete apps** aktivieren. Es wird im Pfad `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` installiert (sowie in Ordnern, die für die anderen Architekturen benannt sind). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`Hyper-V-Hosts oder Hyper-V-Hostcluster in einem separaten Namespace als verwaltete Hyper-V-Hosts hinzuzufügen.

## <a name="getting-help-from-the-command-line"></a>Hilfe von der Befehlszeile

Sie können oder `MakePri.exe help` `MakePri.exe /?` ausführen, um die Befehle anzuzeigen, die Sie mit makepri. exe verwenden können. Sie können auch ausgeben `MakePri.exe <command> /?` , um Einzelheiten zu einem Befehl anzuzeigen, und in sehr seltenen Fällen, `MakePri.exe <command> <option>` auch um Besonderheiten in Bezug auf eine Option anzuzeigen.

## <a name="makepri-commands"></a>MakePri-Befehle

```console
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Createconfig-Befehl

Der Befehl `createconfig` erstellt eine neue, initialisierte PRI-Konfigurationsdatei, in der die Standardwerte für die von Ihnen angegebenen Qualifizierer enthalten sind. Führen Sie `MakePri.exe createconfig /?` aus, um detaillierte Hilfe für diesen Befehl anzuzeigen.

```console
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Dump-Befehl

Der Befehl `dump` gibt den Dump einer XML-Datei aus, die eine Liste aller Ressourcen in einer angegebenen PRI-Datei enthält. Führen Sie `MakePri.exe dump /?` aus, um detaillierte Hilfe für diesen Befehl anzuzeigen.

> [!NOTE]
> Ein schemafreies Ressourcenpaket ist ein Ressourcenpaket, das mit dem Schalter *omitSchemaFromResourcePacks* in der PRI-Konfigurationsdatei erstellt wurde. Um ein schemafreies Ressourcenpaket zu sichern, verwenden Sie den Schalter `/es <main_package_PRI_file>`. Wenn Sie die Hauptdatei nicht angeben, wird die Fehlermeldung "die Ressourcen *. pri im Paket ist beschädigt, sodass die Verschlüsselung fehlgeschlagen ist" angezeigt (Fehler PRI222: 0xdef0000f-nicht angegebener Fehler*aufgetreten) ".

```console
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>New-Befehl

Der Befehl `new` erstellt eine neue PRI-Datei, indem er die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert. Führen Sie `MakePri.exe new /?` aus, um detaillierte Hilfe für diesen Befehl anzuzeigen.

```console
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>ResourcePack-Befehl

Der Befehl `resourcepack` erstellt eine neue PRI-Datei, indem er die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert. Eine Ressourcenpaket-PRI-Datei enthält nur zusätzliche Varianten von Ressourcen, die bereits in einer vorhandenen PRI-Datei angegeben sind. Führen Sie `MakePri.exe resourcepack /?` aus, um detaillierte Hilfe für diesen Befehl anzuzeigen.

```console
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Versioned-Befehl

Der Befehl `versioned` erstellt eine neue versionsverwaltete PRI-Datei, indem er die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert. Führen Sie `MakePri.exe versioned /?` aus, um detaillierte Hilfe für diesen Befehl anzuzeigen.

```console
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

Verwenden Sie die Erweiterungs-DLL-Option (/ex) mit `createconfig`, `dump`, `new`, `resourcepack` und `versioned`, um den Speicherort der Umgebungserweiterungs-DLL für das Ressourcenverwaltungssystem festzulegen.

## <a name="logging47metadata-file"></a>Logging&#47;Metadatendatei

MakePri kann Informationen zu einem Ressourcenpaket in die Indexer-Metadatendatei einfügen. Hier ein Beispiel einer Protokolldatei für `resources.pri` mit den PRI-Ressourcendateien `german.pri` und `highresolution.pri`:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>&#47;IndexFile(if) Option

Verwenden Sie die Indexdateioption (/if) mit `dump`, `resourcepack` und `versioned`, um eine PRI-Eingabedatei anzugeben.

Für `resourcepack` und `versioned` können Sie eine Schemadatei bereitstellen, statt eine PRI-Datei als Eingabeparameter für /IndexFile(if) anzugeben.

```console
/IndexFile(if) <FILEPATH>
```

**FILEPATH** ist ein Token, das den Speicherort der PRI-Eingabedatei oder PRI-Schemadatei angibt

## <a name="47indexoptionsio-option"></a>&#47;IndexOptions (IO)-Option

Sie verwenden die Option Index Optionen (/IO) mit `new`, `resourcepack`und `versioned` , um Optionen anzugeben, die eine ausführliche Kontrolle über das Verhalten von ressourcenindexer bereitstellen. Index Optionen sind standardmäßig deaktiviert.

```console
/IndexOptions(io) <OPTIONS>
```

Die **Optionen** sind eine durch Trennzeichen getrennte Liste, die die folgenden Optionen umfasst.

- +/-HiddenFiles (HF). Index (+) oder ignorieren (-) ausgeblendete Dateien und Ordner.
- +/-LinkedFiles (LF). Index (+) oder IGNORE (-) verknüpfte Dateien und Ordner.

## <a name="47mappingfilemf-option"></a>&#47;MappingFile(mf) Option

Verwenden Sie die Zuordnungsoption (/mf) mit `new`, `resourcepack` und `versioned`, um eine Zuordnungsdatei zu generieren. [MakeAppx.exe](/windows/msix/package/create-app-package-with-makeappx-tool) verwendet die Zuordnungsdatei zum Erzeugen von App-Paketen.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** ist ein Token, mit dem das Format der Zuordnungsdatei angegeben wird. Es wird nur das Format `appx` unterstützt.

```console
/mf appx
```

Hier ein Beispiel für Inhalte einer Hauptzuordnungsdatei:

```console
"ResourceDimensions"                   "language-de-de"
```

Und hier ein Beispiel für Inhalte einer Zuordnungsdatei für ein Ressourcenpaket:

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Ausgabezusammenfassung

Wenn Ressourcenpakete erstellt werden, ist die Ausgabezusammenfassung von MakePRI.exe ausführlicher. Hier sehen Sie ein Beispiel.

```console
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>&#47;Overwrite(o) Option

Wenn die Option zum Überschreiben (/o) fehlt und die angegebenen Ausgabedateien bereits vorhanden sind, benötigt MakePri.exe zum Überschreiben eine Bestätigung.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;OutputFile(of) Option

Verwenden Sie die Ausgabedateioption (/of) mit `dump`, `new`, `resourcepack` und `versioned`, um den Ausgabespeicherort und den Namen der zu generierenden PRI-Datei anzugeben. Sollte MakePri.exe mehrere PRI-Ressourcendateien generieren, werden diese im übergeordneten Ordner der Zieldatei abgelegt. Wenn Sie beispielsweise `/of MyParentFolder\TargetFile.pri` angeben, generiert MakePri.exe `TargetFile.language-en.pri` und `TargetFile.scale-100.pri` zusammen mit `TargetFile.pri` unter `ParentFolder`.

Hier ein Beispiel für eine Fehlerbedingung und die entsprechende Fehlermeldung:

| Fehlerbedingung | Fehlermeldung |
| --------------- | ------------- |
| Der Name der Ausgabedatei entspricht einem der Ressourcenpaketnamen in der Konfiguration. | Ungültige Konfiguration: Der Name <resource pack name> des Ressourcen Pakets darf nicht mit der Ausgabedatei < OutputFileName. pri > identisch sein. |

## <a name="reversemaprm-option"></a>/ReverseMap(rm) Option

Sie können die Option zum Umkehren der Zuordnung (/rm) mit `new`, `resourcepack` und `versioned` verwenden, um in der PRI-Datei einen Abschnitt mit umgekehrter Zuordnung zu generieren, der zum Debuggen verwendet werden kann.

## <a name="47schemafilesf-option"></a>&#47;SchemaFile(sf) Option

Verwenden Sie die Schemadateioption (/sf) mit `new`, `resourcepack` und `versioned`, um eine Schemadatei am angegebenen Speicherort zu schreiben.

Für `resourcepack` und `versioned` können Sie eine Schemadatei bereitstellen, statt eine PRI-Datei als Eingabeparameter für /IndexFile(if) anzugeben.

```console
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** ist ein Token, das angibt, an welchem Speicherort die Schemadatei geschrieben werden soll.

Hier ein Beispiel für eine Datendatei:

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor(vma) ist veraltet

Die Option für die Hauptversion (/vma) (für den Befehl `new`) ist veraltet, und bei der Verwendung der Option wird folgende Warnmeldung angezeigt:

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Verwenden Sie stattdessen in Ihrer Konfigurationsdatei das Attribut [resources@majorVersion](makepri-exe-configuration.md), um die Hauptversionsnummer anzugeben.

## <a name="related-topics"></a>Verwandte Themen

* [Makepri. exe](compile-resources-manually-with-makepri.md)
