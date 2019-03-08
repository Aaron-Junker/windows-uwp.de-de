---
description: In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) Drag & Drop hinzufügen.
title: Drag & Drop
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e508feb8a530f29b40d5a3839df573cb2ce89896
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634395"
---
# <a name="drag-and-drop"></a>Drag & Drop

Drag&Drop ist eine intuitive Methode zum Übertragen von Daten in einer Anwendung oder zwischen Anwendungen auf dem Windows-Desktop. Per Drag&Drop können Benutzer Daten zwischen Anwendungen oder innerhalb einer Anwendung mit einer Standardgeste übertragen (Drücken, Halten und Ziehen mit dem Finger bzw. Drücken und Ziehen mit der Maus oder einem Stift.)

> **Wichtige APIs**: ["CanDrag"-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), ["AllowDrop"-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

Die Ziehquelle, d. h die Anwendung oder der Bereich, in der bzw. dem die Ziehbewegung ausgelöst wird, stellt die zu übertragenden Daten bereit, indem ein Datenpaketobjekt gefüllt wird, das Standarddatenformate enthalten kann, einschließlich Text, RTF, HTML, Bitmaps, Speicherelemente oder benutzerdefinierte Datenformate. Die Quelle gibt auch die Art von Vorgängen an, die unterstützt werden: Kopieren, Verschieben oder Verknüpfen. Wenn der Zeiger losgelassen wird, werden die Daten abgelegt. Das Ablageziel, d. h. die Anwendung oder der Bereich unter dem Zeiger, verarbeitet das Datenpaket und gibt den ausgeführten Vorgangstyp zurück.

Während des Drag&Drop-Vorgangs stellt die Ziehbenutzeroberfläche eine visuelle Anzeige des stattfindenden Drag&Drop-Vorgangstyps bereit. Dieses visuelle Feedback wird anfangs von der Quelle bereitgestellt, kann aber von den Zielen geändert werden, wenn der Mauszeiger über diese bewegt wird.

Modernes Drag&Drop ist auf allen Geräten verfügbar, die UWP unterstützen. Es ermöglicht Datenübertragungen zwischen oder innerhalb von jeder Art von Anwendung, einschließlich klassischen Windows-Apps. Der Schwerpunkt dieses Artikels liegt jedoch auf der XAML-API für modernes Drag&Drop. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung.

Nachfolgend finden Sie ein Überblick darüber, was Sie tun müssen, um Drag&Drop in Ihrer App zu aktivieren:

1. Aktivieren Sie das Ziehen eines Elements, indem Sie die Eigenschaft **CanDrag** auf true festlegen.  
2. Erstellen Sie das Datenpaket. Das System verarbeitet Bilder und Text automatisch, für andere Inhalte müssen Sie jedoch die Ereignisse **DragStarted** und **DragCompleted** bearbeiten und verwenden, um ein eigenes Datenpaket zu erstellen. 
3. Aktivieren Sie das Ablegen, indem Sie die Eigenschaft **AllowDrop** für alle Elemente auf **true** festlegen, die abgelegte Inhalte empfangen können. 
4. Bearbeiten Sie das Ereignis **DragOver**, damit das System erkennt, welche Art von Ziehvorgängen das Element empfangen kann. 
5. Verarbeiten Sie das Ereignis **Drop** so, dass die abgelegten Inhalte empfangen werden. 



## <a name="enable-dragging"></a>Aktivieren des Ziehens

Legen Sie zum Aktivieren des Ziehens eines Elements die Eigenschaft [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) auf **true** fest. Damit wird das Element – und die Elemente, die es enthält, im Fall von Sammlungen wie ListView – ziehbar.

Geben Sie genau an, was ziehbar ist. Benutzer möchten nicht alle Elemente Ihrer App ziehen, sondern nur bestimmte Elemente wie Bilder oder Text. 

Nachfolgend sehen Sie, wie Sie [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) festlegen.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Sie müssen keine weiteren Aktionen durchführen, um das Ziehen zu ermöglichen, es sei denn, Sie möchten die Benutzeroberfläche anpassen (dies wird später in diesem Artikel behandelt). Das Ablegen erfordert einige weitere Schritte.

## <a name="construct-a-data-package"></a>Zusammensetzen eines Datenpakets 

In den meisten Fällen stellt das System ein Datenpaket für Sie zusammen. Das System behandelt Folgendes automatisch:
* Abbilder
* Text 

Für andere Inhalte müssen Sie die Ereignisse **DragStarted** und **DragCompleted** bearbeiten und verwenden, um ein eigenes [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage) zusammenzustellen.

## <a name="enable-dropping"></a>Aktivieren des Ablegens

Das folgende Markup veranschaulicht, wie Sie mithilfe von [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) in XAML einen bestimmten Bereich der App als gültig für Dropvorgänge festlegen. Wenn ein Benutzer versucht, Elemente an anderer Stelle abzulegen, wird dies vom System nicht zugelassen. Wenn Benutzer die Möglichkeit haben sollen, Elemente an einer beliebigen Stelle Ihrer App abzulegen, legen Sie den gesamten Hintergrund als Dropziel fest.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>Verarbeiten des DragOver-Ereignisses

Das [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver)-Ereignis wird ausgelöst, wenn ein Benutzer ein Element über Ihre App gezogen, aber noch nicht abgelegt hat. In diesem Handler müssen Sie mithilfe der [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation)-Eigenschaft angeben, welche Art von Vorgängen Ihre App unterstützt. Kopieren ist der häufigste Vorgang.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Verarbeiten des Dropereignisses

Das [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop)-Ereignis tritt auf, wenn der Benutzer Elemente in einem gültigen Dropbereich ablegt. Verarbeiten Sie dieses Ereignis mit der [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView)-Eigenschaft.

Der Einfachheit halber wird im folgenden Beispiel angenommen, dass der Benutzer ein einzelnes Foto abgelegt hat und direkt auf dieses zugreift. In der Praxis können Benutzer mehrere Elemente unterschiedlicher Formate gleichzeitig ablegen. Die App sollte diese Möglichkeit bereitstellen, indem sie überprüft, welche Dateitypen abgelegt wurden und wie viele vorhanden sind, und jede entsprechend verarbeiten. Sie sollten auch überlegen, ob der Benutzer benachrichtigt werden sollte, wenn er versucht, eine Aktion auszuführen, die die App nicht unterstützt.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Anpassen der Benutzeroberfläche

Das System bietet eine Standardbenutzeroberfläche für Drag & Drop. Sie können jedoch auch verschiedene Teile der Benutzeroberfläche anpassen, indem Sie benutzerdefinierte Beschriftungen und Symbole festlegen oder angeben, dass keine Benutzeroberfläche angezeigt werden soll. Verwenden Sie zum Anpassen der Benutzeroberfläche die [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride)-Eigenschaft.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Öffnen eines Kontextmenüs für ein Element, das per Toucheingabe gezogen werden kann

Bei der Toucheingabe erfordern das Ziehen eines [**UI-Elements**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) und das Öffnen des Kontextmenüs ähnliche Touchgesten. Beide beginnen mit Drücken und Halten. Hier erfahren Sie, wie das System zwischen den beiden Aktionen für Elemente unterscheidet, die beide unterstützen: 

* Wenn der Benutzer ein Element drückt und hält und es innerhalb von 500 Millisekunden zu ziehen beginnt, wird es gezogen. Das Kontextmenü wird nicht angezeigt. 
* Wenn der Benutzer drückt und hält, jedoch nicht innerhalb von 500 Millisekunden zieht, wird das Kontextmenü geöffnet. 
* Wenn der Benutzer bei geöffnetem Kontextmenü versucht, das Element zu ziehen (ohne den Finger anzuheben), wird das Kontextmenü geschlossen und das Ziehen gestartet.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Festlegen eines Elements in einer ListView oder GridView als Ordner

Sie können ein [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) oder [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) als Ordner festlegen. Dies ist besonders hilfreich für TreeView- und Datei-Explorer-Szenarien. Setzen Sie hierzu für das Element die [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop)-Eigenschaft auf **True**. 

Das System zeigt automatisch die entsprechenden Animationen zum Ablegen in einen Ordner anstelle eines Nicht-Ordner-Elements an. Der App-Code muss für das Ordnerelement (sowie das Nicht-Ordner-Element) auch weiterhin das Ereignis [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) beinhalten, damit die Datenquelle aktualisiert und das abgelegte Element im Zielordner hinzugefügt wird.

## <a name="implementing-custom-drag-and-drop"></a>Implementieren von benutzerdefiniertem Drag&Drop

Die [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)-Klasse übernimmt die meiste Arbeit bei der Implementierung von Drag&Drop für Sie. Aber wenn Sie möchten, können Sie Ihre eigene Version implementieren, indem Sie mithilfe der APIs in der [Windows.ApplicationModel.DataTransfer.DragDrop.Core Namespace](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Funktion | WinRT-API |
| --- | --- |
|  Aktivieren des Ziehens | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Erstellen eines Datenpakets | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Übergeben des Ziehens an die Shell  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Empfangen des Drops von der Shell  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Siehe auch

* [App-zu-App-Kommunikation](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* ["DataView"](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
