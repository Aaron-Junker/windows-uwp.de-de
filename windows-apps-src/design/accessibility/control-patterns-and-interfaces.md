---
Description: Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Steuerelementmuster und Schnittstellen
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: adbe1556f48e2f9b362faa303be52586714c73d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157024"
---
# <a name="control-patterns-and-interfaces"></a>Steuerelementmuster und Schnittstellen  



Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.

In der Tabelle in diesem Thema werden die Steuerelementmuster der Microsoft-Benutzeroberflächenautomatisierung erläutert. Die Tabelle enthält außerdem die Klassen, die von Benutzeroberflächenautomatisierungs-Clients für den Zugriff auf die Steuerelementmuster verwendet werden, sowie die Schnittstellen, die Benutzeroberflächenautomatisierungs-Anbieter zu deren Implementierung nutzen. In der Spalte **Steuerelementmuster** wird der Mustername aus Sicht des Benutzeroberflächenautomatisierungs-Clients als Konstantenwert angezeigt, der in [**Eigenschaftsbezeichnern für die Verfügbarkeit von Steuerelementmustern**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) aufgeführt wird. Aus Sicht des Benutzeroberflächenautomatisierungs-Anbieters ist jedes dieser Muster ein [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)-Konstantenname. In der Spalte **Schnittstelle für Klassenanbieter** wird der Name der Windows-Runtime-Schnittstelle angezeigt, die von Anbietern implementiert wird, um dieses Muster für ein benutzerdefiniertes XAML-Steuerelement bereitzustellen.

Weitere Informationen zum Implementieren benutzerdefinierter Automatisierungspeers, von denen Steuerelementmuster verfügbar gemacht und die Schnittstellen implementiert werden, finden Sie unter [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md).

Beim Implementieren eines Steuerelementmusters sollten Sie auch die Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter heranziehen. Darin werden einige Erwartungen beschrieben, die Clients unabhängig davon, welches Benutzeroberflächenframework zum Implementieren verwendet wird, an ein Steuerelementmuster stellen. Einige Informationen, die in der allgemeinen Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter aufgeführt sind, haben Einfluss darauf, wie Sie Ihre Peers implementieren und das Muster richtig unterstützen. Lesen Sie sich die Informationen unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) durch, und zeigen Sie die Seite an, auf der das gewünschte Muster dokumentiert ist.

