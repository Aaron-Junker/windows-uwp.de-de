---
description: Beschreibt die Anforderungen für das Deklarieren Ihrer Windows-App im Microsoft Store als verfügbar.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Barrierefreiheit im Store
label: Accessibility in the Store
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4a46f757629489250ed417a9a67488cba7ec7240
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032703"
---
# <a name="accessibility-in-the-store"></a>Barrierefreiheit im Store  



Beschreibt die Anforderungen für das Deklarieren Ihrer Windows-App im Microsoft Store als verfügbar.

Wenn Sie Ihre APP an den Microsoft Store zur Zertifizierung übermitteln, können Sie Ihre APP als verfügbar deklarieren. Indem Sie Ihre App als barrierefrei deklarieren, kann sie von Benutzern, die an barrierefreien Apps interessiert sind (z. B. Benutzer mit Sehschwächen), leichter gefunden werden. Benutzer ermitteln barrierefreie apps, indem Sie den **zugänglichen** Filter verwenden, während Sie die Microsoft Store durchsuchen. Wenn Sie Ihre App als barrierefrei deklarieren, wird der Beschreibung Ihrer App auch das Kennzeichen **Barrierefrei** hinzugefügt.

Indem Sie Ihre App als barrierefrei deklarieren, erklären Sie, dass sie über die [grundlegenden Informationen zur Barrierefreiheit](basic-accessibility-information.md) verfügt, die Benutzer für wichtige Szenarien unter Verwendung einer oder mehrerer der folgenden Dinge benötigen:

* Die Tastatur.
* Ein Design mit hohem Kontrast.
* Eine variable dpi-Einstellung (dots per inch).
* Allgemeine Hilfstechnologien wie z. B. die Windows-Eingabehilfefunktionen, darunter Sprachausgabe, Bildschirmlupe und Bildschirmtastatur.

Sie sollten Ihre App als barrierefrei erklären, wenn Sie diese unter Berücksichtigung der Barrierefreiheit erstellt und getestet haben. Dafür müssen Sie Folgendes gemacht haben:

* Alle relevanten Barrierefreiheitsinfos für UI-Elemente sind festgelegt, einschließlich Name, Rolle, Wert usw.
* Barrierefreiheit für den Tastaturzugriff implementieren. Benutzer haben so folgende Möglichkeiten:
    * Sämtliche wichtigen App-Szenarien nur mithilfe der Tastatur auszuführen.
    * UI-Elemente in einer logischen Reihenfolge durchzugehen.
    * Mithilfe der Pfeiltasten zwischen den UI-Elementen eines Steuerelements zu navigieren.
    * Tastenkombinationen für wichtige Funktionen der App zu verwenden.
    * Verwenden von Sprachausgabe-Touchgesten für TAB- und Pfeilnavigation für Geräte ohne Tastatur.
* Visuelle Barrierefreiheit der App-UI sicherstellen: Kontrastverhältnis von mindestens 4.5:1, keine ausschließlich auf Farben basierende Darstellung von Informationen usw.
* Tools zum Testen der Barrierefreiheit wurden eingesetzt, z. B. [**Inspect**](/windows/desktop/WinAuto/inspect-objects) und [**UIAVerify**](/windows/desktop/WinAuto/ui-automation-verify), um die Implementierung der Barrierefreiheit zu überprüfen und alle Fehler der Priorität 1 zu beheben, die von diesen Tools gemeldet werden.
* Die primären Szenarien Ihrer App unter Verwendung von Sprachausgabe, Bildschirmlupe, Bildschirmtastatur, einem Design mit hohem Kontrast und angepassten DPI-Einstellungen wurden überprüft.

Unter [Prüfliste für die Barrierefreiheit](accessibility-checklist.md) werden diese Verfahren erläutert. Zudem finden Sie dort Links zu Ressourcen, die Ihnen beim Durchführen der Verfahren helfen.

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen    
* [Bedienungshilfen](accessibility.md)
