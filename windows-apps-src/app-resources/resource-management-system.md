---
Description: Zum Buildzeitpunkt erstellt das Ressourcenverwaltungssystem einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer App gepackt sind. Zur Laufzeit erkennt das System die momentan geltenden Benutzer- und Computereinstellungen und lädt die Ressourcen, die für diese Einstellungen am besten geeignet sind.
title: Ressourcenverwaltungssystem
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: bedbad9e4de22ee098863d013a1e4ad16d86543e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598625"
---
# <a name="resource-management-system"></a>Ressourcenverwaltungssystem
Das Ressourcenverwaltungssystem verfügt sowohl über Buildzeit- als auch über Laufzeitfeatures. Während der Buildzeit erstellt das System einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer App gepackt sind. Dieser ebenfalls im App-Paket enthaltene Index ist der Package Resource Index oder kurz PRI. Zur Laufzeit erkennt das System die momentan geltenden Benutzer- und Computereinstellungen, fragt die Informationen im PRI ab und lädt automatisch die Ressourcen, die für diese Einstellungen am besten geeignet sind.

## <a name="package-resource-index-pri-file"></a>Package Resource Index (PRI)-Datei
Jedes App-Paket muss einen binären Index der App-Ressourcen enthalten. Dieser Index wird zur Buildzeit erstellt und ist in einer oder mehreren Package Resource Index (PRI)-Dateien enthalten.

- Eine PRI-Datei enthält Zeichenfolgenressourcen und einen indizierten Satz von Dateipfaden, die auf die verschiedenen Dateien im Paket verweisen.
- Ein Paket enthält normalerweise eine einzelne PRI-Datei pro Sprache namens „resources.pri”.
- Die Datei „resources.pri” im Stammverzeichnis eines Pakets wird beim Instanziieren von [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) automatisch geladen.
- PRI-Dateien können mit dem Tool [MakePRI.exe](compile-resources-manually-with-makepri.md) erstellt und gesichert werden.
- Für typische App-Entwicklungen benötigen Sie MakePRI.exe nicht, da es bereits in den Visual Studio-Kompilierungsworkflow integriert ist. Und Visual Studio unterstützt das Bearbeiten von PRI-Dateien in einer dedizierten Benutzeroberfläche. Ihre Lokalisierer und die von ihnen verwendeten Tools sind jedoch möglicherweise auf MakePRI.exe angewiesen.
- Jede PRI-Datei enthält eine benannte Sammlung von Ressourcen, die als Ressourcenzuordnung bezeichnet wird. Wenn eine PRI-Datei aus einem Paket geladen wird, wird der Name der Ressourcenzuordnung darauf überprüft, ob sich eine Übereinstimmung mit dem Namen der Paketidentität ergibt.
- PRI-Dateien sind reine Datendateien, die das Portable Executable (PE)-Format nicht verwenden. Sie wurden als Ressourcenformat für Windows speziell als reine Datendateien konzipiert. Sie ersetzen Ressourcen, die im Win32-App-Modell in DLLs enthalten waren.
- Die maximale Größe für eine PRI-Datei beträgt 64 KB.

## <a name="uwp-api-access-to-app-resources"></a>UWP-API-Zugriff auf App-Ressourcen

### <a name="basic-functionality-resourceloader"></a>Grundlegende Funktionalität (ResourceLoader)
Die einfachste Möglichkeit, programmgesteuert auf Ihre App-Ressourcen zuzugreifen, ist die Verwendung des Namespace [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) und der Klasse [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live). **ResourceLoader** ermöglicht einen allgemeinen Zugriff auf Zeichenfolgenressourcen aus einer Reihe von Ressourcendateien, referenzierten Bibliotheken und anderen Paketen.

