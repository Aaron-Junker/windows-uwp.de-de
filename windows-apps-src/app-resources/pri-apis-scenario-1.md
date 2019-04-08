---
Description: In diesem Szenario erstellen wir eine neue App zur Darstellung unseres benutzerdefinierten Buildsystems. Wir erstellen einen Ressourceindexer und fügen diesem Zeichenfolgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.
title: 'Szenario 1: Generieren einer PRI-Datei aus Zeichenfolgenressourcen und Ressourcendateien'
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 0ccb9447e9594f71907f0da5d0e15f9c6c65bb6b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622755"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Szenario 1: Generieren einer PRI-Datei von Zeichenfolgenressourcen und Ressourcendateien
In diesem Szenario verwenden wir die [APIs zur Paketressourcenindizierung (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690), um eine neue App zur Darstellung unseres benutzerdefinierten Buildsystems zu erstellen. Denken Sie daran: Der Zweck dieses benutzerdefinierten Buildsystems besteht darin, PRI-Dateien für eine Ziel-UWP-App zu erstellen. Im Rahmen dieser exemplarischen Vorgehensweise erstellen wir also einige Beispielressourcendateien (mit Zeichenfolgen und anderen Arten von Ressourcen), um die Ressourcen dieser Ziel-UWP-App abzubilden.

## <a name="new-project"></a>Neues Projekt
Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie ein Anwendungsprojekt der **Visual C++-Windows-Konsole**, und nennen Sie es *CBSConsoleApp*.

Wählen Sie *x64* aus der Dropdownliste **Lösungsplattformen**.

## <a name="headers-static-library-and-dll"></a>Header, statische Bibliothek und DLL
Die PRI-APIs werden in der Headerdatei „MrmResourceIndexer.h“ deklariert (diese wird unter `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\` installiert). Öffnen Sie die Datei `CBSConsoleApp.cpp`, und fügen Sie den Header sowie einige andere Header hinzu, die Sie benötigen.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Die APIs sind in „MrmSupport.dll“ implementiert. Diese rufen Sie über die Verknüpfung zur statischen Bibliothek „MrmSupport.lib“ auf. Öffnen Sie die **Eigenschaften** des Projekts, klicken Sie auf **Linker** > **Eingabe**, bearbeiten Sie **Zusätzliche Abhängigkeiten**, und fügen Sie `MrmSupport.lib` hinzu.

Erstellen Sie die Projektmappe, und kopieren Sie `MrmSupport.dll` von `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` in den Build-Ausgabeordner (wahrscheinlich `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`).

Fügen Sie die folgende Hilfsfunktion `CBSConsoleApp.cpp` hinzu, da wir diese benötigen.

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

Fügen Sie in der `main()`-Funktion Aufrufe zur Initialisierung und Aufhebung der Initialisierung von COM hinzu.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Ressourcendateien, die zur Ziel-UWP-App gehören
Wir benötigen nun einige Beispielressourcendateien (mit Zeichenfolgen und anderen Arten von Ressourcen), um die Ressourcen dieser Ziel-UWP-App abzubilden. Diese können sich natürlich an einem beliebigen Ort im Dateisystem befinden. Für diese exemplarische Vorgehensweise empfiehlt es sich jedoch, sie im Projektordner von CBSConsoleApp zu platzieren, damit alles an einem zentralen Ort abgelegt ist. Sie müssen diese Ressourcendateien nur dem Dateisystem hinzuzufügen. Fügen Sie sie nicht dem Projekt „CBSConsoleApp“ hinzu.

Fügen Sie im gleichen Ordner, der `CBSConsoleApp.vcxproj` enthält, einen neuen Unterordner namens `UWPAppProjectRootFolder` hinzu. Erstellen Sie in diesem neuen Unterordner diese Beispielressourcendateien.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Diese Datei kann ein beliebiges PNG-Bild enthalten.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
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

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
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

## <a name="index-the-resources-and-create-a-pri-file"></a>Indizieren der Ressourcen und Erstellen einer PRI-Datei
Deklarieren Sie in der `main()`-Funktion vor dem Aufruf zur COM-Initialisierung einige Zeichenfolgen, die wir benötigen, und erstellen Sie auch den Ausgabeordner, in dem wir die PRI-Datei generieren.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Deklarieren Sie unmittelbar nach dem Aufruf zur COM-Initialisierung einen Ressourcenindexer-Handle, und rufen Sie dann [**MrmCreateResourceIndexer**](/windows/desktop/menurc/mrmcreateresourceindexer) auf, um einen Ressourcenindexer zu erstellen.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Hier werden die Argumente erläutert, die an **MrmCreateResourceIndexer** übergeben werden.

- Der Paketfamilienname der Ziel-UWP-App, der als Name der Ressourcenzuordnung verwendet wird, wenn wir später eine PRI-Datei aus diesem Ressourcenindexer generieren.
- Der Projektstamm der Ziel-UWP-App. Mit anderen Worten: der Pfad zu unserer Ressourcendateien. Wir geben diesen an, damit wir dann Pfade relativ zu diesem Stamm in nachfolgenden API-Aufrufe an den gleichen Ressourcenindexer angeben können.
- Die Version von Windows, die wir anvisieren.
- Eine Liste der Standardressourcenqualifizierer.
- Ein Zeiger auf unseren Ressourcenindexer-Handle, sodass die Funktion diesen festlegen kann.

Im nächsten Schritt fügen wir unsere Ressourcen dem Ressourcenindexer hinzu, den wir gerade erstellt haben. `resources.resw` ist eine Ressourcendatei (.resw), die die neutralen Zeichenfolgen für die Ziel-UWP-app enthält. Führen Sie (in diesem Thema) einen Bildlauf nach oben durch, wenn Sie den Inhalt dieser Datei anzeigen möchten. `de-DE\resources.resw` enthält unsere deutschen Zeichenfolgen, und `en-US\resources.resw` angezeigten englischen Zeichenfolgen. Zum Hinzufügen der Zeichenfolgenressourcen innerhalb einer Ressourcendatei zu einem Ressourcenindexer rufen Sie [**MrmIndexResourceContainerAutoQualifiers**](/windows/desktop/menurc/mrmindexresourcecontainerautoqualifiers) auf. Als Drittes rufen wir die [**MrmIndexFile**](/windows/desktop/menurc/mrmindexfile)-Funktion zu einer Datei auf, die eine neutrale Bildressource für den Ressourcenindexer enthält.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

Im Aufruf an **MrmIndexFile** ist der Wert „ms-resource:///Files/sample-image.png“ der Ressourcen-URI. Das erste Pfadsegment ist „Dateien“. Dieses wird als Name der Ressourcenzuordnung-Unterstruktur verwendet, wenn wir später eine PRI-Datei aus diesem Ressourcenindexer generieren.

Nachdem wir den Ressourcenindexer über unsere Ressourcendateien informiert haben, ist es an der Zeit, dass dieser uns eine PRI-Datei auf dem Datenträger generiert, und zwar durch Aufrufen der [**MrmCreateResourceFile**](/windows/desktop/menurc/mrmcreateresourcefile)-Funktion.

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

Zu diesem Zeitpunkt wurde eine PRI-Datei mit dem Namen `resources.pri` in einem Ordner namens `Generated PRIs` erstellt. Nun, da wir mit dem Ressourcenindexer fertig sind, rufen wir [**MrmDestroyIndexerAndMessages**](/windows/desktop/menurc/mrmdestroyindexerandmessages) auf, um den Handle zu löschen und alle ihm zugewiesenen Computerressourcen freizugeben.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Da eine PRI-Datei ein binäres Format hat, können wir das soeben Generierte leichter anzeigen, wenn wir die binäre PRI-Datei in ihr XML-Äquivalent sichern. Ein Aufruf von [ **MrmDumpPriFile** ](/windows/desktop/menurc/mrmdumpprifile) macht genau das.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Hier werden die Argumente erläutert, die an **MrmDumpPriFile** übergeben werden.

- Der Pfad zur PRI-Datei, die gesichert werden soll. In diesem Aufruf verwenden wir nicht den Ressourcenindexer (wir haben diesen gerade gelöscht). Folglich müssen wir einen vollständigen Dateipfad angeben.
- Keine Schemadatei. Was ein Schema ist, wird weiter unten im Thema erläutert.
- Nur die grundlegenden Informationen.
- Der Pfad einer zu erstellenden XML-Datei.

Dies enthält die PRI-Datei, hier in XML gesichert.

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

Die Informationen beginnen mit einer Ressourcenzuordnung, deren Namen dem Paketfamiliennamen unserer Ziel-UWP-App entspricht. Von der Ressourcenzuordnung eingeschlossen sind zwei Ressourcenzuordnungsteilstrukturen: eine für die Dateiressourcen, die wir indiziert haben, und eine andere für unsere Zeichenfolgenressourcen. Beachten Sie, wie der Paketfamilienname in alle Ressourcen-URIs eingefügt wurde.

Die erste Zeichenfolgenressource lautet *EnOnlyString* aus `en-US\resources.resw`. Dieser umfasst nur einen Kandidaten (der dem Qualifizierer *language-en-US* entspricht). Anschließend folgt *LocalizedString1* sowohl aus `resources.resw` als auch aus `en-US\resources.resw`. Folglich gibt es zwei Kandidaten: einen übereinstimmenden *language-en-US* und einen neutralen Fallback-Kandidaten, der mit einem beliebigen Kontext übereinstimmt. Analog dazu verfügt *LocalizedString2* über zwei Kandidaten: *language-de-DE* und neutral. Und schließlich ist *NeutralOnlyString* nur in neutraler Form vorhanden. Dieser Name soll deutlich machen, dass er nicht lokalisiert werden soll.

## <a name="summary"></a>Zusammenfassung
In diesem Szenario haben wir gezeigt, wie Sie die [APIs zur Paketressourcenindizierung (PRI](https://msdn.microsoft.com/library/windows/desktop/mt845690) verwenden, um einen Ressourcenindexer zu erstellen. Wir haben dem Ressourcenindexer Zeichenfolgenressourcen und Ressourcendateien hinzugefügt. Anschließend haben wir mit dem Ressourcenindexer eine binäre PRI-Datei generiert. Und schließlich haben wir die binäre PRI-Datei im XML-Format gesichert, sodass wir überprüfen konnten, dass sie die erwarteten Informationen enthält.

## <a name="important-apis"></a>Wichtige APIs
* [Ressource "Package" Indizierung (PRI)-Referenz](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Verwandte Themen
* [APIs für die Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme](pri-apis-custom-build-systems.md)
