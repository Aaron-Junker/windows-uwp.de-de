---
title: Lernpfad – Erstellen und Konfigurieren eines Formulars
description: Erfahren Sie, wie Sie ein robustes Formular in einer UWP-App erstellen und konfigurieren, um die Eingabe einer beträchtlichen Menge von Informationen zu verarbeiten.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Layout, Formular
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a674514bfeb2acbc545e59cf1b3fc6e59d697215
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304522"
---
# <a name="create-and-customize-a-form"></a>Erstellen und Anpassen eines Formulars

Wenn Sie eine App erstellen, die umfangreiche Informationen von Benutzern abfragt, werden Sie wahrscheinlich ein Formular erstellen, das sich einfach ausfüllen lässt. In diesem Artikel erfahren Sie, wie Sie ein nützliches und stabiles Formular erstellen.

Dieser Artikel ist kein Tutorial. Wenn Sie ein Tutorial suchen, sehen Sie sich unser [Tutorial zu adaptiven Layouts](../design/basics/xaml-basics-adaptive-layout.md) an, in dem Sie schrittweise Anleitungen finden.

Wir erläutern, welche **XAML-Steuerelemente** in ein Formular gehören, wie diese am besten auf der Seite angeordnet werden und wie Sie Ihr Formular optimal an unterschiedliche Bildschirmgrößen anpassen. Da es in einem Formular jedoch um die relative Position visueller Elemente geht, lassen Sie uns zunächst das Seitenlayout mit XAML besprechen.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

UWP weist kein explizites Formularsteuerelement auf, das Sie Ihrer App hinzufügen und dann konfigurieren können. Stattdessen müssen Sie ein Formular erstellen, indem Sie eine Reihe von Benutzeroberflächenelementen auf einer Seite anordnen.

Dazu sollten Ihnen **Layoutpanels** geläufig sein. Hierbei handelt es sich um Container, die die Benutzeroberflächenelemente Ihrer App enthalten, die Sie anordnen und gruppieren können. Durch das Einfügen von Layoutpanels in andere Layoutpanels können Sie sehr gut kontrollieren, wo und wie Elemente im Verhältnis zueinander angezeigt werden. Damit wird es auch wesentlich einfacher, Ihre App an unterschiedliche Bildschirmgrößen anzupassen.

Lesen Sie [diese Dokumentation zu Layoutpanels](../design/layout/layout-panels.md). Da Formulare in der Regel in einer oder mehreren vertikalen Spalten angezeigt werden, sollten Sie ähnliche Elemente in einem **StackPanel** gruppieren und diese bei Bedarf in einem **RelativePanel** anordnen. Beginnen Sie jetzt mit der Zusammenstellung einiger Panels. Falls Sie eine Referenz benötigen, finden Sie unten ein grundlegendes Layout-Framework für ein zweispaltiges Formular:

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

Sie müssen Ihr Formular mit verschiedenen [XAML-Steuerelementen](../design/controls-and-patterns/controls-and-events-intro.md) füllen. Wahrscheinlich sind Sie damit vertraut. Sie können jedoch gerne weiterlesen, wenn Sie eine Auffrischung gebrauchen können. Sie benötigen insbesondere Steuerelemente, mit denen Benutzer Text eingeben oder Werte aus einer Liste auswählen können. Hier ist eine grundlegende Liste von Optionen, die Sie hinzufügen können – Sie müssen nicht alles darüber lesen, nur so viel, dass Sie verstehen, wie sie aussehen und funktionieren.

* Mit [TextBox](../design/controls-and-patterns/text-box.md) kann ein Benutzer Text in Ihre App eingeben.
* Mit [ToggleSwitch](../design/controls-and-patterns/toggles.md) kann ein Benutzer zwischen zwei Optionen auswählen.
* Mit [DatePicker](../design/controls-and-patterns/date-picker.md) kann ein Benutzer einen Datumswert auswählen.
* Mit [TimePicker](../design/controls-and-patterns/time-picker.md) kann ein Benutzer einen Wert für die Uhrzeit auswählen.
* [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) lässt sich erweitern, um eine Liste mit auswählbaren Elementen anzuzeigen. Weitere Informationen dazu erhalten Sie [hier](../design/controls-and-patterns/combo-box.md).

Sie können auch [Schaltflächen](../design/controls-and-patterns/buttons.md) zum Speichern oder Abbrechen hinzufügen.

## <a name="format-controls-in-your-layout"></a>Formatieren von Steuerelementen im Layout

Sie wissen, wie Sie Layoutpanels anordnen und gewünschte Elemente hinzufügen können, fragen sich aber jetzt, wie diese formatiert werden? Die Seite [Formulare](../design/controls-and-patterns/forms.md) bietet einige spezifische Designleitfäden. Hilfreiche Tipps finden Sie in den Abschnitten **Formulartypen** und **Layout**. In Kürze gehen wir auf Barrierefreiheit und das relative Layout ein.

