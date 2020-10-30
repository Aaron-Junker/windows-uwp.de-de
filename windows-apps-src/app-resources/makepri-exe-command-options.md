---
description: MakePri.exe verfügt über die Befehle "kreateconfig", "Dump", "New", "resourcepack" und "versioniert". In diesem Thema wird deren Verwendung ausführlich erläutert.
title: Befehlszeilenoptionen für „MakePri.exe“
template: detail.hbs
ms.date: 04/10/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 7443efbb227bf3f9ea64db58902ebeb67b02f676
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031733"
---
# <a name="makepriexe-command-line-options"></a>Befehlszeilenoptionen für „MakePri.exe“

[MakePri.exe](compile-resources-manually-with-makepri.md) verfügt über die Befehle,,, `createconfig` `dump` `new` `resourcepack` und `versioned` . In diesem Thema werden die Befehlszeilenoptionen für ihre Verwendung ausführlich erläutert.

> [!NOTE]
> MakePri.exe wird installiert, wenn Sie bei der Installation des Windows Software Development Kit die Option **Windows SDK für verwaltete UWP-apps** aktivieren. Es wird im Pfad installiert `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (sowie in Ordnern, die für die anderen Architekturen benannt sind). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="getting-help-from-the-command-line"></a>Hilfe von der Befehlszeile

Sie können `MakePri.exe help` oder Ausführen `MakePri.exe /?` , um die Befehle anzuzeigen, die Sie mit MakePri.exe verwenden können. Sie können auch ausgeben, `MakePri.exe <command> /?` um Einzelheiten zu einem Befehl anzuzeigen, und in sehr seltenen Fällen, auch `MakePri.exe <command> <option>` um Besonderheiten in Bezug auf eine Option anzuzeigen.

## <a name="makepri-commands"></a>Makepri-Befehle

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

## <a name="createconfig-command"></a>Befehl "| ateconfig"

Der `createconfig` Befehl erstellt eine neue, initialisierte PRI-Konfigurationsdatei, die die von Ihnen angegebenen qualifiziererstandardwerte definiert. Führen `MakePri.exe createconfig /?` Sie aus, um ausführliche Hilfe zu diesem Befehl anzuzeigen.

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

Der `dump` Befehl gibt eine abgegebene XML-Datei aus, die eine Liste aller Ressourcen in einer angegebenen PRI-Datei enthält. Führen `MakePri.exe dump /?` Sie aus, um ausführliche Hilfe zu diesem Befehl anzuzeigen.

> [!NOTE]
> Ein Schema freies Ressourcenpaket, das mit dem Schalter *omitschemafromresourcepacks* in der PRI-Konfigurationsdatei erstellt wurde. Verwenden Sie zum Sichern eines Ressourcen Pakets mit Schema freiem Schema den-Schalter `/es <main_package_PRI_file>` . Wenn Sie die Hauptdatei nicht angeben, wird die Fehlermeldung " *die Ressourcen. pri im Paket ist beschädigt, sodass die Verschlüsselung fehlgeschlagen ist (Fehler PRI222:0xdef0000f-nicht angegebener Fehler aufgetreten)* " angezeigt.

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

## <a name="new-command"></a>Neuer Befehl

Der `new` Befehl erstellt eine neue PRI-Datei, indem die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert werden. Führen `MakePri.exe new /?` Sie aus, um ausführliche Hilfe zu diesem Befehl anzuzeigen.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the .appx package name, class library
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

## <a name="resourcepack-command"></a>Resourcepack-Befehl

Der `resourcepack` Befehl erstellt eine neue PRI-Datei, indem die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert werden. Eine PRI-Datei des Ressourcen Pakets enthält nur zusätzliche Varianten von Ressourcen, die bereits in einer vorhandenen PRI-Datei angegeben sind. Führen `MakePri.exe resourcepack /?` Sie aus, um ausführliche Hilfe zu diesem Befehl anzuzeigen.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
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

## <a name="versioned-command"></a>Versionierter Befehl

Der `versioned` Befehl erstellt eine PRI-Datei mit Versions Angabe, indem die Dateien in Ihrem Projekt gemäß Ihrer Konfigurationsdatei indiziert werden. Führen `MakePri.exe versioned /?` Sie aus, um ausführliche Hilfe zu diesem Befehl anzuzeigen.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
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

## <a name="47extensiondllex"></a>&#47;ExtensionDLL (ex)

Verwenden Sie die Erweiterungs-DLL-Option (/ex) mit `createconfig` , `dump` , `new` , `resourcepack` und, `versioned` um den Speicherort der dll der Resource Management-System Umgebungs Erweiterung anzugeben.

## <a name="logging47metadata-file"></a>Protokollieren&#47;Metadatendatei

Makepri kann Informationen enthalten, die für ein Ressourcenpaket in der indexermetadatendatei spezifisch sind. Im folgenden finden Sie ein Beispiel für eine Protokolldatei für `resources.pri` mit den Ressourcen `german.pri` -pri-Dateien und `highresolution.pri` .

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

## <a name="47indexfileif-option"></a>&#47;Indexfile (IF)-Option

Verwenden Sie die Option Indexdatei (/IF) mit `dump` , `resourcepack` und, `versioned` um eine PRI-Eingabedatei anzugeben.

Für `resourcepack` und `versioned` können Sie stattdessen eine Schema Datei bereitstellen, anstatt eine PRI-Datei als Eingabeparameter für/IndexFile (IF) bereitzustellen.

```console
/IndexFile(if) <FILEPATH>
```

**FilePath** ist ein Token, das den Speicherort der PRI-Eingabedatei oder PRI-Schema Datei angibt.

## <a name="47indexoptionsio-option"></a>&#47;IndexOptions (IO)-Option

Sie verwenden die Option Index Optionen (/IO) mit `new` , `resourcepack` und, `versioned` um Optionen anzugeben, die eine ausführliche Kontrolle über das Verhalten von ressourcenindexer bereitstellen. Index Optionen sind standardmäßig deaktiviert.

```console
/IndexOptions(io) <OPTIONS>
```

Die **Optionen** sind eine durch Trennzeichen getrennte Liste, die die folgenden Optionen umfasst.

- +/-HiddenFiles (HF). Index (+) oder ignorieren (-) ausgeblendete Dateien und Ordner.
- +/-LinkedFiles (LF). Index (+) oder IGNORE (-) verknüpfte Dateien und Ordner.

## <a name="47mappingfilemf-option"></a>&#47;MappingFile (MF)-Option

Verwenden Sie die Option Mapping File (/MF) mit `new` , `resourcepack` und, `versioned` um eine Mapping-Datei zu generieren. [MakeAppx.exe](/windows/msix/package/create-app-package-with-makeappx-tool) verwendet die Mapping-Datei, um App-Pakete zu generieren.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**Mappingfiletype** ist ein Token, das das Format der Zuordnungsdatei angibt. Das einzige gültige unterstützte Format ist `appx` .

```console
/mf appx
```

Dies ist ein Beispiel Inhalt einer Hauptkonfigurationsdatei.

```console
"ResourceDimensions"                   "language-de-de"
```

Dies ist ein Beispiel Inhalt einer Resource Pack-Mapping-Datei.

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Ausgabe Zusammenfassung

Wenn Ressourcen Pakete erstellt werden, ist die Ausgabe Zusammenfassung aus MakePRI.exe ausführlichere Form. Hier sehen Sie ein Beispiel.

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

## <a name="47overwriteo-option"></a>&#47;Überschreibungs Option (o)

Wenn die über Schreiboption (/o) nicht bereitgestellt wird und die angegebene Ausgabedatei (en) bereits vorhanden ist, wird MakePri.exe vor dem Überschreiben eine Bestätigung benötigt.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;OutputFile (of)-Option

Verwenden Sie die Option Output File (/of) mit `dump` , `new` , `resourcepack` und, `versioned` um den Ausgabe Speicherort und den Namen der zu generierenden PRI-Datei anzugeben. Wenn MakePri.exe mehrere Ressourcen-PRI-Dateien generiert, werden diese in den übergeordneten Ordner der Zieldatei platziert. Wenn Sie z. b. angeben, `/of MyParentFolder\TargetFile.pri` MakePri.exe generiert `TargetFile.language-en.pri` und `TargetFile.scale-100.pri` parallel `TargetFile.pri` unter `ParentFolder` .

Im folgenden finden Sie ein Beispiel für eine Fehlerbedingung und die zugehörige Fehlermeldung.

| Fehlerzustand | Fehlermeldung |
| --------------- | ------------- |
| Der Name der Ausgabedatei ist mit dem Namen eines Ressourcen Pakets in der Konfiguration identisch. | Ungültige Konfiguration: der Name <resource pack name> des Ressourcen Pakets darf nicht mit der Ausgabedatei <OutputFileName. pri> identisch sein. |

## <a name="reversemaprm-option"></a>/ReverseMap (RM)-Option

Verwenden Sie die Option umgekehrte Zuordnung (/RM) mit `new` , `resourcepack` und, `versioned` um einen Abschnitt mit umgekehrter Zuordnung in der PRI-Datei zu generieren, der zum Debuggen verwendet werden kann.

## <a name="47schemafilesf-option"></a>&#47;SchemaFile (SF)-Option

Sie verwenden die Schema Datei Option (/SF) mit `new` , `resourcepack` und, `versioned` um eine Schema Datei an der angegebenen Position zu schreiben.

Für `resourcepack` und `versioned` können Sie stattdessen eine Schema Datei bereitstellen, anstatt eine PRI-Datei als Eingabeparameter für/IndexFile (IF) bereitzustellen.

```console
/SchemaFile(sf) <FILEPATH>
```

**FilePath** ist ein Token, das angibt, wo die Schema Datei geschrieben werden soll.

Dies ist ein Beispiel für eine Schema Datei.

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

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor (VMA) ist veraltet.

Die Option für die Hauptversion (/VMA) (für den- `new` Befehl) ist veraltet, und die Verwendung dieser Warnung führt zu dieser Warnmeldung.

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Um die Hauptversionsnummer anzugeben, verwenden Sie das- [resources@majorVersion](makepri-exe-configuration.md) Attribut in der Konfigurationsdatei.

## <a name="related-topics"></a>Verwandte Themen

* [MakePri.exe](compile-resources-manually-with-makepri.md)
