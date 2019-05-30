---
Description: Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Steuerelementmuster und Schnittstellen
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fc8018a4ccf53c106a1e10410593e214d9dcead7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359566"
---
# <a name="control-patterns-and-interfaces"></a>Steuerelementmuster und Schnittstellen  



Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.

In der Tabelle in diesem Thema werden die Steuerelementmuster der Microsoft-Benutzeroberflächenautomatisierung erläutert. Die Tabelle enthält außerdem die Klassen, die von Benutzeroberflächenautomatisierungs-Clients für den Zugriff auf die Steuerelementmuster verwendet werden, sowie die Schnittstellen, die Benutzeroberflächenautomatisierungs-Anbieter zu deren Implementierung nutzen. In der Spalte **Steuerelementmuster** wird der Mustername aus Sicht des Benutzeroberflächenautomatisierungs-Clients als Konstantenwert angezeigt, der in [**Eigenschaftsbezeichnern für die Verfügbarkeit von Steuerelementmustern**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) aufgeführt wird. Aus Sicht des Benutzeroberflächenautomatisierungs-Anbieters ist jedes dieser Muster ein [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)-Konstantenname. In der Spalte **Schnittstelle für Klassenanbieter** wird der Name der Windows-Runtime-Schnittstelle angezeigt, die von Anbietern implementiert wird, um dieses Muster für ein benutzerdefiniertes XAML-Steuerelement bereitzustellen.

Weitere Informationen zum Implementieren benutzerdefinierter Automatisierungspeers, von denen Steuerelementmuster verfügbar gemacht und die Schnittstellen implementiert werden, finden Sie unter [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md).

Beim Implementieren eines Steuerelementmusters sollten Sie auch die Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter heranziehen. Darin werden einige Erwartungen beschrieben, die Clients unabhängig davon, welches Benutzeroberflächenframework zum Implementieren verwendet wird, an ein Steuerelementmuster stellen. Einige Informationen, die in der allgemeinen Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter aufgeführt sind, haben Einfluss darauf, wie Sie Ihre Peers implementieren und das Muster richtig unterstützen. Lesen Sie sich die Informationen unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) durch, und zeigen Sie die Seite an, auf der das gewünschte Muster dokumentiert ist.

