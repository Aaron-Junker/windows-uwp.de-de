---
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: Erstellen und Konvertieren einer Quellinhalt-Gruppenzuordnung
description: Um Ihre Universelle Windows-Plattform (UWP)-App für die UWP-App-Streaming-Installation vorzubereiten, müssen Sie eine Inhalts-Gruppenzuordnung erstellen. Dieser Artikel hilft Ihnen mit den Einzelheiten für das Erstellen und Konvertieren einer Inhalts-Gruppenzuordnung und bietet gleichzeitig einige Tipps und Tricks.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, Uwp, Inhalt-Gruppenzuordnung, Streaming-Installation, Uwp-App-Streaming-Installation, Quellinhalt-Gruppenzuordnung
ms.localizationpriority: medium
ms.openlocfilehash: 7e25f30a2288f6be76ce108de2056ab6407187de
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67821031"
---
# <a name="create-and-convert-a-source-content-group-map"></a>Erstellen und Konvertieren einer Quellinhalt-Gruppenzuordnung

Um Ihre Universelle Windows-Plattform (UWP)-App für die UWP-App-Streaming-Installation vorzubereiten, müssen Sie eine Inhalts-Gruppenzuordnung erstellen. Dieser Artikel hilft Ihnen mit den Einzelheiten für das Erstellen und Konvertieren einer Inhalts-Gruppenzuordnung und bietet gleichzeitig einige Tipps und Tricks.

## <a name="creating-the-source-content-group-map"></a>Erstellen der Quellinhalt-Gruppenzuordnung

Sie müssen eine `SourceAppxContentGroupMap.xml`-Datei erstellen, und entweder Visual Studio oder das **MakeAppx.exe**-Tool verwenden, um diese Datei auf die endgültige Version zu konvertieren: `AppxContentGroupMap.xml`. Ist es möglich, einen Schritt zu überspringen, indem Sie den `AppxContentGroupMap.xml` von Grund auf neu erstellen. Es wird jedoch empfohlen (und es ist in der Regel einfacher), den `SourceAppxContentGroupMap.xml` zu erstellen und ihn zu konvertieren, da Platzhalter in der `AppxContentGroupMap.xml` nicht zulässig sind (und sie sind sehr nützlich). 

Betrachten wir ein einfaches Szenario, in dem eine UWP-App-Streaming-Installation von Vorteil ist. 

Angenommen Sie haben ein UWP-Spiel erstellt, aber die Größe der endgültigen App ist mehr als 100 GB. Dafür eine lange dauern, aus dem Microsoft Store herunterladen, die als unpraktisch erweisen kann. Wenn Sie sich für die UWP-App-Streaming-Installation entscheiden, können Sie die Reihenfolge angeben, in der die Dateien der App heruntergeladen werden. Indem der Benutzer dem Store anordnet, dass zunächst essenzielle Dateien heruntergeladen werden sollen, wird er Ihre App schneller ausprobieren können, während andere nicht unbedingt erforderlichen Dateien im Hintergrund heruntergeladen werden.

> [!NOTE]
> Die Verwendung der UWP-App-Streaming-Installation hängt stark von der Dateiorganisation Ihrer App ab. Es wird empfohlen, dass Sie sich so früh wie möglich Gedanken zum Layout des Inhalts in Bezug auf die UWP-App-Streaming-Installation machen, um das Segmentieren Ihrer App-Dateien zu vereinfachen.

Zunächst erstellen wir eine `SourceAppxContentGroupMap.xml`-Datei.

Bevor wir auf die Details eingehen ist hier ein Beispiel für eine einfache, vollständige `SourceAppxContentGroupMap.xml`-Datei:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

Es gibt zwei Hauptkomponenten in einer Inhalts-Gruppenzuordnung: der **erforderliche** Abschnitt, der die erforderliche Inhaltsgruppe beinhaltet und der **automatische** Abschnitt, der mehrere automatische Inhaltsgruppen enthalten kann.

### <a name="required-content-group"></a>Erforderliche Inhaltsgruppe

Die erforderliche Inhaltsgruppe ist eine einzelne Inhaltsgruppe innerhalb des `<Required>`-Elements von der `SourceAppxContentGroupMap.xml`. Eine erforderliche Inhaltsgruppe sollte alle wichtigen Dateien zum Starten der App mit minimaler Benutzererfahrung enthalten. Aufgrund der .NET Native-Kompilierung muss der gesamte Code (ausführbare Anwendung) Teil der erforderlichen Gruppe sein. Ressourcen und andere Dateien werden den automatischen Gruppen überlassen.

