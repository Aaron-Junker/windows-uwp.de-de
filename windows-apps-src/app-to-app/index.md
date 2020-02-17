---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen und Drag & Drop.
title: App-zu-App-Kommunikation
ms.date: 02/12/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2de9d5ae04c526c7c35fda4a7aef713814b4fbc2
ms.sourcegitcommit: 366c96d16e9a330bd4fbd9e1e19983f890f72642
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77258337"
---
# <a name="app-to-app-communication"></a>App-zu-App-Kommunikation

In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen, Drag & Drop und App-Dienste.

Der Freigabe-Vertrag ermöglicht Benutzern das schnelle Austauschen von Daten zwischen Apps. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern. Wenn Ihre App Inhalte in Szenarien empfängt, die ein Benutzer in einer anderen App schnell abschließen kann, sollten Sie einen Freigabe-Vertrag erwägen.

Eine App kann das Feature „Freigeben“ auf zwei unterschiedliche Arten unterstützen. Zunächst kann sie eine [Quell-App sein, die vom Benutzer freizugebende Inhalte bereitstellt](share-data.md). Außerdem kann es eine [Ziel-App geben, die vom Benutzer als Ziel für die freigegebenen Inhalte ausgewählt wird](receive-data.md). Eine App kann sowohl Quell- als auch Ziel-App sein. Wenn Ihre App als Quell-App zum Teilen von Inhalten fungieren soll, müssen Sie festlegen, welche Datenformate die App bereitstellen kann.

Zusätzlich zum Freigabe-Vertrag können in Apps auch herkömmliche Verfahren zum Übertragen von Daten integriert sein, z. B. [Drag & Drop](../design/input/drag-and-drop.md) oder [Kopieren und Einfügen](copy-and-paste.md). Neben der Kommunikation zwischen UWP-Apps unterstützen diese Methoden auch die Freigabe für Desktopanwendungen.

UWP-Apps können auch [App-Dienste](../launch-resume/how-to-create-and-consume-an-app-service.md) erstellen, die Funktionen für andere UWP-Apps bereitstellen. Ein App-Dienst wird als Hintergrundaufgabe in der Host-App ausgeführt und kann seine Dienste auch anderen Apps bereitstellen. Beispielsweise kann der Barcode-Scanner eines App-Dienstes auch anderen Apps nützlich sein.

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Freigeben von Daten](share-data.md) | In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern. |
| [Empfangen von Daten](receive-data.md) | In diesem Artikel wird erläutert, wie Sie in Ihrer UWP-App Inhalte empfangen, die in einer anderen App mithilfe des Freigabe-Vertrags freigegeben wurden. Mit diesem Freigabe-Vertrag kann Ihre App als Option angezeigt werden, wenn der Benutzer „Freigeben“ aufruft. |
| [Kopieren und Einfügen](copy-and-paste.md) | In diesem Artikel wird erläutert, wie das Kopieren und Einfügen mit der Zwischenablage in UWP-Apps unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen. |
| [Drag & Drop](../design/input/drag-and-drop.md) | In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App Drag & Drop hinzufügen. Drag & Drop ist ein klassisches, natürliches Interaktionsmodell für Inhalte wie Bilder und Dateien. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung. |
| [Erstellen und Verwenden eines App-Diensts](../launch-resume/how-to-create-and-consume-an-app-service.md) | In diesem Artikel wird erläutert, wie Sie einen App-Dienst in einer UWP-App erstellen, die Dienste für andere UWP-Apps bereitstellt.  |

## <a name="see-also"></a>Siehe auch

- [Entwickeln von UWP-Apps](https://docs.microsoft.com/windows/uwp/develop/)