| Steuerelementmuster | Schnittstelle für Klassenanbieter | Beschreibung |
|-----------------|--------------------------|-------------|
| **Annotation** | [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | Damit werden die Eigenschaften einer Anmerkung in einem Dokument verfügbar gemacht. |
| **Dock** | [**IDockProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | Wird für Steuerelemente verwendet, die in einem Andockcontainer angedockt werden können. Beispiele: Symbolleisten oder Toolpaletten. |
| **Ziehen Sie** | [**IDragProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | Wird zum Unterstützen von ziehbaren Steuerelementen bzw. Steuerelementen mit ziehbaren Elementen verwendet. |
| **DropTarget** | [**IDropTargetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | Wird zum Unterstützen von Steuerelementen verwendet, die Ziel eines Drag & Drop-Vorgangs sein können. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | Wird zum Unterstützen von Steuerelementen verwendet, die visuell erweitert oder reduziert werden, um mehr bzw. weniger Inhalt anzuzeigen. |
| **Raster** | [**IGridProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | Wird für Steuerelemente verwendet, von denen Rasterfunktionen wie etwa Größenanpassung und Verschieben in eine angegebene Zelle unterstützt werden. Beachten Sie, dass dieses Muster nicht vom Raster selbst implementiert wird. Dies liegt daran, dass das Raster zwar ein Layout bereitstellt, aber kein Steuerelement ist. |
| **GridItem** | [**IGridItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | Wird für Steuerelemente verwendet, die innerhalb von Rastern über Zellen verfügen. |
| **Rufen Sie** | [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | Wird für Steuerelemente verwendet, die aufgerufen werden können, beispielsweise ein [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)-Steuerelement. |
| **ItemContainer** | [**IItemContainerProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | Ermöglicht es Anwendungen, Elemente in einem Container (z. B. in einer virtualisierten Liste) zu finden. |
| **MultipleView** | [**IMultipleViewProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | Wird für Steuerelemente verwendet, in denen zwischen verschiedenen Darstellungen des gleichen Informationssatzes, Datensatzes oder Satzes von untergeordneten Elementen umgeschaltet werden kann. |
| **ObjectModel** | [**IObjectModelProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | Wird verwendet, um für das zugrunde liegende Objektmodell eines Dokuments einen Zeiger verfügbar zu machen. |
| **RangeValue** | [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | Wird für Steuerelemente verwendet, die über einen Wertebereich verfügen, der auf das Steuerelement angewendet werden kann. So verfügt ein Drehfeld, das Jahre enthält, beispielsweise über einen Bereich von 1900 bis zum aktuellen Jahr. Ein anderes Drehfeld, das für Monate steht, würde jedoch über einen Bereich von 1 bis 12 verfügen. |
| **Scroll** | [**IScrollProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | Wird für Steuerelemente verwendet, für die ein Bildlauf durchgeführt werden kann. Beispiel: ein Steuerelement mit Bildlaufleisten, die aktiv sind, wenn mehr Informationen vorhanden sind, als im Anzeigebereich des Steuerelements angezeigt werden können. |
| **ScrollItem** | [**IScrollItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | Wird für Steuerelemente verwendet, die über einzelne Elemente in einer Liste mit Bildlauf verfügen. Beispiel: ein Steuerelement für eine Liste, das über einzelne Elemente in der Bildlaufliste verfügt, wie etwa ein Steuerelement für ein Kombinationsfeld. |
| **Auswahl** | [**ISelectionProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | Wird für Steuerelemente für Auswahlcontainer verwendet. Beispiel: [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) und [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox). |
| **SelectionItem** | [**ISelectionItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | Wird für einzelne Elemente in Steuerelementen für Auswahlcontainer verwendet, z. B. Listen- und Kombinationsfelder. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | Wird verwendet, um den Inhalt einer Tabellenkalkulation oder eines anderen rasterbasierten Dokuments verfügbar zu machen. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | Wird verwendet, um die Eigenschaften einer Zelle in einer Tabellenkalkulation oder einem anderen rasterbasierten Dokument verfügbar zu machen. |
| **Stile** | [**IStylesProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | Wird verwendet, um ein UI-Element zu beschreiben, das in Bezug auf Stil, Füllfarbe, Füllmuster oder Form über bestimmte Einstellungen verfügt. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | Ermöglicht Benutzeroberflächenautomatisierungs-Client-Apps das Lenken der Maus- oder Tastatureingabe auf ein bestimmtes UI-Element. |
| **Table** | [**ITableProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | Wird für Steuerelemente verwendet, die sowohl über ein Raster als auch über Kopfzeileninformationen verfügen. Beispiel: ein Steuerelement für einen tabellarischen Kalender. |
| **TableItem** | [**ITableItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | Wird für Elemente in einer Tabelle verwendet. |
| **Text** | [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | Wird für Bearbeitungssteuerelemente und für Dokumente verwendet, in denen Informationen in Textform verfügbar gemacht werden. Siehe auch [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) und [**ITextProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | Wird für den Zugriff auf den nächstgelegenen Vorgänger eines Elements verwendet, der das **Text**-Steuerelementmuster unterstützt. |
| **TextEdit** | Keine verwaltete Klasse verfügbar | Gewährt Zugriff auf ein Steuerelement, mit dem Text geändert wird. Dies kann beispielsweise ein Steuerelement sein, mit dem die Autokorrektur durchgeführt oder mithilfe eines Input Method Editors (IME) die Komposition der Eingabe ermöglicht wird. |
| **TextRange** | [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | Bietet Zugriff auf einen Bereich mit fortlaufendem Text in einem Textcontainer, von dem [**ITextProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider) implementiert wird. Siehe auch [**ITextRangeProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Ein-/Ausschalten** | [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | Wird für Steuerelemente verwendet, deren Status geändert werden kann. Beispiel: aktivierbare [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Steuerelemente und Menüelemente. |
| **Transform** | [**ITransformProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | Wird für Steuerelemente verwendet, deren Größe geändert werden kann und die verschoben und gedreht werden können. Gewöhnlich wird das Steuerelementmuster für Transformation in Designern, Formen, Grafikeditoren und Zeichenanwendungen verwendet. |
| **Wert** | [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | Ermöglicht es Clients, einen Wert von Steuerelementen abzurufen, die keinen Wertebereich unterstützen, oder einen Wert dafür festzulegen. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | Macht Elemente in Containern verfügbar, die virtualisiert sind und vollständig als Benutzeroberflächenautomatisierungs-Elemente zur Verfügung stehen müssen. |
| **Window** | [**IWindowProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Informationen speziell für Windows, ein grundlegendes Konzept für das Microsoft Windows-Betriebssystem verfügbar gemacht. Beispiele für Steuerelemente als Fenster: untergeordnete Fenster und Dialogfelder. |

> [!NOTE]
> Implementierungen all dieser Muster sind in vorhandenen XAML-Steuerelementen nicht immer enthalten. Einige Muster verfügen nur über Schnittstellen, um die Parität mit der allgemeinen Benutzeroberflächenautomatierungs-Frameworkdefinition für Muster sowie Automatisierungspeerszenarien zu unterstützen, die für die Unterstützung dieses Musters eine rein benutzerdefinierte Implementierung benötigen.

> [!NOTE]
> Windows Phone Store-Apps unterstützen nicht alle hier aufgeführten Steuerelementmuster der Benutzeroberflächenautomatisierung. Zu den nicht unterstützten Mustern zählen beispielsweise **Annotation**, **Dock**, **Drag**, **DropTarget** und **ObjectModel**.

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen  
* [Benutzerdefinierte Automatisierungs-Peers](custom-automation-peers.md)
* [Bedienungshilfen](accessibility.md) 
