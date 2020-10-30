---
description: In diesem Szenario erstellen wir eine neue APP, die das benutzerdefinierte Buildsystem repräsentiert. Wir erstellen einen ressourcenindexer und fügen ihm Zeichen folgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.
title: Szenario 1 Generieren einer PRI-Datei aus Zeichen folgen Ressourcen und assetdateien
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 44f4b8297cc1a34a378af137f75babca64e4edf2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031673"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Szenario 1: Generieren einer PRI-Datei aus Zeichen folgen Ressourcen und assetdateien
In diesem Szenario verwenden wir die API für die [Paket Ressourcen Indizierung (PRI)](/windows/desktop/menurc/pri-indexing-reference) , um eine neue APP zu erstellen, die das benutzerdefinierte Buildsystem repräsentiert. Der Zweck dieses benutzerdefinierten Buildsystems besteht darin, pri-Dateien für eine UWP-Ziel-APP zu erstellen. Im Rahmen dieser exemplarischen Vorgehensweise erstellen wir einige Beispiel Ressourcen Dateien (mit Zeichen folgen und anderen Arten von Ressourcen), um die Ressourcen der Ziel-UWP-App darzustellen.

## <a name="new-project"></a>Neues Projekt
Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie ein Projekt für eine **Visual C++ Windows-Konsolenanwendung** , und nennen Sie es *cbsconsoleapp* (für "benutzerdefinierte buildsystemkonsolen-app").

Wählen Sie *x64* aus der Dropdown-Dropdown Mappe Projektmappenplattformen **Solution Platforms**

