---
description: In diesem Artikel wird erläutert, wie Sie Drag & Drop in Ihrer Windows-app hinzufügen.
title: Drag &amp; Drop
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9a0005ecf7d51cc6b08bc5cc61350489839d568f
ms.sourcegitcommit: 047004e2bf100e319d134c18518062bf7f3efb5d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92763104"
---
# <a name="drag-and-drop"></a>Drag &amp; Drop

Drag & Drop ist eine intuitive Methode zum Übertragen von Daten in einer Anwendung oder zwischen Anwendungen auf dem Windows-Desktop. Mithilfe von Drag & Drop kann der Benutzerdaten zwischen Anwendungen oder innerhalb einer Anwendung mithilfe einer Standard Bewegung übertragen (drücken Sie-Hold-und-Pan mit dem Finger oder drücken und Schwenken mit einer Maus oder einem Tablettstift).

> **Wichtige APIs** : [Kerzen Eigenschaft](/uwp/api/windows.ui.xaml.uielement.candrag), [AllowDrop (Eigenschaft](/uwp/api/windows.ui.xaml.uielement.allowdrop) ) 

Die Zieh Quelle, bei der es sich um die Anwendung oder den Bereich handelt, in der die Zieh Bewegung ausgelöst wird, stellt die zu übertragenden Daten bereit, indem ein Datenpaket Objekt gefüllt wird, das Standarddaten Formate enthalten kann, einschließlich Text, RTF, HTML, Bitmaps, Speicherelementen oder benutzerdefinierten Datenformaten. Die Quelle gibt auch die Art der unterstützten Vorgänge an: kopieren, verschieben oder verknüpfen. Wenn der Zeiger freigegeben wird, tritt Drop auf. Das Ablage Ziel, bei dem es sich um die Anwendung oder den Bereich unterhalb des Zeigers handelt, verarbeitet das Datenpaket und gibt den Typ des ausgeführten Vorgangs zurück.

Beim Ziehen und ablegen bietet die Zieh Benutzeroberfläche eine visuelle Anzeige der Art des Drag & Drop-Vorgangs. Dieses visuelle Feedback wird anfänglich von der Quelle bereitgestellt, kann jedoch durch die Ziele geändert werden, wenn der Mauszeiger darüber bewegt wird.

Modernes Drag & Drop ist auf allen Geräten verfügbar, die UWP unterstützen. Sie ermöglicht die Datenübertragung zwischen oder innerhalb einer beliebigen Art von Anwendung, einschließlich klassischer Windows-apps, obwohl sich dieser Artikel auf die XAML-API für modernes Drag & Drop konzentriert. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung.

Im folgenden finden Sie eine Übersicht darüber, was Sie tun müssen, um Drag & Drop in Ihrer APP zu aktivieren:

1. Aktivieren Sie das ziehen für ein Element, indem Sie die Eigenschaft **Kerzen** Wert auf true festlegen.  
2. Erstellen Sie das Datenpaket. Das System verarbeitet Bilder und Text automatisch, aber für andere Inhalte müssen Sie das **DragStarted** -und das **dragabgeschlossene** -Ereignis behandeln und zum Erstellen eines eigenen Datenpakets verwenden. 
3. Ermöglicht das Ablegen durch Festlegen der **AllowDrop** -Eigenschaft auf **true** für alle Elemente, die gelöschten Inhalt empfangen können. 
4. Behandeln Sie das **DragOver** -Ereignis, um dem System mitzuteilen, welche Art von Zieh Vorgängen das Element empfangen kann. 
5. Verarbeiten Sie das **Drop** -Ereignis, um den gelöschten Inhalt zu empfangen. 



## <a name="enable-dragging"></a>Ziehen aktivieren

Legen Sie die Eigenschaft " [**Kerzen**](/uwp/api/windows.ui.xaml.uielement.candrag) " auf " **true** " fest, um das ziehen für ein Element zu aktivieren. Dadurch wird das Element – und die darin enthaltenen Elemente im Fall von Auflistungen wie ListView – Draggable.

Stellen Sie sich vor, was es dragfähig ist. Benutzer möchten nicht alles in Ihre APP ziehen, sondern nur bestimmte Elemente, wie z. b. Bilder oder Text. 

So legen Sie [**Kerzen**](/uwp/api/windows.ui.xaml.uielement.candrag)fest.

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDragArea":::

Sie müssen keine weiteren Aktionen durchführen, um das Ziehen zu ermöglichen, es sei denn, Sie möchten die Benutzeroberfläche anpassen (dies wird später in diesem Artikel behandelt). Das Ablegen erfordert einige weitere Schritte.

## <a name="construct-a-data-package"></a>Erstellen eines Datenpakets 

In den meisten Fällen wird das System ein Datenpaket für Sie erstellen. Das System verarbeitet automatisch Folgendes:
* Bilder
* Text 

Für andere Inhalte müssen Sie das **DragStarted** -Ereignis und das **dragabgeschlossene** -Ereignis behandeln und zum Erstellen Ihres eigenen [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)verwenden.

## <a name="enable-dropping"></a>Löschen aktivieren

Das folgende Markup veranschaulicht, wie Sie mithilfe von [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) in XAML einen bestimmten Bereich der App als gültig für Dropvorgänge festlegen. Wenn ein Benutzer versucht, Elemente an anderer Stelle abzulegen, wird dies vom System nicht zugelassen. Wenn Benutzer die Möglichkeit haben sollen, Elemente an einer beliebigen Stelle Ihrer App abzulegen, legen Sie den gesamten Hintergrund als Dropziel fest.

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDropArea":::


