---
description: Übersicht über die MRT-Kernkomponenten und ihre Funktionsweise beim Laden von Anwendungs Ressourcen (Projekt Zusammenführung)
title: Einführung in MRT Core (Projekt Zusammenführung)
ms.topic: article
ms.date: 12/11/2020
keywords: MRT, mrtcore, PRI, makepri, Ressourcen, Laden von Ressourcen
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 0039d2a585f850bd7a15afc619d3500c092de50b
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349384"
---
# <a name="introduction-to-mrt-core-project-reunion"></a>Einführung in MRT Core (Projekt Zusammenführung)

MRT Core ist eine optimierte Version des modernen Windows- [Ressourcen Verwaltungssystems](/windows/uwp/app-resources/resource-management-system) , das als Teil der [Projekt Zusammenführung](../index.md)verteilt wird.

MRT Core hat sowohl Build-als auch Lauf Zeitfunktionen. Zum Zeitpunkt der Erstellung erstellt das System einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer APP verpackt werden. Bei diesem Index handelt es sich um den Paket Ressourcen Index oder den PRI-Index, der auch in Ihrem App-Paket enthalten ist. Zur Laufzeit werden vom System die Benutzer-und Computereinstellungen erkannt, die in Kraft sind, die Informationen in der PRI werden und automatisch die Ressourcen geladen, die für diese Einstellungen am besten geeignet sind.

## <a name="package-resource-index-pri-file"></a>Paket Ressourcen Index Datei (PRI)

Jedes app-Paket sollte einen binären Index der Ressourcen in der App enthalten. Dieser Index wird zum Zeitpunkt der Erstellung erstellt und ist in mindestens einer Ressourcen Datei (. resw) enthalten.

Eine resw-Datei enthält tatsächliche Zeichen folgen Ressourcen und einen indizierten Satz von Dateipfaden, die auf verschiedene Dateien im Paket verweisen.
Ein Paket enthält in der Regel eine einzelne resw-Datei mit dem Namen Resources. resw. Die Datei Resources. resw im Stammverzeichnis jedes Pakets wird automatisch geladen, wenn ResourceManager instanziiert wird.

Jede resw-Datei enthält eine benannte Auflistung von Ressourcen, die als Ressourcen Zuordnung bezeichnet wird. Wenn eine resw-Datei aus einem Paket geladen wird, wird der Name der Ressourcen Zuordnung so überprüft, dass er mit dem Namen der Paket Identität identisch ist.

Resw-Dateien enthalten nur Daten, sodass Sie nicht das PE-Format (portable ausführbare Datei) verwenden. Sie sind speziell auf Daten beschränkt.

## <a name="using-mrt-core-to-access-app-resources"></a>Verwenden von MRT Core zum Zugreifen auf App-Ressourcen

### <a name="resource-loader-basic-functionality"></a>Ressourcen Lader (grundlegende Funktionalität)

Die einfachste Möglichkeit, Programm gesteuert auf Ihre APP-Ressourcen zuzugreifen, ist die Verwendung des [Microsoft. applicationmodel. Resources](/windows/winui/api/microsoft.applicationmodel.resources) -Namespace und der resourceloader-Klasse. Resourceloader ermöglicht Ihnen den einfachen Zugriff auf Zeichen folgen Ressourcen aus dem Satz von Ressourcen Dateien, referenzierten Bibliotheken oder anderen Paketen.

### <a name="resource-manager-advanced-functionality"></a>Ressourcen-Manager (erweiterte Funktionalität)

Die [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) -Klasse stellt zusätzliche Informationen zu Ressourcen bereit, z. b. Enumeration und Überprüfung. Dies geht über die Bereitstellung der **resourceloader** -Klasse hinaus.

Ein [resourcecandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) -Objekt stellt einen einzelnen konkreten Ressourcen Wert und seine Qualifizierer dar, z. b. die Zeichenfolge "Hallo Welt" für Englisch oder "logo.scale-100.jpg" als qualifizierte Bildressource, die für die Auflösung von "Scale-100" spezifisch ist.

