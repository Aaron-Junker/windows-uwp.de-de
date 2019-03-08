---
Description: Bietet eine Prüfliste, mit der Sie sicherstellen können, dass Ihre App für die Universelle Windows-Plattform (UWP) barrierefrei ist.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Prüfliste für die Barrierefreiheit
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c9ff9760b3ae9b852fe1ae1b86d1cc48e49c5dd4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602445"
---
# <a name="accessibility-checklist"></a>Prüfliste für die Barrierefreiheit

Bietet eine Prüfliste, mit der Sie sicherstellen können, dass Ihre App für die Universelle Windows-Plattform (UWP) barrierefrei ist.

Hier finden Sie eine Prüfliste, die Sie verwenden können, um den Zugriff auf Ihre App sicherzustellen.

1. Legen Sie den Namen (erforderlich) und die Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App fest.

    Der Name zur Verwendung durch Screenreader-Software ist eine kurze, beschreibende Textzeichenfolge, mit der die Sprachausgabe ein UI-Element ansagt. Einige UI-Elemente wie [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) und [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) unterstützen ihren Textinhalt als standardmäßigen Namen für Bildschirmleseprogramme (siehe [Grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md#name_from_inner_text)).

    Für Bilder oder andere Steuerelemente, bei denen der innere Text nicht als impliziter Name zur Verwendung durch Screenreader-Software verwendet werden kann, muss der Name explizit festgelegt werden. Verwenden Sie Bezeichnungen für Formularelemente, damit der Bezeichnungstext als [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769)-Ziel im Microsoft-Benutzeroberflächenautomatisierungs-Modell zum Korrelieren von Bezeichnungen und Eingaben verwendet werden kann. Wenn Sie mehr Informationen und Anweisungen zur Benutzeroberfläche für Benutzer bereitstellen möchten als normalerweise im Namen für Bildschirmleseprogramme enthalten sind, können Sie Beschreibungen und QuickInfos für Bildschirmleseprogramme implementieren.

    Weitere Informationen finden Sie unter [Name zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md#accessible_name) und [Beschreibung zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md).

2. Implementieren Sie Barrierefreiheit für den Tastaturzugriff:

    * Testen Sie die standardmäßige Aktivierreihenfolge (Tabindex) für eine Benutzeroberfläche. Passen Sie die Aktivierreihenfolge ggf. an. Dazu müssen Sie möglicherweise bestimmte Steuerelemente aktivieren oder deaktivieren oder die Standardwerte von [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) für einige UI-Elemente ändern.
    * Verwenden Sie Steuerelemente, die eine Navigation mit Pfeiltasten für zusammengesetzte Elemente unterstützen. Für standardmäßige Steuerelemente ist die Navigation mit Pfeiltasten normalerweise bereits implementiert.
    * Verwenden Sie Steuerelemente, die die Tastaturaktivierung unterstützen. Für standardmäßige Steuerelemente (insbesondere diejenigen, die das [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582)-Muster der Benutzeroberflächenautomatisierung unterstützen) ist die Tastaturaktivierung normalerweise verfügbar. Hinweise dazu finden Sie in der Dokumentation der jeweiligen Steuerelemente.
    * Implementieren Sie Tastenkombinationen für bestimmte Teile der Benutzeroberfläche, die Interaktion unterstützen.
    * Überprüfen Sie für alle benutzerdefinierten Steuerelemente der Benutzeroberfläche, ob Sie sie mit der entsprechenden [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Unterstützung für die Aktivierung implementiert haben. Stellen Sie auch sicher, dass Sie die notwendigen Überschreibungen für die Tastenbehandlung definiert haben, um Aktivieren, Durchlaufen und Auswählen oder Tastenkombinationen zu unterstützen.

    Weitere Informationen finden Sie unter [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3. Stellen Sie sicher, dass Text um eine lesbare Größe beträgt

    * Windows umfasst verschiedene Tools für Barrierefreiheit und Einstellungen, die Benutzer nutzen können, und passen Sie ihre eigenen Anforderungen und die Einstellungen für das Lesen von Text. Dazu gehören:
        * Das Bildschirmlupe-Tool, das die Benutzeroberfläche ein ausgewählten Bereichs vergrößert. Sie sollten sicherstellen, dass das Layout von Text in Ihrer app verwenden Sie Bildschirmlupe zum Lesen erschweren nicht.
        * Globale Skalierung und Auflösung von Einstellungen in **Einstellungen -> System -> Anzeige & gt; Skalierung und Layout**. Genau die Größenanpassungsoptionen zur verfügbar sind, können variieren, wie die Funktionen des Anzeigegeräts sowie.
        * Die Einstellungen für Text in **erleichterte Bedienung "Einstellungen ->" -> Anzeige**. Anpassen der **vergrößern Text** Einstellung, um nur die Größe des Texts in die Steuerelemente für alle Anwendungen und Bildschirme (alle UWP-Text-Steuerelemente unterstützen den Text, der Skalierung ohne Anpassung oder Vorlagen) unterstützen.
        > [!NOTE]
        > Die **korrektes größere** Einstellung ermöglicht es einem Benutzer in der Regel geben Sie ihre bevorzugte Größe für Text- und apps auf ihre primären Bildschirm nur.

4. Schauen Sie sich die Benutzeroberfläche an, um sicherzustellen, dass der Textkontrast ausreicht, Elemente in Designs mit hohem Kontrast richtig dargestellt werden und Farben korrekt verwendet werden.

    * Stellen Sie mithilfe eines Farbanalysetools sicher, dass das Textkontrastverhältnis mindestens 4,5:1 beträgt.
    * Wechseln Sie zu einem Design mit hohem Kontrast, und überprüfen Sie, ob die Benutzeroberfläche Ihrer App leserlich ist und verwendet werden kann.
    * Stellen Sie sicher, dass die Benutzeroberfläche Informationen nicht nur mithilfe von Farben vermittelt.

    Weitere Informationen finden Sie unter [Designs mit hohem Kontrast](high-contrast-themes.md) und [Anforderungen für barrierefreien Text](accessible-text-requirements.md).

5. Führen Sie Tools zum Testen der Barrierefreiheit aus. Behandeln Sie gemeldete Probleme und überprüfen Sie die Qualität der Sprachausgabe.

    Überprüfen Sie mithilfe von Tools wie [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) den programmgesteuerten Zugriff, führen Sie Diagnosetools wie [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) aus, um allgemeine Fehler zu ermitteln, und überprüfen Sie die Qualität der Sprachausgabe.

    Weitere Informationen finden Sie unter [Barrierefreiheitstests](accessibility-testing.md).

6. Stellen Sie sicher, dass die Einstellungen für das App-Manifest den Richtlinien für Barrierefreiheit entsprechen.

7. Deklarieren Sie Ihre App im Microsoft Store als barrierefrei.

    Wenn Sie die grundlegende Unterstützung für Barrierefreiheit implementiert haben, können Sie durch das Kennzeichnen Ihrer App im Microsoft Store mehr Kunden erreichen und zusätzliche gute Bewertungen erhalten.

    Weitere Informationen finden Sie unter [Barrierefreiheit im Store](accessibility-in-the-store.md).

## <a name="related-topics"></a>Verwandte Themen  

* [Anforderungen für barrierefreien Text](accessible-text-requirements.md)
* [Skalieren von Text](../input/text-scaling.md)
* [Bedienungshilfen](accessibility.md)
* [Entwerfen für Barrierefreiheit](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)