Vor diesem Hintergrund sollten Sie beginnen, die gewünschten Steuerelemente in das Layout einzufügen. Achten Sie darauf, sie mit geeigneten Bezeichnungen zu versehen und im richtigen Abstand anzuordnen. Hier sehen Sie ein einfach strukturiertes Beispiel für ein einseitiges Formular, in dem das Layout, die Steuerelemente und die Designleitfäden berücksichtigt wurden, die oben genannt sind:

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

Um die visuelle Erfahrung zu verbessern, können Sie die Steuerelemente jederzeit mit weiteren Eigenschaften anpassen.

## <a name="make-your-layout-responsive"></a>Entwerfen von dynamischen Layouts

Benutzer zeigen die App-Oberfläche möglicherweise auf verschiedenen Geräten mit unterschiedlichen Bildschirmbreiten an. Um unabhängig vom jeweiligen Bildschirm eine benutzerfreundliche Anzeige sicherzustellen, sollten Sie ein [dynamisches Design](../design/layout/responsive-design.md) verwenden. Auf dieser Seite finden Sie wertvolle Designphilosophien, die Sie bei Ihren weiteren Schritten berücksichtigen sollten.

Auf der Seite [Dynamische Layouts mit XAML](../design/layout/layouts-with-xaml.md) finden Sie eine ausführliche Übersicht mit Informationen zur Implementierung. Im Augenblick konzentrieren wir uns auf **dynamische Layouts** und **visuelle Zustände in XAML**.

Der grundlegende Formularentwurf, den wir zusammengestellt haben, weist bereits ein **dynamisches Layout** auf, da er hauptsächlich von der relativen Position der Steuerelemente und nur minimal von bestimmten Pixelgrößen und Positionen abhängig ist. Berücksichtigen Sie diese Anweisung jedoch für weitere Benutzeroberflächen, die Sie möglicherweise in Zukunft entwickeln.

Wichtiger für dynamische Layouts sind **visuelle Zustände.** Ein visueller Zustand definiert Eigenschaftswerte, die auf ein bestimmtes Element angewendet werden, wenn eine bestimmte Bedingung zutrifft. [Lesen Sie mehr zur Vorgehensweise in XAML](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup), und implementieren Sie diese dann in Ihrem Formular. So könnte ein *sehr* einfacher Ansatz in unserem vorherigen Beispiel aussehen:

```xaml
<Page ...>
    <Grid>
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
            <!-- Customer StackPanel -->
            <!-- Associate StackPanel -->
            <!-- Save StackPanel -->
        </RelativePanel>
    </Grid>
</Page>
```

> [!IMPORTANT]
> Bei Verwendung von StateTrigger-Elementen müssen Sie immer sicherstellen, dass VisualStateGroups an das erste untergeordnete Element des Stammelements angefügt wird. Hier ist **Grid** das erste untergeordnete Element des **Page**-Stammelements.

