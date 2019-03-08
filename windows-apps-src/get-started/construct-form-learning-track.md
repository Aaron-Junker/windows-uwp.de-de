---
title: Lernpfad – Erstellen und Konfigurieren eines Formulars
description: Erfahren Sie, was Sie tun müssen, um ein robustes Formular in Ihrer App zu erstellen.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Layout, Formular
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 45c4acda953808a85363d1cec7c623dd79cc08f1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656445"
---
# <a name="create-and-customize-a-form"></a>Erstellen und Anpassen eines Formulars

Wenn Sie eine app erstellen, die Benutzer eine erhebliche Menge an Informationen eingeben müssen, ist es wahrscheinlich, dass Sie ein Formular zu füllen Sie erstellen möchten. In diesem Artikel zeigt Ihnen, was müssen Sie wissen, um ein Formular erstellen, die nützlich und stabiler ist.

Dies ist kein Lernprogramm. Wenn Sie ein Lernprogramm suchen, sehen Sie sich unser [Lernprogramm zum adaptiven Layout](../design/basics/xaml-basics-adaptive-layout.md) an, in dem Sie schrittweise Anleitungen finden.

Wir erläutern, welche **XAML-Steuerelemente** in ein Formular gehören, wie diese am besten auf der Seite angeordnet werden, und wie Sie Ihrs Formular für sich ändernde Bildschirmgrößen optimieren. Aber da es in einem Formular um die relative Position visueller Elemente geht, lassen Sie uns zunächst das Seitenlayout mit XAML besprechen.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

UWP verfügt nicht über ein explizites Formularsteuerelement, das Sie Ihrer App hinzufügen und dann konfigurieren können. Stattdessen müssen Sie ein Formular erstellen, indem Sie eine Sammlung von Benutzeroberflächenelementen auf einer Seite anordnen.

Dafür müssen Sie **Layoutpanels** verstehen. Dabei handelt es sich um Container, die die Benutzeroberflächenelemente Ihrer App enthalten, die Sie anordnen und gruppieren können. Durch die Platzierung von Layoutpanels in andere Layoutpanels erhalten Sie ein hohes Maß an Kontrolle darüber, wo und wie Ihre Elemente im Verhältnis zueinander angezeigt werden. Damit wird es auch wesentlich einfacher, Ihre App an sich ändernde Bildschirmgrößen anzupassen.

Lesen Sie [diese Dokumentation zu Layoutpanels](../design/layout/layout-panels.md). Da Formulare in der Regel in einer oder mehreren vertikalen Spalten angezeigt werden, sollten Sie ähnliche Elemente in einem **StackPanel** gruppieren und diese bei Bedarf in einem **RelativePanel** anordnen. Beginnen Sie jetzt damit, einige Panels zusammenzustellen – wenn Sie eine Referenz benötigen, finden Sie unten ein grundlegendes Layout-Framework für ein zweispaltiges Formular:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>Was gehört in ein Formular?

Sie müssen Ihr Formular mit verschiedenen [XAML-Steuerelementen](../design/controls-and-patterns/controls-and-events-intro.md) füllen. Wahrscheinlich sind Sie mit diesen vertraut, lesen Sie jedoch gerne weiter, wenn Sie eine Auffrischung benötigen. Sie benötigen insbesondere Steuerelemente, mit denen Benutzer Text eingeben oder Optionen aus einer Liste von Werten auswählen können. Dies ist eine einfache Liste der Optionen, die Sie hinzufügen können – Sie müssen nicht alles über die sie genug, damit Sie verstehen, wie sie Aussehen und ihre Funktionsweise zu lesen.