### <a name="advanced-functionality-resourcemanager"></a>Erweiterte Funktionalität (ResourceManager)
Die Klasse [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (aus dem Namespace [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) ) stellt zusätzliche Inspektionsmöglichkeiten und Informationen zu Ressourcen bereit, z. B. Enumerationen. Solche Daten kann die Klasse **ResourceLoader** nicht liefern.

Ein [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live)-Objekt ist eine einzelne logische Ressource mit mehreren Sprachvarianten oder sonstigen qualifizierten Varianten. Es beschreibt die logische Ansicht des Elements oder der Ressource mit einem Bezeichner für Zeichenfolgenressourcen wie `Header1` oder einem Ressourcendateinamen wie `logo.jpg`.

Ein [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live)-Objekt stellt einen einzelnen konkreten Ressourcenwert und seine Qualifizierer dar, z. B. die Zeichenfolge „Hello World” für Englisch oder „logo.scale-100.jpg” als eine qualifizierte Bildressource mit der spezifischen Auflösung **scale-100**.

Die für eine App verfügbaren Ressourcen werden in hierarchischen Sammlungen gespeichert, auf die Sie mit einem [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live)-Objekt zugreifen können. Die Klasse **ResourceManager** ermöglicht den Zugriff auf die verschiedenen von der App verwendeten **ResourceMap**-Instanzen oberster Ebene, die den unterschiedlichen Paketen für die App entsprechen. Der [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap)-Wert entspricht der Ressourcenzuordnung für das aktuelle App-Paket. Alle referenzierten Frameworkpakete sind ausgeschlossen. Jede **ResourceMap** wird nach dem Paketnamen benannt, der im Paketmanifest angegeben ist. Eine **ResourceMap** umfasst Unterstrukturen (siehe [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)), die weitere **NamedResource**-Objekte enthalten. Die Unterstrukturen entsprechen üblicherweise den Ressourcendateien, in denen die Ressource enthalten ist. Weitere Informationen finden Sie unter [Lokalisieren von Zeichenfolgen in Benutzeroberfläche und App-Paketmanifest](localize-strings-ui-manifest.md) und [Laden von Bilder und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast und anders](images-tailored-for-scale-theme-contrast.md).

Hier sehen Sie ein Beispiel.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Hinweis:** Der Ressourcenbezeichner wird als URI (Uniform Resource Identifier)-Fragment behandelt und unterliegt der URI-Semantik. Beispielsweise wird `GetValue("Caption%20")` wie `GetValue("Caption ")` behandelt. Verwenden Sie „?” oder „#” nicht in Ressourcenbezeichnern, da diese Zeichen die Auswertung des Ressourcenpfads beenden. Beispielsweise wird „MyResource?3” wie „MyResource” behandelt.

Der **ResourceManager** unterstützt nicht nur den Zugriff auf die Zeichenfolgenressourcen einer App, sondern erlaubt außerdem die Enumeration und Inspektion der verschiedenen Dateiressourcen. Um Konflikte zwischen Dateien und anderen Ressourcen zu vermeiden, die einer Datei entstammen, befinden sich indizierte Dateipfade in einer reservierten **ResourceMap** -Unterstruktur namens „Files”. Die Datei `\Images\logo.png` entspricht beispielsweise dem Ressourcennamen `Files/images/logo.png`.

Die [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live)-APIs behandeln Verweise auf Dateien transparent als Ressourcen und eignen sich für die üblichen Verwendungsszenarien. Der **ResourceManager** sollte nur für erweiterte Szenarien verwendet werden, z. B. wenn der aktuellen Kontext überschrieben werden soll.

### <a name="resourcecontext"></a>ResourceContext
Ressourcenkandidaten werden anhand eines bestimmten [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)-Elements ausgewählt. Dabei handelt es sich um eine Sammlung von Ressourcenqualifiziererwerten (für Sprache, Skalierung, Kontrast usw.). Sofern keine Überschreibung erfolgt, wird von einem Standardkontext die aktuelle App-Konfiguration für die einzelnen Qualifiziererwerte verwendet. Zum Beispiel können Ressourcen in Bezug auf die Skalierung qualifiziert werden, die von einem Monitor zum anderen und somit auch von einer Anwendungsansicht zur anderen variieren kann. Aus diesem Grund verfügt jede Anwendungsansicht über einen eigenen Standardkontext. Der Standardkontext für eine bestimmte Ansicht kann mithilfe von [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView) abgerufen werden. Beim Abrufen eines Ressourcenkandidaten sollte eine **ResourceContext**-Instanz übergeben werden, um für die jeweilige Ansicht den am besten passenden Wert zu erhalten.

## <a name="important-apis"></a>Wichtige APIs
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [Ressourcen-Manager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.](images-tailored-for-scale-theme-contrast.md)
