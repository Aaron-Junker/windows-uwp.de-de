---
Description: Beschreibt die Funktionen Navigationszwecken und die Überschriften der Barrierefreiheit.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Orientierungspunkte und Überschriften
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 28a1bd46646deae8f6e6519d72295d42ce2766db
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359556"
---
# <a name="landmarks-and-headings"></a>Orientierungspunkte und Überschriften

Eine Benutzeroberfläche ist in der Regel visuell effizient organisiert, so dass ein Benutzer schnell nach dem suchen kann, was ihn interessiert, ohne den *gesamten* Inhalt lesen zu müssen. Ein Sprachausgabe-Benutzer muss die gleiche Möglichkeit haben. Orientierungspunkte und Überschriften definieren Bereiche einer Benutzeroberfläche, die Benutzer bei der effizienten Navigation von Hilfstechnologien (Assistive-Technology, AT) unterstützen. Die Markierung der Inhalte in Orientierungspunkten und Überschriften bietet einem Sprachausgabe-Benutzer die Möglichkeit, Inhalte ähnlich wie andere Benutzer zu überfliegen.

Die Konzepte von [ARIA-Orientierungspunkten](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page),[ ARIA-Überschriften](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html) und [HTML-Überschriften](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) werden seit Jahren in Webinhalten verwendet, um eine schnellere Navigation für Sprachausgabe-Benutzer zu ermöglichen. Webseiten verwenden Orientierungspunkte und Überschriften, um ihren Inhalt besser nutzbar zu machen, indem sie es dem AT-Benutzer ermöglichen, schnell zum großen (Orientierungspunkt) und kleinen (Überschrift) Teilen zu gelangen. Insbesondere ermöglichen Sprachausgabe Befehle dem Benutzer, zwischen Orientierungspunkten und Überschriften zu springen (nächste/vorherige oder bestimmte Überschriftenebene). Aus den gleichen Gründen ist es wichtig, Orientierungspunkte und Überschriften in Ihren Apps zu berücksichtigen.

Orientierungspunkte ermöglichen es, Inhalte in verschiedene Kategorien wie Suche, Navigation, Hauptinhalte usw. zu gruppieren. Einmal gruppiert, kann der AT-Benutzer schnell zwischen den Gruppen navigieren. Diese schnelle Navigation ermöglicht es dem Benutzer, potentiell große Mengen an Inhalten zu überspringen, für die zuvor möglicherweise über einzelne Elemente navigiert werden musste, was zu einer schlechten Erfahrung führt. 

Wenn Sie z. B. ein Registerkartenfeld verwenden, berücksichtigen Sie dies als eine Navigations-Orientierungspunkt. Wenn Sie ein Such-Eingabefeld verwenden, berücksichtigen Sie dieses als einen Such-Orientierungspunkt und erwägen Sie, Ihren Hauptinhalt als einen Haupt-Orientierungspunkt zu nutzen. Ob innerhalb und außerhalb eines Orientierungspunkts, berücksichtigen Sie Unterelemente als Überschriften mit logischen Überschriftenebenen. 

Berücksichtigen Sie die Seite **Erleichterte Bedienung** in der Windows-Einstellungen-App. 

![Die „Erleichterte Bedienung”-Seite in der Windows-Einstellungen-App](images/EaseOfAccessSettings.png)  

Es gibt ein Such-Eingabefeld, das in einen Such-Orientierungspunkt eingeschlossen ist. Die Navigationselemente auf der linken Seite sind in einen Navigations-Orientierungspunkt und der Hauptinhalt auf der rechten Seite in einen Hauptinhalts-Orientierungspunkt eingeschlossen. In der Navigationsleiste gibt es eine Hauptgruppenüberschrift mit dem Namen **Erleichterte Bedienung**, die eine Überschrift der Ebene 1 ist. Darunter befinden sich die Unteroptionen **Sprachausgabe**, **Bildchirmlupe** und so weiter. Sie haben die Überschriftebene 2. Das Setzen von Überschriften wird innerhalb des Hauptinhalts fortgesetzt, wobei das Hauptthema (**Anzeige**) als Überschriftenebene 1 und Untergruppen wie z. B. **Alles größer** als Überschriftenebene 2 eingestellt wird. 

Einstellungen-App wäre auch ohne Orientierungspunkte und Überschriften zugänglich, aber sie wird damit besser nutzbar. Ein Sprachausgabe-Benutzer kann schnell und einfach zu der Gruppe (Orientierungspunkt) gelangen, die er benötigt, und dann auch schnell zur Untergruppe (Überschrift). 

Verwenden Sie [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty), um das UI-Element als den gewünschten [Orientierungspunkttyp](https://docs.microsoft.com/windows/desktop/WinAuto/landmark-type-identifiers) einzurichten. Dieses Orientierungspunkt-UI-Element würde alle anderen UI-Elemente kapseln, die für diesen Orientierungspunkt sinnvoll sind. 

Verwenden Sie [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty), um die Orientierungspunkte spezifisch zu benennen. Wenn Sie einen vordefinierten Orientierungspunkttyp wie Haupt- oder Navigationsname auswählen, werden diese Namen für den Orientierungspunktnamen verwendet. Wenn Sie jedoch den Orientierungspunkttyp auf benutzerdefiniert setzen, müssen Sie den Orientierungspunkt über diese Eigenschaft spezifisch benennen. Sie können diese Eigenschaft auch verwenden, um die Standardnamen der nicht benutzerdefinierten Orientierungspunkttypen zu überschreiben. 

Verwenden Sie [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty), um das UI-Element als Überschrift einer bestimmten Ebene von *Level1* bis *Level9* zu setzen.

