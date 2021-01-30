---
description: Erweitern Sie die Grundfunktionen von **Cortana** durch Sprachbefehle, die eine einzelne Aktion in einer Windows-Anwendung starten und ausführen.
title: Cortana-Interaktionen in Windows-apps
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, Cortana-Canvas, Cortana-Design, Benutzeroberfläche, Sprachbefehle, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ca9f77d10f6e22d4e244b102cb8b85e1f75113fc
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057561"
---
# <a name="cortana-interactions-in-windows-apps"></a>Cortana-Interaktionen in Windows-apps

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.

Erweitern Sie die Grundfunktionen von **Cortana** durch Sprachbefehle, die eine einzelne Aktion in einer Windows-Anwendung starten und ausführen.

Die Ziel-App kann abhängig von der Komplexität der Interaktion entweder im Vordergrund gestartet (die App erhält den Fokus, und **Cortana** wird geschlossen) oder im Hintergrund aktiviert werden (**Cortana** behält den Fokus und zeigt Ergebnisse aus der App an). Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern, werden im Allgemeinen am besten in einer Vordergrund-App behandelt, während grundlegende Befehle in **Cortana** über eine Hintergrund-App behandelt werden können.

**Cortana** integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer. Wenn Sie diese Verknüpfung für die APP-Funktionalität bereitstellen und die Notwendigkeit, apps zu wechseln, verringern, kann der Benutzer viel Zeit und Aufwand sparen.

> [!NOTE]
> Ein Sprachbefehl ist eine einzelne Äußerung mit einer bestimmten Absicht, die in einer sprach Befehls Definitionsdatei (VCD) definiert ist, die an eine installierte App über **Cortana** gerichtet ist.
>
> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.
>
> Sprach Befehlsdefinitionen können von der Komplexität abweichen. Sie können alles von einer einzelnen, eingeschränkten Äußerung zu einer Sammlung flexibler, natürlicher Spracheingaben unterstützen, die alle dieselbe Absicht bezeichnen.

## <a name="other-speech-and-conversation-components"></a>Weitere sprach-und Konversations Komponenten

### <a name="speech-voice-and-conversation-in-windows-10"></a>Sprache, Stimme und Konversation in Windows 10

Informationen dazu, wie die verschiedenen Windows-Entwicklungs Frameworks Spracherkennung, Sprachsynthese und Konversations Unterstützung für Entwickler bereitstellen, die Windows-Anwendungen erstellen, finden Sie unter [Sprache, Stimme und Konversation in Windows 10](/windows/apps/speech) .

### <a name="cortana-skills-kit"></a>Cortana-Funktionskit

Sehen Sie sich das [Cortana Skills Kit](/cortana/skills/) an, wenn Sie Cortana erweitern möchten, indem Sie Ihre eigenen Fähigkeiten hinzufügen, die es Benutzern ermöglichen, über Cortana mit Ihrem **Dienst** zu interagieren. [**Veraltete Hinweise:** als Teil unseres Ziels, die modernen Produktivitäts Erfahrungen zu transformieren, indem Cortana tief in Microsoft 365 eingebettet wird, wird das Cortana Skills Kit für Consumer (die Entwicklerplattform) und alle Fähigkeiten, die auf dieser Plattform erstellt wurden, zurückgezogen.

## <a name="related-articles"></a>Verwandte Artikel

- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)