Wenn Ihre App beispielsweise ein Spiel ist, kann die erforderliche Gruppe z. B. Dateien umfassen, die im Hauptmenü oder auf der Startseite des Spiels verwendet werden.

Hier ist der Ausschnitt aus der ursprünglichen `SourceAppxContentGroupMap.xml`-Beispieldatei: 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

Es müssen einige wichtige Dinge erwähnt werden:

- Die `<ContentGroup>` innerhalb des `<Required>`-Elements **muss** „Erforderlich” genannt werden. Dieser Name ist nur für die erforderliche Inhaltsgruppe reserviert und kann nicht mit einer anderen `<ContentGroup>` in der endgültigen Inhaltsgruppen-Zuordnung verwendet werden.
- Es gibt nur ein `<ContentGroup>`. Dies ist beabsichtigt, da nur eine Gruppe von wichtigen Dateien werden soll.
- Die Datei in diesem Beispiel ist eine einzelne `.exe`-Datei. Eine erforderliche Inhaltsgruppe ist nicht auf eine Datei beschränkt, es kann mehrere geben. 

Für die ersten Schritte beim Schreiben dieser Datei wird empfohlen, eine neue Seite in Ihrem bevorzugten Text-Editor zu öffnen, mit Drücken auf „Speichern unter” die Datei in dem App-Projektordner zu speichern, und die neu erstellte Datei folgendermaßen zu nennen: `SourceAppxContentGroupMap.xml`.

> [!IMPORTANT]
> Wenn Sie eine C++-UWP-App entwickeln, müssen Sie die Dateieigenschaften Ihrer `SourceAppxContentGroupMap.xml` anpassen. Setzen Sie die `Content`-Eigenschaft auf **true** und die `File Type`-Eigenschaft auf **XML-Datei**. 

