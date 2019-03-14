---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: "In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen und Drag\_&\_Drop."
title: App-zu-App-Kommunikation
ms.date: 02/08/2017
ms.topic: article
keywords: 'Windows 10, UWP'
ms.localizationpriority: medium
---
# <a name="app-to-app-communication"></a>App-zu-App-Kommunikation


In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen und Drag & Drop.

Der Freigabe-Vertrag ermöglicht Benutzern das schnelle Austauschen von Daten zwischen Apps. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern. Wenn Ihre App Inhalte in Szenarien empfängt, die ein Benutzer in einer anderen App schnell abschließen kann, sollten Sie einen Freigabe-Vertrag erwägen.

Eine App kann das Feature „Freigeben“ auf zwei unterschiedliche Arten unterstützen. Zunächst kann sie eine Quell-App sein, die vom Benutzer freizugebende Inhalte bereitstellt. Außerdem kann es eine Ziel-App geben, die vom Benutzer als Ziel für die freigegebenen Inhalte ausgewählt wird. Eine App kann sowohl Quell- als auch Ziel-App sein. Wenn Ihre App als Quell-App zum Teilen von Inhalten fungieren soll, müssen Sie festlegen, welche Datenformate die App bereitstellen kann.

Zusätzlich zum Freigabe-Vertrag können in Apps auch herkömmliche Verfahren zum Übertragen von Daten integriert sein, z. B. Drag & Drop und Kopieren und Einfügen. Neben der Kommunikation zwischen UWP-Apps unterstützen diese Methoden auch die Freigabe für Desktopanwendungen.



## <a name="in-this-section"></a>Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Freigeben von Daten](share-data.md) | In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern. |
| [Empfangen von Daten](receive-data.md) | In diesem Artikel wird erläutert, wie Sie in Ihrer UWP-App Inhalte empfangen, die in einer anderen App mithilfe des Freigabe-Vertrags freigegeben wurden. Mit diesem Freigabe-Vertrag kann Ihre App als Option angezeigt werden, wenn der Benutzer „Freigeben“ aufruft. |
| [Kopieren und Einfügen](copy-and-paste.md) | In diesem Artikel wird erläutert, wie das Kopieren und Einfügen mit der Zwischenablage in UWP-Apps unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen. |
| [Drag & Drop](../design/input/drag-and-drop.md) | In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App Drag & Drop hinzufügen. Drag & Drop ist ein klassisches, natürliches Interaktionsmodell für Inhalte wie Bilder und Dateien. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung. |

## <a name="see-also"></a>Siehe auch
- [Entwickeln von UWP-Apps](https://developer.microsoft.com/windows/develop)