## <a name="headers-static-library-and-dll"></a>Header, statische Bibliothek und dll
Die PRI-APIs werden in der "mrmresourceindexer. h"-Header Datei (die in installiert ist `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\` ) deklariert. Öffnen Sie die Datei `CBSConsoleApp.cpp` , und schließen Sie den Header zusammen mit anderen Headern ein, die Sie benötigen.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Die APIs werden in MrmSupport.dll implementiert, auf die Sie zugreifen, indem Sie eine Verknüpfung mit der statischen Bibliothek "mrmsupport. lib" herstellen. Öffnen Sie die **Eigenschaften** des Projekts, klicken Sie auf **Linker**  >  **Eingabe** , bearbeiten Sie **additionalabhängigkeiten** , und fügen Sie hinzu `MrmSupport.lib` .

Erstellen Sie die Projekt Mappe, und kopieren Sie Sie `MrmSupport.dll` aus in den Buildausgabeordner `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` (wahrscheinlich `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\` ).

Fügen Sie die folgende Hilfsfunktion hinzu `CBSConsoleApp.cpp` , da wir Sie benötigen.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

Fügen Sie in der- `main()` Funktion Aufrufe zum Initialisieren und zum Deaktivieren der com-Initialisierung hinzu.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Ressourcen Dateien, die zur Ziel-UWP-App gehören
Nun benötigen wir einige Beispiel Ressourcen Dateien (mit Zeichen folgen und anderen Arten von Ressourcen), um die Ressourcen der Ziel-UWP-App darzustellen. Diese können sich natürlich an einer beliebigen Stelle im Dateisystem befinden. In dieser exemplarischen Vorgehensweise ist es jedoch praktisch, Sie in den Projektordner von cbsconsoleapp einzufügen, damit alles an einer Stelle ist. Sie müssen diese Ressourcen Dateien nur dem Dateisystem hinzufügen. Fügen Sie Sie nicht dem cbsconsoleapp-Projekt hinzu.

Fügen Sie innerhalb desselben Ordners `CBSConsoleApp.vcxproj` , der enthält, einen neuen Unterordner mit dem Namen hinzu `UWPAppProjectRootFolder` . Erstellen Sie diese Beispiel Ressourcen Dateien in diesem neuen Unterordner.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Diese Datei kann ein beliebiges PNG-Bild enthalten.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\Uwpappprojectrootfolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\Uwpappprojectrootfolder\be-de \Resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\Uwpappprojectrootfolder\en-\Resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>Indizieren Sie die Ressourcen, und erstellen Sie eine PRI-Datei.
`main()`Deklarieren Sie in der-Funktion vor dem-Befehl zum Initialisieren von com einige Zeichen folgen, die wir benötigen, und erstellen Sie außerdem den Ausgabeordner, in dem wir die PRI-Datei erstellen.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Deklarieren Sie unmittelbar nach dem Aufruf von com einen ressourcenindexer-handle, und rufen Sie dann [**mrmkreateresourceindexer**](/windows/desktop/menurc/mrmcreateresourceindexer) auf, um einen ressourcenindexer zu erstellen.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Im folgenden finden Sie eine Erläuterung der Argumente, die an **mrmkreateresourceingedexer** übergeben werden.

- Der Paket Familienname der Ziel-UWP-APP, die als Ressourcen Zuordnungs Name verwendet wird, wenn wir später eine PRI-Datei von diesem ressourcenindexer generieren.
- Der Projektstamm der UWP-Ziel-app. Anders ausgedrückt: der Pfad zu den Ressourcen Dateien. Wir geben diese Angabe an, damit wir in nachfolgenden API-aufrufen desselben ressourcenindexers Pfade relativ zu diesem Stamm angeben können.
- Die Version von Windows, auf die wir abzielen möchten.
- Eine Liste der Standard Ressourcen Qualifizierer.
- Ein Zeiger auf das ressourcenindexer-handle, damit die Funktion diese festlegen kann.

Der nächste Schritt besteht darin, die Ressourcen dem soeben erstellten ressourcenindexer hinzuzufügen. `resources.resw` ist eine Ressourcen Datei (. resw), die die neutralen Zeichen folgen für die Ziel-UWP-app enthält. Scrollen Sie nach oben (in diesem Thema), wenn der Inhalt angezeigt werden soll. `de-DE\resources.resw` enthält unsere deutschen Zeichen folgen und `en-US\resources.resw` unsere englischen Zeichen folgen. Zum Hinzufügen der Zeichen folgen Ressourcen in einer Ressourcen Datei zu einem ressourcenindexer wird [**mrmindexresourcecontainerautoqualifizierer**](/windows/desktop/menurc/mrmindexresourcecontainerautoqualifiers)aufgerufen. Drittens wird die [**mrmindexfile**](/windows/desktop/menurc/mrmindexfile) -Funktion für eine Datei aufgerufen, die eine neutrale Bildressource für den ressourcenindexer enthält.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

Im-Befehl von **mrmindexfile** ist der Wert L "MS-Resource:///Files/sample-image.png" der Ressourcen-URI. Das erste Pfad Segment ist "Files", und das wird als Name der Unterstruktur der Ressourcen Zuordnung verwendet, wenn wir später eine PRI-Datei von diesem ressourcenindexer generieren.

Wenn Sie den ressourcenindexer über unsere Ressourcen Dateien informiert haben, ist es an der Zeit, eine PRI-Datei auf dem Datenträger zu generieren, indem Sie die [**mrmcreateresourcefile**](/windows/desktop/menurc/mrmcreateresourcefile) -Funktion aufrufen.

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

An diesem Punkt wurde eine PRI-Datei mit dem Namen `resources.pri` in einem Ordner mit dem Namen erstellt `Generated PRIs` . Nun, da wir mit dem ressourcenindexer fertig sind, wird [**mrmdestroyindexerandmessages**](/windows/desktop/menurc/mrmdestroyindexerandmessages) aufgerufen, um das Handle zu zerstören und alle zugeordneten Computerressourcen freizugeben.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Da eine PRI-Datei binär ist, wird es einfacher, das soeben generierte zu sehen, wenn die binäre PRI-Datei in der XML-Entsprechung gespeichert wird. Der Befehl " [**mrmdumpprifile**](/windows/desktop/menurc/mrmdumpprifile) " führt genau das aus.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Im folgenden finden Sie eine Erläuterung der Argumente, die an **mrmdumpprifile** übermittelt werden.

- Der Pfad zur PRI-Datei, die gesichert werden soll. Wir verwenden den ressourcenindexer in diesem Aufruf nicht (wir haben ihn soeben zerstört), daher müssen wir einen vollständigen Dateipfad angeben.
- Keine Schema Datei. In diesem Thema wird erläutert, worum es sich bei einem Schema handelt.
- Nur die grundlegenden Informationen.
- Der Pfad einer XML-Datei, die erstellt werden soll.

Hier enthält die PRI-Datei, die hier in XML gekippt ist,.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

Die Informationen beginnen mit einer Ressourcen Zuordnung, die mit dem Paket Familiennamen der Ziel-UWP-App benannt wird. In der Ressourcen Zuordnung sind zwei Ressourcen Zuordnungs Substrukturen enthalten: eine für die von uns indizierten Datei Ressourcen und eine für die Zeichen folgen Ressourcen. Beachten Sie, dass der Paket Familienname in alle Ressourcen-URIs eingefügt wurde.

Die erste Zeichen folgen Ressource ist *enonlystring* aus `en-US\resources.resw` , und Sie weist nur einen Kandidaten auf (der mit dem Qualifizierer *language-en-US* übereinstimmt). Der nächste Schritt ist *LocalizedString1* von `resources.resw` und `en-US\resources.resw` . Folglich gibt es zwei Kandidaten: eine passende *Sprache (en-US* ) und einen Fall Back neutralen Kandidaten, der mit jedem Kontext übereinstimmt. Ebenso hat *LocalizedString2* zwei Kandidaten: " *language-de-de* " und "neutral". Und schließlich ist *neutralonlystring* nur in neutraler Form vorhanden. Ich habe diesen Namen gegeben, um klar zu machen, dass er nicht lokalisiert werden soll.

## <a name="summary"></a>Zusammenfassung
In diesem Szenario haben wir gezeigt, wie Sie die API für die [Paket Ressourcen Indizierung (PRI)](/windows/desktop/menurc/pri-indexing-reference) zum Erstellen eines ressourcenindexers verwenden. Wir haben dem ressourcenindexer Zeichen folgen Ressourcen und assetdateien hinzugefügt. Anschließend haben wir den ressourcenindexer verwendet, um eine binäre PRI-Datei zu generieren. Und schließlich haben wir die binäre PRI-Datei in Form von XML gekippt, damit wir bestätigen konnten, dass Sie die erwarteten Informationen enthält.

## <a name="important-apis"></a>Wichtige APIs
* [Referenz zur Paket Ressourcen Indizierung (PRI)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Verwandte Themen
* [APIs für die Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme](pri-apis-custom-build-systems.md)
