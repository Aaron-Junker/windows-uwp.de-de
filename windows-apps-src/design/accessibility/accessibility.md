---
description: Führt Barrierefreiheits Konzepte ein, die sich auf Windows-apps beziehen.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: Zugriff
label: Accessibility
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f49c784dcd6ed8872753bf4aedd9e79718ccfd99
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032604"
---
# <a name="accessibility"></a>Zugriff  

Barrierefreiheit bezieht sich auf das Erstellen von Umgebungen, mit denen Ihre Windows-Anwendung von Benutzern verwendet werden kann, die Technologie in einer Vielzahl von Umgebungen verwenden und sich mit einer Vielzahl von Anforderungen und Erfahrungen an Ihre Benutzeroberfläche wenden. Für bestimmte Situationen gelten gesetzlich vorgeschriebene Vorgaben im Hinblick auf Barrierefreiheit. Es wird jedoch empfohlen, die Barrierefreiheit unabhängig von gesetzlichen Anforderungen zu berücksichtigen, um sicherzustellen, dass Ihre Apps die größtmögliche Benutzergruppe erreichen.

> Außerdem gibt es eine Microsoft Store Deklaration bezüglich des Zugriffs auf die app.

| Artikel | Beschreibung |
|---------|-------------|
| [Barrierefreiheit im Überblick](accessibility-overview.md) | Dieser Artikel stellt eine Übersicht über die Konzepte und Technologien im Zusammenhang mit Barrierefreiheits Szenarien für Windows-apps dar. |
| [Entwerfen von barrierefreier Software](designing-inclusive-software.md) | Erfahren Sie mehr über die Entwicklung eines inklusiven Entwurfs mit Windows-Apps für Windows 10.  Entwerfen und erstellen Sie inklusive Software unter Berücksichtigung der Barrierefreiheit. |
| [Entwickeln von barrierefreien Windows-Apps](developing-inclusive-windows-apps.md) | Dieser Artikel ist eine Roadmap für die Entwicklung barrierefreier Windows-apps. |
| [Testen der Barrierefreiheit](accessibility-testing.md) | Zu befolgende Test Prozeduren, um sicherzustellen, dass die Windows-App zugänglich ist |
| [Barrierefreiheit im Store](accessibility-in-the-store.md) | Beschreibt die Anforderungen für das Deklarieren Ihrer Windows-App im Microsoft Store als verfügbar. |
| [Prüfliste für die Barrierefreiheit](accessibility-checklist.md) | Bietet eine Prüfliste, um sicherzustellen, dass die Windows-App zugänglich ist. |
| [Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit](basic-accessibility-information.md) | Grundlegende Informationen zur Barrierefreiheit werden häufig in die Kategorien Name, Rolle und Wert unterteilt. In diesem Thema wird der Code beschrieben, mit dem Ihre App die grundlegenden Informationen verfügbar machen kann, die für Hilfstechnologien erforderlich sind. |
| [Barrierefreiheit der Tastaturnavigation](keyboard-accessibility.md) | Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen. |
| [Sprachausgaben und Hardwaresystemschaltflächen](system-button-narration.md) | Bildschirme (z. b. die Sprachausgabe) müssen in der Lage [sein, Hardware](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator)System-Schaltflächen Ereignisse zu erkennen und zu behandeln und ihren Zustand Benutzern mitzuteilen. In einigen Fällen muss die Sprachausgabe möglicherweise nur Schaltflächen Ereignisse behandeln und Sie nicht an andere Handler weitergeben. |
| [Orientierungspunkte und Überschriften](landmarks-and-headings.md) | „Unterstützte Orientierungspunkte und Überschriften” definieren Bereiche einer Benutzeroberfläche, die Benutzer bei der effizienten Navigation von Hilfstechnologien wie Bildschirmleseprogrammen unterstützen. |
| [Designs mit hohem Kontrast](high-contrast-themes.md) | Beschreibt die erforderlichen Schritte, um sicherzustellen, dass Ihre Windows-App verwendbar ist, wenn ein Design mit hohem Kontrast aktiv ist. |
| [Anforderungen für barrierefreien Text](accessible-text-requirements.md) | In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist. In diesem Thema werden auch die Microsoft-Benutzeroberflächenautomatisierungs-Rollen erläutert, die Textelemente in einer Windows-APP haben können, sowie bewährte Methoden für Text in Grafiken. |
| [Nicht empfehlenswerte Praktiken für die Barrierefreiheit](practices-to-avoid.md) | Listet die Vorgehensweisen auf, um zu vermeiden, dass Sie eine barrierefreie Windows-app erstellen möchten. |
| [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md) | Beschreibt das Konzept von Automatisierungspeers für die Benutzeroberflächenautomatisierung und erläutert, wie Sie Unterstützung zur Benutzeroberflächenautomatisierung für eigene benutzerdefinierte UI-Klassen bereitstellen können. |
| [Steuerelement Muster und-Schnittstellen](control-patterns-and-interfaces.md) | Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden. |

## <a name="related-topics"></a>Verwandte Themen  
* [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation) 
* [Einstieg in die Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
