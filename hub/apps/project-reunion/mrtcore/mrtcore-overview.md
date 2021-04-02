---
description: Übersicht über die MRT-Kernkomponenten und ihre Funktionsweise beim Laden von Anwendungs Ressourcen (Projekt Zusammenführung)
title: Verwalten von Ressourcen für den MRT-Kern (Projekt Zusammenführung)
ms.topic: article
ms.date: 03/31/2021
keywords: MRT, mrtcore, PRI, makepri, Ressourcen, Laden von Ressourcen
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 4b86e3d1b232a9c0da87808e8fa07a13b18ffdd6
ms.sourcegitcommit: d793c82587b8368e241d74be1473f4f0af5bb9ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106164370"
---
# <a name="manage-resources-with-mrt-core"></a>Verwalten von Ressourcen mit MRT Core 

MRT Core ist eine optimierte Version des modernen Windows- [Ressourcen Verwaltungssystems](/windows/uwp/app-resources/resource-management-system) , das als Teil der [Projekt Zusammenführung](../index.md)verteilt wird.

MRT Core hat sowohl Build-als auch Lauf Zeitfunktionen. Zum Zeitpunkt der Erstellung erstellt das System einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer APP verpackt werden. Bei diesem Index handelt es sich um den Paket Ressourcen Index oder den PRI-Index, der auch in Ihrem App-Paket enthalten ist.

## <a name="package-resource-index-pri-file"></a>Paket Ressourcen Index Datei (PRI)

Jedes app-Paket sollte einen binären Index der Ressourcen in der App enthalten. Dieser Index wird zum Zeitpunkt der Erstellung erstellt und ist in einer oder mehreren PRI-Dateien enthalten. Jede PRI-Datei enthält eine benannte Auflistung von Ressourcen, die als Ressourcenzuordnung bezeichnet wird.

Eine PRI-Datei enthält tatsächliche Zeichen folgen Ressourcen. Eingebettete Binär-und Datei Pfad Ressourcen werden direkt aus den Projektdateien indiziert. Ein Paket enthält in der Regel eine einzelne PRI-Datei pro Sprache mit dem Namen **Resources. pri**. Die Datei **Resources. pri** im Stammverzeichnis jedes Pakets wird automatisch geladen, wenn das [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) -Objekt instanziiert wird.

PRI-Dateien enthalten nur Daten, sodass Sie nicht das PE-Format (portable ausführbare Datei) verwenden. Sie sind speziell auf Daten beschränkt.

> [!NOTE]
> Bevor Sie mit MRT Core Zeichen folgen und Bilder in einem WinUI 3-Projekt abrufen können, das c#/.net 5 verwendet, müssen Sie sicherstellen, dass diese Ressourcen so konfiguriert sind, dass Sie in der Datei "Resources. pri" indiziert werden können. Andernfalls können diese Ressourcen nicht von MRT Core abgerufen werden.
>
> * Stellen Sie für eine Zeichen folgen-Ressourcen Datei (. resw) sicher, dass die Eigenschaft Buildvorgang **für die Datei** auf **priresource** festgelegt ist.
> * Stellen Sie für eine Bilddatei sicher, dass die Eigenschaft Buildvorgang **für die Datei** auf **Inhalt** festgelegt ist.

## <a name="access-app-resources-with-mrt-core"></a>Zugreifen auf App-Ressourcen mit MRT Core

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

## <a name="sample"></a>Beispiel

Ein Beispiel für die Verwendung der MRT Core-API finden Sie im Beispiel für den [MRT](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)-Kern.