## <a name="handle-the-dragover-event"></a>Verarbeiten des DragOver-Ereignisses

Das [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)-Ereignis wird ausgelöst, wenn ein Benutzer ein Element über Ihre App gezogen, aber noch nicht abgelegt hat. In diesem Handler müssen Sie mithilfe der [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)-Eigenschaft angeben, welche Art von Vorgängen Ihre App unterstützt. Kopieren ist der häufigste Vorgang.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOver":::

## <a name="process-the-drop-event"></a>Verarbeiten des Dropereignisses

Das [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop)-Ereignis tritt auf, wenn der Benutzer Elemente in einem gültigen Dropbereich ablegt. Verarbeiten Sie dieses Ereignis mit der [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview)-Eigenschaft.

Der Einfachheit halber gehen wir davon aus, dass der Benutzer ein einzelnes Foto abgelegt hat und direkt darauf zugreift. In der Praxis können Benutzer mehrere Elemente unterschiedlicher Formate gleichzeitig ablegen. Diese Möglichkeit sollte von Ihrer APP behandelt werden, indem Sie überprüfen, welche Dateitypen gelöscht wurden und wie viele vorhanden sind, und diese entsprechend verarbeiten. Sie sollten den Benutzer auch benachrichtigen, wenn Sie versuchen, etwas zu tun, das Ihre APP nicht unterstützt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_Drop":::

## <a name="customize-the-ui"></a>Anpassen der Benutzeroberfläche

Das System bietet eine Standardbenutzeroberfläche für Drag & Drop. Sie können jedoch auch verschiedene Teile der Benutzeroberfläche anpassen, indem Sie benutzerdefinierte Beschriftungen und Symbole festlegen oder angeben, dass keine Benutzeroberfläche angezeigt werden soll. Verwenden Sie zum Anpassen der Benutzeroberfläche die [**DragEventArgs.DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)-Eigenschaft.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOverCustom":::

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Öffnen eines Kontextmenüs für ein Element, das per Toucheingabe gezogen werden kann

Bei der Toucheingabe erfordern das Ziehen eines [**UI-Elements**](/uwp/api/Windows.UI.Xaml.UIElement) und das Öffnen des Kontextmenüs ähnliche Touchgesten. Beide beginnen mit Drücken und Halten. Hier erfahren Sie, wie das System zwischen den beiden Aktionen für Elemente unterscheidet, die beide unterstützen: 

* Wenn der Benutzer ein Element drückt und hält und es innerhalb von 500 Millisekunden zu ziehen beginnt, wird es gezogen. Das Kontextmenü wird nicht angezeigt. 
* Wenn der Benutzer drückt und hält, jedoch nicht innerhalb von 500 Millisekunden zieht, wird das Kontextmenü geöffnet. 
* Wenn der Benutzer bei geöffnetem Kontextmenü versucht, das Element zu ziehen (ohne den Finger anzuheben), wird das Kontextmenü geschlossen und das Ziehen gestartet.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Festlegen eines Elements in einer ListView oder GridView als Ordner

Sie können ein [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) oder [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) als Ordner festlegen. Dies ist besonders hilfreich für TreeView- und Datei-Explorer-Szenarien. Setzen Sie hierzu für das Element die [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop)-Eigenschaft auf **True** . 

Das System zeigt automatisch die entsprechenden Animationen zum Ablegen in einen Ordner anstelle eines Nicht-Ordner-Elements an. Der App-Code muss für das Ordnerelement (sowie das Nicht-Ordner-Element) auch weiterhin das Ereignis [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) beinhalten, damit die Datenquelle aktualisiert und das abgelegte Element im Zielordner hinzugefügt wird.

## <a name="enable-drag-and-drop-reordering-within-listviews"></a>Neuanordnen von Drag & Drop innerhalb von ListViews aktivieren

[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)s unterstützt die Drag-basierte Neuanordnung standardmäßig mit einer API, die der in diesem Artikel beschriebenen **canDrop** -API sehr ähnelt. Sie fügen mindestens die Eigenschaften **AllowDrop** und **canreorderitems** hinzu.

Weitere Informationen finden Sie unter [**listviewbase. canreorderitems**](/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) .

## <a name="implementing-custom-drag-and-drop"></a>Implementieren von benutzerdefiniertem Drag & Drop

Die [UIElement](/uwp/api/windows.ui.xaml.uielement) -Klasse erledigt den größten Teil der Arbeit bei der Implementierung von Drag & Drop für Sie. Wenn Sie möchten, können Sie Ihre eigene Version implementieren, indem Sie die APIs im [Windows. applicationmodel. datatransfer. DragDrop. Core-Namespace](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)verwenden.

| Funktionalität | WinRT-API |
| --- | --- |
|  Ziehen aktivieren | [Coredragoperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Erstellen eines Datenpakets | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Übergabe per Drag & amp; Drop in die Shell  | [Coredragoperation. startasync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Drop von der Shell empfangen  | [Coredragdropmanager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[Icoredropoperationtarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Weitere Informationen

* [App-zu-App-Kommunikation](index.md)
* [AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [DragOver](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [Dropdown](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)
