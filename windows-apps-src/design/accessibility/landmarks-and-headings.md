---
description: Beschreibt die Features und Überschriften der Barrierefreiheit.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Orientierungspunkte und Überschriften
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 942c24e8f5c7c521502ee5a9f9eb7175bf04b94f
ms.sourcegitcommit: 2a1ceeacf5cdadc803bad83dc3ceb57a16ce79a3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89067512"
---
# <a name="landmarks-and-headings"></a>Orientierungspunkte und Überschriften

Eine Benutzeroberfläche ist in der Regel auf visuell effiziente Weise organisiert, sodass ein aussehender Benutzer schnell in die gewünschten Elemente einst, ohne dass Sie den *gesamten* Inhalt lesen müssen. Ein Benutzer mit Bildschirm Leseberechtigung muss über dieselbe abkürzen verfügen. Mit den Überschriften und Überschriften werden Abschnitte einer Benutzeroberfläche definiert, die eine effizientere Navigation für Benutzer von Hilfstechnologien (at) unterstützen. Wenn Sie den Inhalt in die Bereiche "Markierungen" und "Überschriften" markieren, bietet ein Bildschirm Lese Benutzer die Möglichkeit, den Inhalt zu erhalten, ähnlich wie bei einem sehbaren Benutzer.

Die Konzepte von [Aria](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page)-, [Aria](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html)-und HTML-über [Schriften](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) wurden seit Jahren in Webinhalten verwendet, um eine schnellere Navigation durch Benutzer von Bildschirmlesern zu ermöglichen. Webseiten verwenden die Verwendung von Marken und Überschriften, um ihren Inhalt besser verwendbar zu machen, indem Sie es dem Benutzer ermöglichen, schnell zum großen Block ("Landmark") und einem kleineren Block (Überschrift) zu gelangen. Sprachausgaben verfügen insbesondere über Befehle, die es Benutzern ermöglichen, Zwischenstand Orten zu springen und zwischen Überschriften zu springen (Next/Previous oder bestimmte Überschrift Ebene). Aus den gleichen Gründen ist es wichtig, dass Sie Überschriften und Überschriften in ihren apps in Erwägung gezogen werden.

Mit-Funktionen können Inhalte in verschiedene Kategorien unterteilt werden, z. b. Suche, Navigation, Hauptinhalte usw. Sobald der Benutzer gruppiert ist, kann er schnell zwischen den Gruppen navigieren. Diese schnelle Navigation ermöglicht es dem Benutzer, potenziell beträchtliche Mengen an Inhalten zu überspringen, die zuvor möglicherweise durch ein Element durch ein Element durch die Verwendung von Elementen navigiert wurden.

Wenn Sie z. b. einen Registerkarten Bereich verwenden, sehen Sie sich dieses als Navigations Ende an. Wenn Sie ein suchbearbeitungsfeld verwenden, sehen Sie sich dies als Suchbegriff an, und legen Sie fest, dass Sie den Hauptinhalt als Hauptinhalts-Wahrzeichen Unabhängig davon, ob Sie sich in einem-oder sogar außerhalb eines-Elements befinden, sollten Sie untergeordnete Elemente als Überschriften mit logischen Überschriften

Sehen Sie sich die Seite **Easy of Access** in der Windows-Einstellungs-APP an.

![Seite "Easy of Access" in der Windows-Einstellungs-APP](images/EaseOfAccessSettings.png)  

Es gibt ein suchbearbeitungsfeld, das in einem Such-Landmark umschließt ist. Die Navigationselemente auf der linken Seite werden in einem Navigationsbereich umschließt, und der Hauptinhalt auf der rechten Seite wird in ein Hauptinhalts-Zeichenbereich umschließt. In diesem Fall gibt es im Navigationsbereich eine Hauptgruppen Überschrift mit dem Namen " **Easy of Access** ", die eine Überschrift Ebene 1 ist. Darunter sind die unter Optionen **Vison**, **Hearing**, usw. Diese haben eine Überschrift Ebene 2. Das Festlegen von Überschriften wird im Hauptinhalt wieder fortgesetzt, wobei der Haupt Betreff, die **Anzeige**, die Überschrift Ebene 1 und Untergruppen, wie z. b. **alles größere** als Überschrift Ebene 2.

Der Zugriff auf die Einstellungs-APP wäre ohne Überschriften und Überschriften möglich, Sie ist jedoch besser verwendbar. Ein Benutzer mit Bildschirm Leseberechtigung kann schnell und einfach die gewünschte Gruppe (das gewünschte Bildschirm) erreichen und dann schnell zur Untergruppe (Überschrift) gelangen.

Verwenden Sie [AutomationProperties. landmarktypeproperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) , um das UI-Element als den gewünschten [Typ von "Landmark](https://docs.microsoft.com/windows/desktop/WinAuto/landmark-type-identifiers) " einzurichten. Dieses Benutzeroberflächen Element von Landmark würde alle anderen Benutzeroberflächen Elemente kapseln, die für dieses Wahrzeichen sinnvoll sind.

Verwenden Sie [AutomationProperties. localizedlandmarktypeproperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) , um das-Attribut explizit zu benennen. Wenn Sie einen vordefinierten Typ für den-Typ auswählen, z. b. Main oder Navigational, werden diese Namen für den Namen des wegzeichens verwendet. Wenn Sie jedoch den Typ "Landmark" auf "Custom" festlegen, müssen Sie das-Zeichen aus dieser Eigenschaft explizit benennen. Sie können diese Eigenschaft auch verwenden, um die Standardnamen der Nichtbenutzer definierten Typen zu überschreiben.

Verwenden Sie [AutomationProperties. headinglevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) , um das UI-Element als Überschrift einer bestimmten Ebene von *Level1* bis *Level9*festzulegen.

## <a name="examples"></a>Beispiele

Zahlreiche Codebeispiele, die veranschaulichen, wie viele häufige Probleme mit der programmgesteuerten Barrierefreiheit in Windows-Desktop-Apps gelöst werden können, finden Sie unter [Codebeispiele zum Beheben allgemeiner Probleme mit der programmgesteuerten Barrierefreiheit in Windows](https://docs.microsoft.com/accessibility-tools-docs/)

Auf diese Codebeispiele wird direkt von[ Microsoft-Barrierefreiheits Informationen für Windows](https://github.com/microsoft/accessibility-insights-windows)verwiesen, was Ihnen helfen kann, viele Barrierefreiheits Probleme in der Benutzeroberfläche zu beleuchten.