Ressourcen, die für eine app verfügbar sind, werden in hierarchischen Sammlungen gespeichert, auf die Sie mit einem [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) -Objekt zugreifen können. Die **ResourceManager** -Klasse bietet Zugriff auf die verschiedenen von der APP verwendeten **ResourceMap** -Instanzen der obersten Ebene, die den verschiedenen Paketen für die App entsprechen. Der [ResourceManager. mainresourcemap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap) -Wert entspricht der Ressourcen Zuordnung für das aktuelle App-Paket und schließt alle referenzierten Framework-Pakete aus. Jede **ResourceMap** wird als Paketname benannt, der im Manifest des Pakets angegeben wird. In einer " **ResourceMap** " sind Teil Strukturen (siehe [ResourceMap. getsubtree]/Windows/WinUI/API/Microsoft.applicationmodel.resources.ResourceMap.getsubtree)), die weitere **namedresource** -Objekte enthalten. Die Teil Strukturen entsprechen in der Regel den Ressourcen Dateien, die die Ressource enthalten.

Beachten Sie, dass der Ressourcen Bezeichner als Uniform Resource Identifier (URI)-Fragment behandelt wird, das der URI-Semantik unterliegt. Beispielsweise `GetValue("Caption%20")` wird als behandelt `GetValue("Caption ")` . Verwenden Sie nicht "?" oder "#" in Ressourcen bezeichgern, da die Auswertung des Ressourcen Pfads beendet wird. Beispiel: "MyResource? 3" wird als "MyResource" behandelt.

Der **ResourceManager** -Dienst unterstützt nicht nur den Zugriff auf die Zeichen folgen Ressourcen einer APP, sondern auch die Möglichkeit, die verschiedenen Datei Ressourcen aufzulisten und zu überprüfen. Um Konflikte zwischen Dateien und anderen Ressourcen zu vermeiden, die aus einer Datei stammen, befinden sich alle indizierten Dateipfade in einer reservierten **ResourceMap** -Unterstruktur. Beispielsweise entspricht die Datei ' \Images\logo.png ' dem Ressourcennamen ' files/Images/logo.png '.

Die [storagefile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) -APIs behandeln Verweise auf Dateien transparent als Ressourcen und sind für typische Verwendungs Szenarien geeignet. Der **ResourceManager** sollte nur für erweiterte Szenarien verwendet werden, z. b. Wenn Sie den aktuellen Kontext überschreiben möchten.

### <a name="resourcecontext"></a>ResourceContext

Ressourcen Kandidaten werden basierend auf einem bestimmten [resourcecontext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)ausgewählt. dabei handelt es sich um eine Sammlung von Ressourcen Qualifiziererwerten (Sprache, Skalierung, Kontrast usw.). Ein Standardkontext verwendet die aktuelle Konfiguration der APP für jeden qualifiziererwert, es sei denn, Sie wird überschrieben. Beispielsweise können Ressourcen wie z. b. Images für die Skalierung qualifiziert werden, die von einem Monitor zu einem anderen und somit von einer Anwendungs Ansicht zu einem anderen abweicht. Aus diesem Grund hat jede Anwendungs Ansicht einen eindeutigen Standardkontext. Der Standardkontext für eine bestimmte Ansicht kann mithilfe von [resourcecontext. getforcurrentview](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)abgerufen werden. Wenn Sie einen Ressourcen Kandidaten abrufen, sollten Sie eine **resourcecontext** -Instanz übergeben, um den am besten geeigneten Wert für eine bestimmte Ansicht zu erhalten.

### <a name="important-apis"></a>Wichtige APIs

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>Beispiel

Ein Beispiel für die Verwendung der MRT Core-API finden Sie im Beispiel für den [MRT](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)-Kern.