* [Textfeld](../design/controls-and-patterns/text-box.md) können Sie ein Eingabetext für Benutzer in Ihrer app.
* Mit [ToggleSwitch](../design/controls-and-patterns/toggles.md) kann ein Benutzer zwischen zwei Optionen auswählen.
* Mit [DatePicker](../design/controls-and-patterns/date-picker.md) kann ein Benutzer einen Datumswert auswählen.
* Mit [TimePicker](../design/controls-and-patterns/time-picker.md) kann ein Benutzer einen Zeitwert auswählen.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) ermöglicht eine Erweiterung, um eine Liste mit auswählbaren Elementen anzuzeigen. Weitere Informationen dazu erhalten Sie [hier](../design/controls-and-patterns/lists.md#drop-down-lists).

Möglicherweise sollten Sie auch [Schaltflächen](../design/controls-and-patterns/buttons.md) hinzufügen, damit der Benutzer speichern oder abbrechen kann.

## <a name="format-controls-in-your-layout"></a>Formatieren von Steuerelementen in Ihrem Layout

Sie wissen jetzt, wie Sie Layoutpanels anordnen und gewünschte Elemente hinzufügen können, aber wie sollten diese formatiert werden? Die Seite [Formulare](../design/controls-and-patterns/forms.md) bietet einige spezifische Designleitfäden. Nützliche Ratschläge finden Sie in den Abschnitten **Formulartypen** und **Layout**. Wir werden später noch näher auf Bedienungshilfen und das relative Layout eingehen.

Mit diesem Ratschlag im Hinterkopf sollten Sie beginnen, Steuerelemente Ihrer Wahl zu Ihrem Layout hinzuzufügen. Stellen Sie dabei sicher, dass sie Beschriftungen erhalten und ordnungsgemäß verteilt sind. Als Beispiel sehen Sie hier einen minimalen Entwurf für ein einseitiges Formular mithilfe der oben genannten Anweisungen für Layout, Steuerelemente und Design:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

Für eine bessere visuelle Erfahrung können Sie die Steuerelemente jederzeit mit weiteren Eigenschaften anpassen.

## <a name="make-your-layout-responsive"></a>Entwerfen von dynamischen Layouts

Benutzer zeigen Ihre Benutzeroberfläche möglicherweise auf verschiedenen Geräten mit unterschiedlichen Bildschirmbreiten an. Um ein hohes Maß an Benutzerfreundlichkeit unabhängig vom jeweiligen Bildschirm sicherzustellen, sollten Sie ein [dynamisches Design](../design/layout/responsive-design.md) verwenden. Lesen Sie die Seite, um einige gute Ratschläge zu den Designphilosophien zu erhalten, die Sie bei Ihren weiteren Schritten berücksichtigen sollten.

Auf der Seite [Dynamische Layouts mit XAML](../design/layout/layouts-with-xaml.md) finden Sie eine ausführliche Übersicht mit Informationen zur Implementierung. Für den Moment konzentrieren uns auf **dynamische Layouts** und **visuelle Zustände in XAML**.

Der grundlegende Formularentwurf, den wir zusammengestellt haben, ist bereits ein **dynamisches Layout**, da es hauptsächlich von der relativen Position der Steuerelemente mit nur minimaler Nutzung von bestimmten Pixelgrößen und Positionen abhängig ist. Berücksichtigen Sie diese Anweisung jedoch für weitere Benutzeroberflächen, die Sie möglicherweise in Zukunft erstellen.

Wichtiger für dynamische Layouts sind **visuelle Zustände.** Ein visueller Zustand definiert Eigenschaftswerte, die auf ein bestimmtes Element angewendet werden, wenn eine bestimmte Bedingung zutrifft. [Lesen Sie mehr zur Vorgehensweise in XAML](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup), und implementieren Sie diese dann in Ihrem Formular. Hier ist gezeigt, wie ein *sehr* grundlegender in unserem vorherigen Beispiel aussehen könnte:

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

Es ist nicht sinnvoll, visuelle Zustände für eine Vielzahl von Bildschirmgrößen zu erstellen. Außerdem haben mehr als einige wahrscheinlich keine bedeutende Auswirkung auf die Benutzerfreundlichkeit Ihrer App. Wir empfehlen, stattdessen für einige Schlüsselgrößen, sogenannte Breakpoints, zu entwickeln, über die Sie [hier mehr erfahren](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Hinzufügen von Unterstützung für Bedienungshilfen

Nachdem Sie jetzt ein gut konstruiertes Layout zusammengestellt haben, das auf Änderungen der Bildschirmgrößen reagiert, können Sie die Benutzerfreundlichkeit in einem letzten Schritt weiter verbessern, indem Sie [Ihrer App Bedienungshilfen hinzufügen](../design/accessibility/accessibility-overview.md). Das kann eine Menge Arbeit bedeuten, aber in einem Formular wie diesem ist es einfacher, als es zunächst scheint. Konzentrieren Sie sich auf Folgendes:

* Tastaturunterstützung: Stellen Sie sicher, dass die Reihenfolge der Elemente in Ihren Benutzeroberflächenbereichen damit übereinstimmt, wie sie auf dem Bildschirm angezeigt werden, damit Benutzer auf einfache Weise mit der TAB-Taste navigieren können.
* Unterstützung für die Sprachausgabe: Stellen Sie sicher, dass alle Steuerelemente über einen beschreibenden Namen verfügen.

Wenn Sie komplexere Layouts mit mehr visuellen Elementen erstellen, finden Sie weitere Details in der [Prüfliste für die Barrierefreiheit](../design/accessibility/accessibility-checklist.md). Bedienungshilfe sind zwar für eine App nicht notwendig, tragen aber dazu bei, eine größere Zielgruppe zu erreichen und einzubinden.

## <a name="going-further"></a>Vertiefung

Auch wenn Sie hier ein Formular erstellt haben, gelten die Konzepte von Layouts und Steuerelementen für alle XAML-Benutzeroberflächen, die Sie möglicherweise erstellen. Die Dokumentation zurückkehren, wir haben Sie verknüpft, und experimentieren Sie mit dem Formular hinzufügen von neuen Funktionen der Benutzeroberfläche und die benutzerfreundlichkeit weiter zu optimieren und welche, können. Wenn Sie eine schrittweise Anleitung durch ausführlichere Layoutfunktionen möchten, finden Sie unter unserem [adaptive Layout-Tutorial](../design/basics/xaml-basics-adaptive-layout.md)

Formulare existieren außerdem nicht in einem Vakuum – Sie können einen Schritt weiter gehen und Ihres in ein [Master-/Detailmuster](../design/controls-and-patterns/master-details.md) oder [Pivot-Steuerelement](../design/controls-and-patterns/pivot.md) einbetten. Oder wenn Sie an dem CodeBehind für Ihr Formular arbeiten möchten, finden Sie die ersten Schritte in unserer [Übersicht über Ereignisse](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung zu den APIs und weiterer nützlicher Dokumentation, die Sie bei Ihren ersten Schritten rund um die Datenbindung unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [Steuerelemente, die nützlich für Formulare](../design/controls-and-patterns/forms.md#input-controls) | Eine Liste nützlicher Eingabesteuerelemente für das Erstellen von Formularen und ein allgemeiner Überblick zu ihrer Verwendung |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | Ein Panel zum Anordnen von Elementen in Layouts mit mehreren Zeilen und Spalten |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Ein Panel zum Anordnen von Elementen in Bezug auf andere Elemente und die Grenzen des Panels |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Ein Panel zum Anordnen von Elementen in einer einzigen horizontalen oder vertikalen Linie |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | Ermöglicht die Festlegung des Erscheinungsbilds von Benutzeroberflächenelementen, wenn sich diese in bestimmten Zuständen befinden |

### <a name="useful-docs"></a>Nützliche Dokumente

| Thema | Beschreibung |
|-------|----------------|
| [Barrierefreiheit im Überblick](../design/accessibility/accessibility-overview.md) | Eine umfassende Übersicht über Optionen für die Barrierefreiheit in Apps |
| [Checkliste für die Barrierefreiheit](../design/accessibility/accessibility-checklist.md) | Eine praktische Prüfliste, um sicherzustellen, dass Ihre App Standards für die Barrierefreiheit erfüllt |
| [Übersicht über Ereignisse](../xaml-platform/events-and-routed-events-overview.md) | Details zum Hinzufügen und Strukturieren von Ereignissen zur Verarbeitung von Benutzeroberflächenaktionen |
| [Formulare](../design/controls-and-patterns/forms.md) | Allgemeine Anweisungen zum Erstellen von Formularen |
| [Layoutpanels](../design/layout/layout-panels.md) | Übersicht über die Arten von Layoutpanels und ihre Verwendung |
| [Master-/Detail-Musters](../design/controls-and-patterns/master-details.md) | Ein Entwurfsmuster, das rund um ein oder mehrere Formulare implementiert werden kann |
| [Pivot-Steuerelement](../design/controls-and-patterns/pivot.md) | Ein Steuerelement, das ein oder mehrere Formulare enthalten kann |
| [Reaktionsfähiges design](../design/layout/responsive-design.md) | Eine Übersicht über Prinzipien für das umfangreiche dynamische Design | 
| [Reaktionsfähige Layouts mit XAML](../design/layout/layouts-with-xaml.md) | Spezifische Informationen zu visuellen Zuständen und anderen Implementierungen des dynamischen Designs |
| [Bildschirmgrößen für reaktionsfähiges design](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Anweisungen, welche Bildschirmgrößen für dynamische Layouts festgelegt werden sollten |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Adaptive Layout-tutorial](../design/basics/xaml-basics-adaptive-layout.md) | Eine schrittweise geführte Anleitung zu adaptiven Layouts und zum dynamischen Design | 
| [Customer-Orders-Datenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Layout und Formulare in Aktion in einem mehrseitigen Unternehmensbeispiel |
| [XAML-Steuerelementsammlungen](https://github.com/Microsoft/Xaml-Controls-Gallery) | Eine Auswahl von XAML-Steuerelementen und Informationen zu deren Implementierung |
| [Weitere Codebeispiele](https://developer.microsoft.com//windows/samples) | Wählen Sie **Steuerelemente, Layout und Text** in der Dropdownliste „Kategorie“ aus, um verwandte Codebeispiele anzuzeigen. |
