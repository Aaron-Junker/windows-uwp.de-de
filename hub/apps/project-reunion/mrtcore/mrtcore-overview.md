---
description: Übersicht über die MRT-Kernkomponenten und ihre Funktionsweise beim Laden von Anwendungs Ressourcen (Projekt Zusammenführung)
title: Verwalten von Ressourcen für den MRT-Kern (Projekt Zusammenführung)
ms.topic: article
ms.date: 03/09/2021
keywords: MRT, mrtcore, PRI, makepri, Ressourcen, Laden von Ressourcen
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 2b732deb0f387c11b2675193c047d33fa3e55ace
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730484"
---
# <a name="manage-resources-with-mrt-core"></a>Verwalten von Ressourcen mit MRT Core 

MRT Core ist eine optimierte Version des modernen Windows- [Ressourcen Verwaltungssystems](/windows/uwp/app-resources/resource-management-system) , das als Teil der [Projekt Zusammenführung](../index.md)verteilt wird.

MRT Core hat sowohl Build-als auch Lauf Zeitfunktionen. Zum Zeitpunkt der Erstellung erstellt das System einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer APP verpackt werden. Bei diesem Index handelt es sich um den Paket Ressourcen Index oder den PRI-Index, der auch in Ihrem App-Paket enthalten ist.

## <a name="package-resource-index-pri-file"></a>Paket Ressourcen Index Datei (PRI)

Jedes app-Paket sollte einen binären Index der Ressourcen in der App enthalten. Dieser Index wird zum Zeitpunkt der Erstellung erstellt und ist in einer oder mehreren PRI-Dateien enthalten. Jede PRI-Datei enthält eine benannte Auflistung von Ressourcen, die als Ressourcenzuordnung bezeichnet wird.

Eine PRI-Datei enthält tatsächliche Zeichen folgen Ressourcen. Eingebettete Binär-und Datei Pfad Ressourcen werden direkt aus den Projektdateien indiziert. Ein Paket enthält in der Regel eine einzelne PRI-Datei pro Sprache mit dem Namen **Resources. pri**. Die Datei **Resources. pri** im Stammverzeichnis jedes Pakets wird automatisch geladen, wenn das [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) -Objekt instanziiert wird.

PRI-Dateien enthalten nur Daten, sodass Sie nicht das PE-Format (portable ausführbare Datei) verwenden. Sie sind speziell auf Daten beschränkt.

## <a name="access-app-resources"></a>Zugreifen auf App-Ressourcen

MRT Core bietet verschiedene Möglichkeiten, auf Ihre APP-Ressourcen zuzugreifen.

### <a name="basic-functionality-with-resourceloader"></a>Grundlegende Funktionalität mit resourceloader

Die einfachste Möglichkeit, Programm gesteuert auf Ihre APP-Ressourcen zuzugreifen, ist die Verwendung der [resourceloader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader) -Klasse. **Resourceloader** ermöglicht Ihnen den einfachen Zugriff auf Zeichen folgen Ressourcen aus dem Satz von Ressourcen Dateien, referenzierten Bibliotheken oder anderen Paketen.

### <a name="advanced-functionality-with-resourcemanager"></a>Erweiterte Funktionalität mit ResourceManager

Die [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) -Klasse stellt zusätzliche Informationen zu Ressourcen bereit, z. b. Enumeration und Überprüfung. Dies geht über die Bereitstellung der **resourceloader** -Klasse hinaus.

Ein [resourcecandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) -Objekt stellt einen einzelnen konkreten Ressourcen Wert und seine Qualifizierer dar, z. b. die Zeichenfolge "Hallo Welt" für Englisch oder "logo.scale-100.jpg" als qualifizierte Bildressource, die für die Auflösung von "Scale-100" spezifisch ist.

Ressourcen, die für eine app verfügbar sind, werden in hierarchischen Sammlungen gespeichert, auf die Sie mit einem [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) -Objekt zugreifen können. Die **ResourceManager** -Klasse bietet Zugriff auf die verschiedenen von der APP verwendeten **ResourceMap** -Instanzen der obersten Ebene, die den verschiedenen Paketen für die App entsprechen. Der [ResourceManager. mainresourcemap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap) -Wert entspricht der Ressourcen Zuordnung für das aktuelle App-Paket und schließt alle referenzierten Framework-Pakete aus. Jede **ResourceMap** wird als Paketname benannt, der im Manifest des Pakets angegeben wird. In einer " **ResourceMap** " sind Teil Strukturen (siehe " [ResourceMap. getsubtree](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)"). Die Teil Strukturen entsprechen in der Regel den Ressourcen Dateien, die die Ressource enthalten.

Der **ResourceManager** -Dienst unterstützt nicht nur den Zugriff auf die Zeichen folgen Ressourcen einer APP, sondern auch die Möglichkeit, die verschiedenen Datei Ressourcen aufzulisten und zu überprüfen. Um Konflikte zwischen Dateien und anderen Ressourcen zu vermeiden, die aus einer Datei stammen, befinden sich alle indizierten Dateipfade in einer reservierten **ResourceMap** -Unterstruktur. Beispielsweise entspricht die Datei ' \Images\logo.png ' dem Ressourcennamen ' files/Images/logo.png '.

### <a name="qualify-resource-selection-with-resourcecontext"></a>Ressourcen Auswahl mit resourcecontext qualifizieren

Ressourcen Kandidaten werden basierend auf einem bestimmten [resourcecontext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)ausgewählt. dabei handelt es sich um eine Sammlung von Ressourcen Qualifiziererwerten (Sprache, Skalierung, Kontrast usw.). Ein Standardkontext verwendet die aktuelle Konfiguration der APP für jeden qualifiziererwert, es sei denn, Sie wird überschrieben. Beispielsweise können Ressourcen wie z. b. Images für die Skalierung qualifiziert werden, die von einem Monitor zu einem anderen und somit von einer Anwendungs Ansicht zu einem anderen abweicht. Aus diesem Grund hat jede Anwendungs Ansicht einen eindeutigen Standardkontext. Wenn Sie einen Ressourcen Kandidaten abrufen, sollten Sie eine **resourcecontext** -Instanz übergeben, um den am besten geeigneten Wert für eine bestimmte Ansicht zu erhalten.

### <a name="load-images"></a>Images laden

Wenn Sie MRT Core zum Abrufen von Bildern verwenden möchten, die Sie Ihrem Projekt hinzugefügt haben, müssen Sie das Abbild so konfigurieren, dass es als Inhalt erstellt wird. Wenn Sie dies nicht tun, wird das Image nicht in der Datei "Resources. pri" indiziert und kann nicht von MRT Core abgerufen werden.

So konfigurieren Sie ein Bild, das als Inhalt erstellt werden soll:

* Legen Sie in einem c#-Projekt/.net 5 **die Eigenschaft Buildvorgang für das Bild** auf **Inhalt** fest.
* Legen Sie in einem C++/WinRT-Projekt die **Content** -Eigenschaft für das Bild auf **true** fest.

## <a name="sample"></a>Beispiel

Ein Beispiel für die Verwendung der MRT Core-API finden Sie im Beispiel für den [MRT](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)-Kern.
