---
Description: Zum Buildzeitpunkt erstellt das Ressourcenverwaltungssystem einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer App gepackt sind. Zur Laufzeit erkennt das System die momentan geltenden Benutzer- und Computereinstellungen und lädt die Ressourcen, die für diese Einstellungen am besten geeignet sind.
title: Ressourcenverwaltungssystem
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: afe538292fe804dcf042c969005978c3161ec6b6
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868880"
---
# <a name="resource-management-system"></a>Ressourcenverwaltungssystem
Das Ressourcen Verwaltungs System verfügt sowohl über Buildzeit-als auch Lauf Zeitfunktionen. Zum Zeitpunkt der Erstellung erstellt das System einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer APP verpackt werden. Bei diesem Index handelt es sich um den Paket Ressourcen Index oder den PRI-Index, der auch in Ihrem App-Paket enthalten ist. Zur Laufzeit werden vom System die Benutzer-und Computereinstellungen erkannt, die in Kraft sind, die Informationen in der PRI werden und automatisch die Ressourcen geladen, die für diese Einstellungen am besten geeignet sind.

## <a name="package-resource-index-pri-file"></a>Paket Ressourcen Index Datei (PRI)
Jedes app-Paket sollte einen binären Index der Ressourcen in der App enthalten. Dieser Index wird zum Zeitpunkt der Erstellung erstellt und ist in mindestens einer Paket Ressourcen Indexdatei (PRI) enthalten.

- Eine PRI-Datei enthält tatsächliche Zeichen folgen Ressourcen und einen indizierten Satz von Dateipfaden, die auf verschiedene Dateien im Paket verweisen.
- Ein Paket enthält in der Regel eine einzelne PRI-Datei pro Sprache mit dem Namen Resources. pri.
- Die Datei "Resources. pri" im Stammverzeichnis jedes Pakets wird automatisch geladen, wenn " [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) " instanziiert wird.
- PRI-Dateien können mit dem Tool [makepri. exe](compile-resources-manually-with-makepri.md)erstellt und gekippt werden.
- Für eine typische App-Entwicklung benötigen Sie "makepri. exe" nicht, da Sie bereits in den Visual Studio-Kompilierungs Workflow integriert ist. Und Visual Studio unterstützt das Bearbeiten von PRI-Dateien in einer dedizierten Benutzeroberfläche. Die lokalisierungssoren und die verwendeten Tools können sich jedoch auf "makepri. exe" stützen.
- Jede PRI-Datei enthält eine benannte Auflistung von Ressourcen, die als Ressourcenzuordnung bezeichnet wird. Wenn eine PRI-Datei aus einem Paket geladen wird, wird der Name der Ressourcen Zuordnung so überprüft, dass er mit dem Namen der Paket Identität identisch ist.
- PRI-Dateien enthalten nur Daten, sodass Sie nicht das PE-Format (portable ausführbare Datei) verwenden. Sie sind speziell darauf ausgelegt, nur Daten als das Ressourcen Format für Windows zu haben. Sie ersetzen Ressourcen, die in DLLs im Win32-App-Modell enthalten sind.

## <a name="uwp-api-access-to-app-resources"></a>UWP-API-Zugriff auf App-Ressourcen

### <a name="basic-functionality-resourceloader"></a>Grundlegende Funktionalität (resourceloader)
Die einfachste Möglichkeit, Programm gesteuert auf Ihre APP-Ressourcen zuzugreifen, ist die Verwendung des [**Windows. applicationmodel. Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) -Namespace und der [**resourceloader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) -Klasse. **Resourceloader** ermöglicht Ihnen den einfachen Zugriff auf Zeichen folgen Ressourcen aus dem Satz von Ressourcen Dateien, referenzierten Bibliotheken oder anderen Paketen.

### <a name="advanced-functionality-resourcemanager"></a>Erweiterte Funktionalität (ResourceManager)
Die [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) -Klasse (im [**Windows. applicationmodel. resources. Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) -Namespace) bietet zusätzliche Informationen zu Ressourcen, z. b. Enumeration und Überprüfung. Dies geht über die Bereitstellung der **resourceloader** -Klasse hinaus.

Ein [**namedresource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) -Objekt stellt eine einzelne logische Ressource mit mehreren Sprachen oder anderen qualifizierten Varianten dar. Es beschreibt die logische Ansicht des Assets oder der Ressource mit einem Zeichen folgen-Ressourcen Bezeichner `Header1`wie oder einem Ressourcen Dateinamen, `logo.jpg`z. b..

