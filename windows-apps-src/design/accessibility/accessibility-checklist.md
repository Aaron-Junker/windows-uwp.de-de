---
description: Bietet eine Prüfliste, um sicherzustellen, dass die Windows-App zugänglich ist.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Prüfliste für die Barrierefreiheit
label: Accessibility checklist
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fd16c93b3914987741a486e4f40d4b60e0b274ee
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032713"
---
# <a name="accessibility-checklist"></a>Prüfliste für die Barrierefreiheit

Bietet eine Prüfliste, um sicherzustellen, dass die Windows-App zugänglich ist.

Hier finden Sie eine Prüfliste, die Sie verwenden können, um den Zugriff auf Ihre App sicherzustellen.

1. Legen Sie den Namen (erforderlich) und die Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App fest.

    Der Name zur Verwendung durch Screenreader-Software ist eine kurze, beschreibende Textzeichenfolge, mit der die Sprachausgabe ein UI-Element ansagt. Einige Elemente der Benutzeroberfläche, z. b. [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) und [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) , Stufen Ihren Text Inhalt als den standardmäßigen zugänglichen Namen herauf. siehe [grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md#name_from_inner_text).

    Für Bilder oder andere Steuerelemente, bei denen der innere Text nicht als impliziter Name zur Verwendung durch Screenreader-Software verwendet werden kann, muss der Name explizit festgelegt werden. Verwenden Sie Bezeichnungen für Formularelemente, damit der Bezeichnungstext als [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95))-Ziel im Microsoft-Benutzeroberflächenautomatisierungs-Modell zum Korrelieren von Bezeichnungen und Eingaben verwendet werden kann. Wenn Sie mehr Informationen und Anweisungen zur Benutzeroberfläche für Benutzer bereitstellen möchten als normalerweise im Namen für Bildschirmleseprogramme enthalten sind, können Sie Beschreibungen und QuickInfos für Bildschirmleseprogramme implementieren.

    Weitere Informationen finden Sie unter [barrierefreie Namen](basic-accessibility-information.md#accessible_name) und [barrierefreie Beschreibungen](basic-accessibility-information.md).

2. Implementieren Sie Barrierefreiheit für den Tastaturzugriff:

    * Testen Sie die standardmäßige Aktivierreihenfolge (Tabindex) für eine Benutzeroberfläche. Passen Sie die Aktivierreihenfolge ggf. an. Dazu müssen Sie möglicherweise bestimmte Steuerelemente aktivieren oder deaktivieren oder die Standardwerte von [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) für einige UI-Elemente ändern.
    * Verwenden Sie Steuerelemente, die eine Navigation mit Pfeiltasten für zusammengesetzte Elemente unterstützen. Für standardmäßige Steuerelemente ist die Navigation mit Pfeiltasten normalerweise bereits implementiert.
    * Verwenden Sie Steuerelemente, die die Tastaturaktivierung unterstützen. Für standardmäßige Steuerelemente (insbesondere diejenigen, die das [**Invoke**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider)-Muster der Benutzeroberflächenautomatisierung unterstützen) ist die Tastaturaktivierung normalerweise verfügbar. Hinweise dazu finden Sie in der Dokumentation der jeweiligen Steuerelemente.
    * Implementieren Sie Tastenkombinationen für bestimmte Teile der Benutzeroberfläche, die Interaktion unterstützen.
    * Überprüfen Sie für alle benutzerdefinierten Steuerelemente der Benutzeroberfläche, ob Sie sie mit der entsprechenden [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)-Unterstützung für die Aktivierung implementiert haben. Stellen Sie auch sicher, dass Sie die notwendigen Überschreibungen für die Tastenbehandlung definiert haben, um Aktivieren, Durchlaufen und Auswählen oder Tastenkombinationen zu unterstützen.

    Weitere Informationen finden Sie unter [Tastatur Interaktionen](../input/keyboard-interactions.md).

3. Sicherstellen, dass Text eine lesbare Größe ist

    * Windows umfasst verschiedene Tools und Einstellungen für die Barrierefreiheit, die Benutzer nutzen und ihren eigenen Anforderungen und Vorlieben zum Lesen von Text anpassen können. Dazu gehören:
        * Das Bildschirm Vergrößerungs Tool, das einen ausgewählten Bereich der Benutzeroberfläche vergrößert. Sie sollten sicherstellen, dass das Layout des Texts in der APP die Verwendung der Bildschirmlupe nicht erschwert.
        * Globale Skalierungs-und Auflösungseinstellungen in **Einstellungen->System >Anzeige >Skalierung und Layout** . Welche Größen Anpassungsoptionen verfügbar sind, kann variieren, je nachdem, welche Möglichkeiten das Anzeigegerät hat.
        * Einstellungen für die Textgröße in den **Einstellungen->einfache Zugriffs >Anzeige** . Passen Sie die Einstellung **Text vergrößern** an, um nur die Textgröße in unterstützenden Steuerelementen für alle Anwendungen und Bildschirme anzugeben (alle UWP-Text Steuerelemente unterstützen die Text Skalierung ohne Anpassung oder Vorlagen).
        > [!NOTE]
        > Mit der Einstellung **alles erstellen** kann ein Benutzer seine bevorzugte Größe für Text und apps im Allgemeinen nur auf dem primären Bildschirm angeben.

4. Schauen Sie sich die Benutzeroberfläche an, um sicherzustellen, dass der Textkontrast ausreicht, Elemente in Designs mit hohem Kontrast richtig dargestellt werden und Farben korrekt verwendet werden.

    * Stellen Sie mithilfe eines Farbanalysetools sicher, dass das Textkontrastverhältnis mindestens 4,5:1 beträgt.
    * Wechseln Sie zu einem Design mit hohem Kontrast, und überprüfen Sie, ob die Benutzeroberfläche Ihrer App leserlich ist und verwendet werden kann.
    * Stellen Sie sicher, dass die Benutzeroberfläche Informationen nicht nur mithilfe von Farben vermittelt.

    Weitere Informationen finden Sie unter Designs mit [hohem Kontrast](high-contrast-themes.md) und [barrierefreie textanforderungen](accessible-text-requirements.md).

5. Führen Sie Tools zum Testen der Barrierefreiheit aus. Behandeln Sie gemeldete Probleme und überprüfen Sie die Qualität der Sprachausgabe.

    Überprüfen Sie mithilfe von Tools wie [**Inspect**](/windows/desktop/WinAuto/inspect-objects) den programmgesteuerten Zugriff, führen Sie Diagnosetools wie [**AccChecker**](/windows/desktop/WinAuto/ui-accessibility-checker) aus, um allgemeine Fehler zu ermitteln, und überprüfen Sie die Qualität der Sprachausgabe.

    Weitere Informationen finden Sie unter [Barrierefreiheits Tests](accessibility-testing.md).

6. Stellen Sie sicher, dass die Einstellungen für das App-Manifest den Richtlinien für Barrierefreiheit entsprechen.

7. Deklarieren Sie Ihre APP im Microsoft Store als verfügbar.

    Wenn Sie die Unterstützung für die grundlegende Barrierefreiheit implementiert haben, können Sie Ihre APP im Microsoft Store als verfügbar deklarieren, um mehr Kunden zu erreichen und einige zusätzliche gute Bewertungen zu erhalten.

    Weitere Informationen finden Sie unter [Barrierefreiheit im Store](accessibility-in-the-store.md).

## <a name="related-topics"></a>Verwandte Themen  

* [Anforderungen für barrierefreien Text](accessible-text-requirements.md)
* [Textskalierung](../input/text-scaling.md)
* [Bedienungshilfen](accessibility.md)
* [Entwerfen für Barrierefreiheit](./accessibility-overview.md)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)