Es ist nicht sinnvoll, visuelle Zustände für eine Vielzahl von Bildschirmgrößen zu erstellen. Ein paar Zustände sollten ausreichen, da sich die Benutzerfreundlichkeit Ihrer App durch viele weitere Zustände wahrscheinlich nicht deutlich beeinflussen lässt. Wir empfehlen, stattdessen Zustände für einige Schlüsselgrößen, sogenannte Breakpoints, zu entwerfen, über die Sie [hier mehr erfahren](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Unterstützung der Barrierefreiheit

Nachdem Sie jetzt über ein gut aufgebautes Layout verfügen, das auf veränderte Bildschirmgrößen reagiert, können Sie zuletzt die Benutzerfreundlichkeit verbessern, indem Sie [Ihre App barrierefrei gestalten](../design/accessibility/accessibility-overview.md). Hier gibt es zahlreiche Möglichkeiten. In einem Formular wie diesem ist es aber einfacher als gedacht. Konzentrieren Sie sich auf Folgendes:

* Tastaturunterstützung: Stellen Sie sicher, dass die Reihenfolge der Elemente in den Benutzeroberflächenpanels mit der Darstellung auf dem Bildschirm übereinstimmt, damit Benutzer mühelos durch die Elemente navigieren können.
* Unterstützung der Sprachausgabe: Vergewissern Sie sich, dass alle Steuerelemente über einen aussagekräftigen Namen verfügen.

Wenn Sie komplexere Layouts mit mehreren visuellen Elementen erstellen, finden Sie weitere Details in der [Prüfliste für die Barrierefreiheit](../design/accessibility/accessibility-checklist.md). Zwar ist Barrierefreiheit für eine App nicht unbedingt erforderlich, trägt aber dazu bei, dass Sie eine größere Zielgruppe erreichen und einbinden.

## <a name="going-further"></a>Vertiefung

Auch wenn Sie hier ein Formular erstellt haben, gelten die Layout- und Steuerelementkonzepte für alle XAML-Benutzeroberflächen, die Sie entwickeln können. Sie können jederzeit zu den hier verlinkten Dokumenten zurückkehren und mit dem vorhandenen Formular experimentieren, indem Sie neue Benutzeroberflächenfeatures hinzufügen und die Benutzererfahrung weiter optimieren. Eine schrittweise Anleitung zu detaillierteren Layoutfeatures finden Sie in unserem [Tutorial zu adaptiven Layouts](../design/basics/xaml-basics-adaptive-layout.md).

Formulare existieren außerdem nicht in einem Vakuum – Sie können einen Schritt weitergehen und Ihr Formular in ein [Master/Details-Muster](../design/controls-and-patterns/master-details.md) oder [Pivotsteuerelement](../design/controls-and-patterns/pivot.md) einbetten. Wenn Sie das CodeBehind des Formulars bearbeiten möchten, finden Sie die ersten Schritte in unserer [Übersicht über Ereignisse](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung zu den APIs und weitere nützliche Dokumente, die Sie bei Ihren ersten Schritten rund um die Datenbindung unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [Nützliche Steuerelemente für Formulare](../design/controls-and-patterns/forms.md#input-controls) | Eine Liste nützlicher Eingabesteuerelemente für das Erstellen von Formularen und ein allgemeiner Überblick zu ihrer Verwendung |
| [Grid](/uwp/api/Windows.UI.Xaml.Controls.Grid) | Ein Panel zum Anordnen von Elementen in Layouts mit mehreren Zeilen und Spalten |
| [RelativePanel](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Ein Panel zum Anordnen von Elementen im Verhältnis zu anderen Elementen und zu den Grenzen des Panels |
| [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Ein Panel zum Anordnen von Elementen in einer einzigen horizontalen oder vertikalen Linie |
| [VisualState](/uwp/api/Windows.UI.Xaml.VisualState) | Ermöglicht es Ihnen, das Erscheinungsbild von Benutzeroberflächenelementen festzulegen, wenn sie einen bestimmten Zustand aufweisen. |

### <a name="useful-docs"></a>Nützliche Dokumentation

| Thema | Beschreibung |
|-------|----------------|
| [Barrierefreiheit im Überblick](../design/accessibility/accessibility-overview.md) | Eine umfassende Übersicht über Optionen für die Barrierefreiheit in Apps |
| [Prüfliste für die Barrierefreiheit](../design/accessibility/accessibility-checklist.md) | Eine praktische Prüfliste, um sicherzustellen, dass Ihre App Standards für die Barrierefreiheit erfüllt |
| [Übersicht über Ereignisse](../xaml-platform/events-and-routed-events-overview.md) | Details zum Hinzufügen und Strukturieren von Ereignissen zur Verarbeitung von Benutzeroberflächenaktionen |
| [Formulare](../design/controls-and-patterns/forms.md) | Allgemeine Anweisungen zum Erstellen von Formularen |
| [Layoutpanels](../design/layout/layout-panels.md) | Übersicht über die Arten von Layoutpanels und ihre Verwendung |
| [Master/Details-Muster](../design/controls-and-patterns/master-details.md) | Ein Entwurfsmuster, das rund um eines oder mehrere Formulare implementiert werden kann |
| [Pivotsteuerelement](../design/controls-and-patterns/pivot.md) | Ein Steuerelement, das eines oder mehrere Formulare enthalten kann |
| [Dynamisches Design](../design/layout/responsive-design.md) | Eine Übersicht über Prinzipien für das umfangreiche dynamische Design |
| [Dynamische Layouts mit XAML](../design/layout/layouts-with-xaml.md) | Spezifische Informationen zu visuellen Zuständen und anderen Implementierungen des dynamischen Designs |
| [Bildschirmgrößen für das dynamische Design](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Anweisungen dazu, welche Bildschirmgrößen für dynamische Layouts festgelegt werden sollten |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Tutorial zu adaptiven Layouts](../design/basics/xaml-basics-adaptive-layout.md) | Eine schrittweise geführte Anleitung zu adaptiven Layouts und zum dynamischen Design |
| [Datenbank für Kundenaufträge](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Layout und Formulare in Aktion in einem mehrseitigen Unternehmensbeispiel |
| [XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) | Eine Auswahl von XAML-Steuerelementen und Informationen zu deren Implementierung |
| [Weitere Codebeispiele](https://developer.microsoft.com/windows/samples) | Wählen Sie **Controls, layout, and text** (Steuerelemente, Layout und Text) in der Dropdownliste „Kategorie“ aus, um verwandte Codebeispiele anzuzeigen. |