Ein [**resourcecandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) -Objekt stellt einen einzelnen konkreten Ressourcen Wert und seine Qualifizierer, z. b. die Zeichenfolge "Hallo Welt" für Englisch oder "Logo. Scale-100. jpg", als qualifizierte Bildressource dar, die für die Auflösung von " **Scale-100** " spezifisch ist.

Ressourcen, die für eine app verfügbar sind, werden in hierarchischen Sammlungen gespeichert, auf die Sie mit einem [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) -Objekt zugreifen können. Die **ResourceManager** -Klasse bietet Zugriff auf die verschiedenen von der APP verwendeten **ResourceMap** -Instanzen der obersten Ebene, die den verschiedenen Paketen für die App entsprechen. Der [**mainresourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) -Wert entspricht der Ressourcen Zuordnung für das aktuelle App-Paket und schließt alle referenzierten Framework-Pakete aus. Jede **ResourceMap** wird als Paketname benannt, der im Manifest des Pakets angegeben wird. In einer " **ResourceMap** " sind Unterstrukturen (siehe " [**ResourceMap. getsubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)") enthalten, die weitere **namedresource** -Objekte enthalten. Die Teil Strukturen entsprechen in der Regel den Ressourcen Dateien, die die Ressource enthalten. Weitere Informationen finden [Sie unter Lokalisieren von Zeichen folgen in Ihrer Benutzeroberfläche und im App-Paket Manifest](localize-strings-ui-manifest.md) und [Laden von Images und Assets, die auf Skalierung, Design, hoher Kontrast und andere zugeschnitten](images-tailored-for-scale-theme-contrast.md)sind.

Hier sehen Sie ein Beispiel.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Hinweis** Der Ressourcen Bezeichner wird als Uniform Resource Identifier (URI)-Fragment behandelt und unterliegt der URI-Semantik. Beispielsweise `GetValue("Caption%20")` wird als `GetValue("Caption ")`behandelt. Verwenden Sie nicht "?" oder "#" in Ressourcen bezeichgern, da die Auswertung des Ressourcen Pfads beendet wird. Beispiel: "MyResource? 3" wird als "MyResource" behandelt.

Der **ResourceManager** -Dienst unterstützt nicht nur den Zugriff auf die Zeichen folgen Ressourcen einer APP, sondern auch die Möglichkeit, die verschiedenen Datei Ressourcen aufzulisten und zu überprüfen. Um Konflikte zwischen Dateien und anderen Ressourcen zu vermeiden, die aus einer Datei stammen, befinden sich alle indizierten Dateipfade in einer reservierten **ResourceMap** -Unterstruktur. Beispielsweise entspricht die Datei `\Images\logo.png` dem Ressourcennamen `Files/images/logo.png`.

Die [**storagefile**](/uwp/api/Windows.Storage.StorageFile?branch=live) -APIs behandeln Verweise auf Dateien transparent als Ressourcen und sind für typische Verwendungs Szenarien geeignet. Der **ResourceManager** sollte nur für erweiterte Szenarien verwendet werden, z. b. Wenn Sie den aktuellen Kontext überschreiben möchten.

### <a name="resourcecontext"></a>ResourceContext
Ressourcen Kandidaten werden basierend auf einem bestimmten [**resourcecontext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)ausgewählt. dabei handelt es sich um eine Sammlung von Ressourcen Qualifiziererwerten (Sprache, Skalierung, Kontrast usw.). Ein Standardkontext verwendet die aktuelle Konfiguration der APP für jeden qualifiziererwert, es sei denn, Sie wird überschrieben. Beispielsweise können Ressourcen wie z. b. Images für die Skalierung qualifiziert werden, die von einem Monitor zu einem anderen und somit von einer Anwendungs Ansicht zu einem anderen abweicht. Aus diesem Grund hat jede Anwendungs Ansicht einen eindeutigen Standardkontext. Der Standardkontext für eine bestimmte Ansicht kann mithilfe von [**resourcecontext. getforcurrentview**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)abgerufen werden. Wenn Sie einen Ressourcen Kandidaten abrufen, sollten Sie eine **resourcecontext** -Instanz übergeben, um den am besten geeigneten Wert für eine bestimmte Ansicht zu erhalten.

## <a name="important-apis"></a>Wichtige APIs
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Zugehörige Themen
* [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.](images-tailored-for-scale-theme-contrast.md)