Wenn Sie die `SourceAppxContentGroupMap.xml`erstellen, es ist es bei der Benennung von Dateinamen nützlich, Platzhalter zu verwenden. Weitere Informationen finden Sie unter dem Abschnitt [Tipps und Tricks für die Verwendung von Platzhaltern](#wildcards).

Wenn Sie Ihre App mit Visual Studio entwickelt haben, empfiehlt es sich, dass Sie Folgendes hinsichtlich der erforderlichen Inhaltsgruppe berücksichtigen:

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

Die einzelnen hinzugefügten Platzhalter-Dateinamen werden Datei umfassen, die dem Projektverzeichnis aus Visual Studio hinzugefügt wurden, wie z. B. die ausführbare App oder DLL-Dateien. Die WinMetadata- und Eigenschaften-Ordner müssen die anderen Ordner enthalten, die Visual Studio generiert. Die Platzhalterressourcen müssen die Logo- und SplashScreen-Bilder wählen, die für die Installation der App erforderlich sind.

Beachten Sie, dass Sie das doppelte Platzhalterzeichen nicht, "**", im Stamm der Dateistruktur verwenden können, um alle Dateien im Projekt einzuschließen, da dies bei der Konvertierung der `SourceAppxContentGroupMap.xml` auf das endgültige `AppxContentGroupMap.xml` fehlschlagen wird.

Es sollte auch beachtet werden, dass Fußabdruckdateien (AppxManifest.xml, Appxsignature.p7x, resources.pri usw.) nicht in der Inhalts-Gruppenzuordnung vorhanden sein sollten. Wenn sich Fußabdruckdateien in einer der von Ihnen angegebenen Platzhalter-Dateinamen befinden, werden sie ignoriert.

### <a name="automatic-content-groups"></a>Automatische Inhaltsgruppen

Automatische Inhaltsgruppen sind die Ressourcen, die im Hintergrund heruntergeladen werden, während der Benutzer mit den bereits heruntergeladenen Inhaltsgruppen interagiert. Diese enthalten alle zusätzlichen Dateien, die nicht für das Starten der App erforderlich sind. Sie können beispielsweise automatische Inhaltsgruppen in verschiedene Ebenen aufteilen und jede Ebene als separate Inhaltsgruppen definieren. Wie bereits im Abschnitt zur erforderlichen Inhaltsgruppe erwähnt: aufgrund der .NET Native-Kompilierung, muss der gesamte (für die Anwendung ausführbare) Code Teil der erforderlichen Gruppe sein, und die Ressourcen und anderen Dateien müssen der automatischen Gruppen überlassen werden.

Schauen wir uns die automatische Inhaltsgruppe aus unserem `SourceAppxContentGroupMap.xml`-Beispiel etwas genauer an:
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

Das Layout der automatischen Gruppe ist sehr ähnlich zu der erforderlichen Gruppe, mit wenigen Ausnahmen:

- Es gibt mehrere Inhaltsgruppen.
- Automatische Inhaltsgruppen können einzigartige Namen haben, mit Ausnahme von „Erforderlich”, was für die erforderliche Inhaltsgruppe reserviert ist.
- Automatische Inhaltsgruppen dürfen nicht **alle** Dateien aus der Gruppe der erforderlichen Inhaltsgruppe beinhalten. 
- Eine automatische Inhaltsgruppe kann Dateien enthalten, die sich auch in anderen automatischen Inhaltsgruppen befinden. Die Dateien werden nur ein Mal heruntergeladen, und werden mit der ersten automatischen Inhaltsgruppe heruntergeladen, die sie enthält.

#### Tipps und Tricks für die Verwendung von Platzhaltern<a name="wildcards"></a>

Der Datei-Layout für Inhaltsgruppen-Zuordnungen steht immer im Zusammenhang mit Ihrem Projektstammordner.

In unserem Beispiel werden Platzhalter in beiden `<ContentGroup>`.Elementen verwendet, um alle Dateien innerhalb einer Dateiebene von „Assets\Level2” oder „Assets\Level3” abzurufen. Wenn Sie eine genauere Ordnerstruktur verwenden, können Sie die doppelten Platzhalter einsetzen:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

Sie können auch Platzhalter mit Text für Dateinamen verwenden. Wenn beispielsweise Ihr Ordner „Assets” jede Datei mit dem Namen „Level2” enthalten soll, können Sie etwa Folgendes verwenden:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>Konvertieren von SourceAppxContentGroupMap.xml in AppxContentGroupMap.xml

Konvertiert die `SourceAppxContentGroupMap.xml` auf die endgültige Version `AppxContentGroupMap.xml`, können Sie Visual Studio-2019 oder **MakeAppx.exe** -Befehlszeilentools.

Verwenden von Visual Studio für die Konvertierung Ihrer Inhaltsgruppen-Zuordnung:
1. Fügen Sie die `SourceAppxContentGroupMap.xml` zu Ihrem Projektordner hinzu.
2. Ändern Sie den Buildvorgang der `SourceAppxContentGroupMap.xml`zu „AppxSourceContentGroupMap” im Eigenschaftenfenster.
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt.
3. Navigieren Sie zum Store -> Inhaltsgruppen-Zuordnungsdatei konvertieren

Wenn Sie Ihre App nicht in Visual Studio entwickelt haben, oder wenn Sie einfach nur die Verwendung der Befehlszeile bevorzugen, verwenden Sie das **MakeAppx.exe**-Tool zum Konvertieren Ihrer `SourceAppxContentGroupMap.xml`. 

Ein einfacher **MakeAppx.exe**-Befehl sieht etwa wie folgt aus:
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

Die Option /s gibt den Pfad zur `SourceAppxContentGroupMap.xml` an, und /f gibt den Pfad zur `AppxContentGroupMap.xml` an. Die letzte Option, /d gibt das Verzeichnis für das Erweitern von Dateiplatzhaltern an. In diesem Fall ist es das App-Projekt-Verzeichnis.

Für weitere Informationen zu den Optionen, die Sie mit **MakeAppx.exe** verwenden können, öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu **MakeAppx.exe**, und geben Sie Folgendes ein:

```syntax
MakeAppx convertCGM /?
```

Das ist alles, was Sie brauchen, um Ihre endgültige `AppxContentGroupMap.xml` für Ihre App vorzubereiten! Es gibt noch mehr durchführen müssen, bevor die app für den Microsoft Store vollständig bereit ist. Weitere Informationen zum gesamten Prozess für das Hinzufügen der UWP-App-Streaming-Installationsfunktion für Ihre App finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).