| Steuerelementmuster | Schnittstelle für Klassenanbieter | BESCHREIBUNG |
|-----------------|--------------------------|-------------|
| **Anmerkung** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | Damit werden die Eigenschaften einer Anmerkung in einem Dokument verfügbar gemacht. |
| **Dock** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | Wird für Steuerelemente verwendet, die in einem Dockingcontainer angedockt werden können. Beispielsweise Symbolleisten oder Toolpaletten. |
| **Ziehen** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | Wird zum Unterstützen von ziehbaren Steuerelementen bzw. Steuerelementen mit ziehbaren Elementen verwendet. |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | Wird zum Unterstützen von Steuerelementen verwendet, die Ziel eines Drag & Drop-Vorgangs sein können. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | Wird zum Unterstützen von Steuerelementen verwendet, die visuell erweitert oder reduziert werden, um mehr bzw. weniger Inhalt anzuzeigen. |
| **Raster** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | Wird für Steuerelemente verwendet, die Rasterfunktionen wie z. B. Größenanpassung und Verschieben in eine bestimmte Zelle unterstützen. Beachten Sie, dass dieses Muster nicht vom Raster selbst implementiert wird. Dies liegt daran, dass das Raster zwar ein Layout bereitstellt, aber kein Steuerelement ist. |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | Wird für Steuerelemente verwendet, die Zellen innerhalb von Rastern haben. |
| **Invoke** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | Wird für Steuerelemente verwendet, die aufgerufen werden können, z. b. eine  [**Schaltfläche**](/uwp/api/Windows.UI.Xaml.Controls.Button). |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | Ermöglicht es Anwendungen, Elemente in einem Container (z. B. in einer virtualisierten Liste) zu finden. |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | Wird für Steuerelemente verwendet, die zwischen mehreren Darstellungen derselben Informationen, Daten oder untergeordneten Elemente wechseln können. |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | Wird verwendet, um für das zugrunde liegende Objektmodell eines Dokuments einen Zeiger verfügbar zu machen. |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | Wird für Steuerelemente verwendet, die einen Bereich von Werten haben, die auf das Steuerelement angewendet werden können. So verfügt ein Drehfeld, das Jahre enthält, beispielsweise über einen Bereich von 1900 bis zum aktuellen Jahr. Ein anderes Drehfeld, das für Monate steht, würde jedoch über einen Bereich von 1 bis 12 verfügen. |
| **Scrollen** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | Wird für Steuerelemente verwendet, in denen gescrollt werden kann. Beispielsweise ein Steuerelement, das Scrollleisten hat, die nur aktiv sind, wenn mehr Informationen vorhanden sind, als im Anzeigebereich des Steuerelements angezeigt werden können. |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | Wird für Steuerelemente verwendet, die einzelne Elemente in einer Liste haben, die gescrollt werden kann. Beispielsweise ein Listensteuerelement, das einzelne Elementen in der Scrollliste hat, etwa ein Kombinationsfeld-Steuerelement. |
| **Auswahl** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | Wird für Auswahlcontainer-Steuerelemente verwendet. Beispiel: [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) und [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox). |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | Wird für einzelne Elemente in Auswahlcontainer-Steuerelementen verwendet, z. B. Listen- und Kombinationsfelder. |
| **Spreadsheet** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | Wird verwendet, um den Inhalt einer Tabellenkalkulation oder eines anderen rasterbasierten Dokuments verfügbar zu machen. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | Wird verwendet, um die Eigenschaften einer Zelle in einer Tabellenkalkulation oder einem anderen rasterbasierten Dokument verfügbar zu machen. |
| **Stile** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | Wird verwendet, um ein UI-Element zu beschreiben, das in Bezug auf Stil, Füllfarbe, Füllmuster oder Form über bestimmte Einstellungen verfügt. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | Ermöglicht Benutzeroberflächenautomatisierungs-Client-Apps das Lenken der Maus- oder Tastatureingabe auf ein bestimmtes UI-Element. |
| **Table** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | Wird für Steuerelemente verwendet, die sowohl ein Raster als auch Überschrifteninformationen haben. Beispiel: ein Steuerelement für einen tabellarischen Kalender. |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | Wird für Elemente in einer Tabelle verwendet. |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | Wird für Bearbeitungssteuerelemente und Dokumente verwendet, die Textinformationen verfügbar machen. Siehe auch [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) und [**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | Wird für den Zugriff auf den nächstgelegenen Vorgänger eines Elements verwendet, der das **Text**-Steuerelementmuster unterstützt. |
| **TextEdit** | Keine verwaltete Klasse verfügbar | Gewährt Zugriff auf ein Steuerelement, mit dem Text geändert wird. Dies kann beispielsweise ein Steuerelement sein, mit dem die Autokorrektur durchgeführt oder mithilfe eines Input Method Editors (IME) die Komposition der Eingabe ermöglicht wird. |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | Bietet Zugriff auf einen Bereich mit fortlaufendem Text in einem Textcontainer, von dem [**ITextProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider) implementiert wird. Siehe auch [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Ein-/Ausschalten** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | Wird für Steuerelemente verwendet, deren Zustand umgeschaltet werden kann. Beispiel: aktivierbare [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Steuerelemente und Menüelemente. |
| **Transformieren** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | Wird für Steuerelemente verwendet, die in der Größe geändert, verschoben und gedreht werden können. Typische Einsatzfälle für das Transform-Steuerelementmuster sind Designer, Formulare, Grafik-Editoren und Zeichnungsanwendung. |
| **Wert** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | Ermöglicht Clients das Abrufen oder Festlegen eines Werts für ein Steuerelement, das keinen Wertebereich unterstützt. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | Macht Elemente in Containern verfügbar, die virtualisiert sind und vollständig als Benutzeroberflächenautomatisierungs-Elemente zur Verfügung stehen müssen. |
| **Fenster** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Macht für bestimmte Fenster spezifische Informationen verfügbar. Hierbei handelt es sich um ein grundlegendes Konzept des Microsoft Windows-Betriebssystems. Beispiele für Steuerelemente als Fenster: untergeordnete Fenster und Dialogfelder. |

> [!NOTE]
> Implementierungen all dieser Muster sind in vorhandenen XAML-Steuerelementen nicht immer enthalten. Einige Muster verfügen nur über Schnittstellen, um die Parität mit der allgemeinen Benutzeroberflächenautomatierungs-Frameworkdefinition für Muster sowie Automatisierungspeerszenarien zu unterstützen, die für die Unterstützung dieses Musters eine rein benutzerdefinierte Implementierung benötigen.

> [!NOTE]
> Windows Phone Store-Apps unterstützen nicht alle hier aufgeführten Steuerelementmuster der Benutzeroberflächenautomatisierung. Zu den nicht unterstützten Mustern zählen beispielsweise **Annotation**, **Dock**, **Drag**, **DropTarget** und **ObjectModel**.

<span id="related_topics"/>

## <a name="related-topics"></a>Zugehörige Themen  
* [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md)
* [Bedienungshilfen](accessibility.md)