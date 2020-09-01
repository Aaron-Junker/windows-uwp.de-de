---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: In diesem Artikel wird beschrieben, wie Medien von einem Gerät importiert werden, darunter die Suche nach verfügbaren Medienquellen, das Importieren von Dateien wie Fotos und Sidecardateien und das Löschen der importierten Dateien vom Quellgerät.
title: Importieren von Medien
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 71743459227b05fff23524a81d8d192c382d4973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157434"
---
# <a name="import-media-from-a-device"></a>Importieren von Medien von einem Gerät

In diesem Artikel wird beschrieben, wie Medien von einem Gerät importiert werden, darunter die Suche nach verfügbaren Medienquellen, das Importieren von Dateien wie Videos, Fotos und Sidecardateien und das Löschen der importierten Dateien vom Quellgerät.

> [!NOTE] 
> Der Code in diesem Artikel wurde aus dem [**MediaImport UWP-App-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) übernommen und angepasst. Sie können dieses Beispiel klonen oder aus dem [**Git-Repository für universelle Windows-App-Beispiele**](https://github.com/Microsoft/Windows-universal-samples) herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="create-a-simple-media-import-ui"></a>Erstellen einer einfachen Medienimport-Benutzeroberfläche
Für das Beispiel in diesem Artikel wird eine minimale Benutzeroberfläche verwendet, um die grundlegenden Medienimportszenarien zu ermöglichen. Informationen zum Erstellen einer stabileren Benutzeroberfläche für eine Medienimport-App finden Sie im [**MediaImport-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Der folgende XAML-Code erstellt ein StackPanel-Element mit den folgenden Steuerelementen:
* Ein [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)-Steuerelement zum Initiieren der Suche nach Quellen, aus denen Medien importiert werden können.
* Ein [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)-Steuerelement zum Auflisten und Auswählen der Medienimportquellen, die gefunden werden.
* Ein [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelement zum Anzeigen und Auswählen der Medienelemente aus der ausgewählten Importquelle.
* Ein **Button**-Steuerelement zum Initiieren des Imports von Medienelementen aus der ausgewählten Quelle.
* Ein **Button**-Steuerelement zum Initiieren des Löschens der Elemente, die aus der ausgewählten Quelle importiert wurden.
* Ein **Button**-Steuerelement zum Abbrechen eines asynchronen Medienimportvorgangs.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>Einrichten der CodeBehind-Datei
Fügen Sie *using*-Direktiven hinzu, um die in diesem Beispiel verwendeten Namespaces einzubeziehen, die noch nicht in der Standardprojektvorlage enthalten sind.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>Einrichten des Abbruchs von Aufgaben für Medienimportvorgänge

Da Medienimportvorgänge lange dauern können, werden sie mit [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) asynchron ausgeführt. Deklarieren Sie eine Klassenmembervariable vom Typ [**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource), mit der ein laufender Vorgang abgebrochen wird, wenn der Benutzer auf die Schaltfläche zum Abbrechen klickt.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

Implementieren Sie einen Handler für die Schaltfläche zum Abbrechen. In den Beispielen weiter unten in diesem Artikel wird **CancellationTokenSource** initialisiert, wenn ein Vorgang beginnt, und auf Null festgelegt, wenn der Vorgang abgeschlossen ist. Überprüfen Sie im Handler für die Schaltfläche zum Abbrechen, ob das Token Null ist, und rufen Sie andernfalls [**Cancel**](/dotnet/api/system.threading.cancellationtokensource.cancel#System_Threading_CancellationTokenSource_Cancel) auf, um den Vorgang abzubrechen.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>Datenbindungs-Hilfsklassen

In einem typischen Medienimportszenario wird dem Benutzer eine Liste der zum Import verfügbaren Medienelemente angezeigt. Es kann eine große Anzahl von Mediendateien zur Auswahl vorhanden sein, und in der Regel soll eine Miniaturansicht für jedes Medienelement angezeigt werden. Aus diesem Grund werden in diesem Beispiel drei Hilfsklassen zum inkrementellen Laden von Einträgen in das ListView-Steuerelement verwendet, wenn der Benutzer in der Liste nach unten scrollt.

* **IncrementalLoadingBase**-Klasse – Implementiert [**IList**](/dotnet/api/system.collections.ilist), [**ISupportIncrementalLoading**](/uwp/api/windows.ui.xaml.data.isupportincrementalloading) und [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged), um das grundlegende inkrementelle Ladeverhalten bereitzustellen.
* **GeneratorIncrementalLoadingClass**-Klasse – Stellt die Implementierung der Basisklasse für das inkrementelle Laden bereit.
* **ImportableItemWrapper**-Klasse – Ein dünner Wrapper um die [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem)-Klasse zum Hinzufügen einer bindbaren [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Eigenschaft für das Miniaturbild für jedes importierte Element.

Diese Klassen werden im [**MediaImport-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) bereitgestellt und können dem Projekt ohne Änderungen hinzugefügt werden. Deklarieren Sie nach dem Hinzufügen der Hilfsklassen zum Projekt eine Klassenmembervariable vom Typ **GeneratorIncrementalLoadingClass**, die weiter unten in diesem Beispiel verwendet wird.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>Suchen nach verfügbaren Quellen, aus denen Medien importiert werden können

Rufen Sie im Klickhandler für die Schaltfläche zur Suche nach Quellen die statische Methode [**PhotoImportManager.FindAllSourcesAsync**](/uwp/api/windows.media.import.photoimportmanager.findallsourcesasync) auf, um die Systemsuche nach Geräten, von denen Medien importiert werden können, zu starten. Durchlaufen Sie nach dem Warten auf den Abschluss des Vorgangs jedes [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource)-Objekt in der zurückgegebenen Liste, fügen Sie einen Eintrag zu **ComboBox** hinzu, und legen Sie die **Tag**-Eigenschaft direkt auf das Quellobjekt fest, damit es einfach abgerufen werden kann, wenn der Benutzer eine Auswahl trifft.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

Deklarieren Sie eine Klassenmembervariable zum Speichern der ausgewählten Importquelle des Benutzers.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

Legen Sie im [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Handler für **ComboBox** für die Importquelle die Klassenmembervariable auf die ausgewählte Quelle fest, und rufen Sie dann die **FindItems**-Hilfsmethode auf, die weiter unten in diesem Artikel erläutert wird. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>Suchen nach zu importierenden Elementen

Fügen Sie Klassenmembervariablen vom Typ [**PhotoImportSession**](/uwp/api/Windows.Media.Import.PhotoImportSession) und [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) hinzu, die in den folgenden Schritten verwendet werden.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

Initialisieren Sie in der **FindItems** -Methode die **cancellationdekensource** -Variable, damit Sie bei Bedarf verwendet werden kann, um den Suchvorgang abzubrechen. Erstellen Sie in einem **try**-Block eine neue Importsitzung durch Aufrufen von [**CreateImportSession**](/uwp/api/windows.media.import.photoimportsource.createimportsession) für das vom Benutzer ausgewählte [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource)-Objekt. Erstellen Sie ein neues [**Progress**](/dotnet/api/system.progress-1)-Objekt, um einen Rückruf zum Anzeigen des Status des Suchvorgangs bereitzustellen. Aufrufen Sie als nächstes **[finditemsasync](/uwp/api/windows.media.import.photoimportsession.finditemsasync)** , um den Suchvorgang zu starten. Stellen Sie einen [**PhotoImportContentTypeFilter**](/uwp/api/Windows.Media.Import.PhotoImportContentTypeFilter)-Wert bereit, um anzugeben, ob Fotos, Videos oder beides zurückgegeben werden sollen. Stellen Sie einen [**PhotoImportItemSelectionMode**](/uwp/api/Windows.Media.Import.PhotoImportItemSelectionMode)-Wert bereit, um anzugeben, ob alle, keine oder nur die neuen Medienelemente zurückgegeben werden, deren [**IsSelected**](/uwp/api/windows.media.import.photoimportitem.isselected)-Eigenschaft auf „true“ festgelegt ist. Diese Eigenschaft ist an ein Kontrollkästchen für jedes Medienelement in der ListBox-Elementvorlage gebunden.

**FindItemsAsync** gibt [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) zurück. Die Erweiterungsmethode [**AsTask**](/dotnet/api/system) wird verwendet, um eine Aufgabe zu erstellen, auf die gewartet werden kann, die mit dem Abbruchtoken abgebrochen werden kann und den Status mit dem bereitgestellten **Progress**-Objekt meldet.

Als Nächstes wird die Datenbindungs-Hilfsklasse **GeneratorIncrementalLoadingClass** initialisiert. **FindItemsAsync** gibt bei der Rückkehr aus dem Wartezustand ein [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult)-Objekt zurück. Dieses Objekt enthält Statusinformationen über den Suchvorgang, darunter den Erfolg des Vorgangs und die Anzahl der verschiedenen Typen von Medienelementen, die gefunden wurden. Die [**FoundItems**](/uwp/api/windows.media.import.photoimportfinditemsresult.founditems)-Eigenschaft enthält eine Liste von [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem)-Objekten, die die gefundenen Medienelemente darstellen. Der **GeneratorIncrementalLoadingClass**-Konstruktor verwendet als Argumente die Gesamtzahl der Elemente, die inkrementell geladen werden, und eine Funktion, die je nach Bedarf zu ladende neue Elemente generiert. In diesem Fall erstellt der bereitgestellten Lambda-Ausdruck eine neue Instanz von **ImportableItemWrapper**, der **PhotoImportItem** umschließt und eine Miniaturansicht für jedes Element enthält. Nachdem die Klasse zum inkrementellen Laden initialisiert wurde, legen Sie diese auf die [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)-Eigenschaft des **ListView**-Steuerelements in der UI fest. Jetzt werden die gefundenen Medienelemente inkrementell geladen und in der Liste angezeigt.

Als Nächstes werden die Statusinformationen für den Suchvorgang ausgegeben. Eine typische App zeigt diese Informationen dem Benutzer in der UI an, aber in diesem Beispiel werden die Informationen einfach in der Debugkonsole ausgegeben. Legen Sie zum Schluss das Abbruchtoken auf Null fest, da der Vorgang abgeschlossen ist.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>Importieren von Medienelementen

Deklarieren Sie vor dem Implementieren des Importvorgangs ein [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult)-Objekt zur Speicherung der Ergebnisse des Importvorgangs. Dieses wird später zum Löschen von Medienelementen verwendet, die erfolgreich aus der Quelle importiert wurden.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

Initialisieren Sie vor dem Starten der Medienimportvorgangs die **CancellationTokenSource**-Variable, und legen Sie den Wert des [**ProgressBar**](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)-Steuerelements auf 0 fest.

Wenn keine ausgewählten Elemente im **ListView**-Steuerelement vorhanden sind, gibt es nichts zu importieren. Initialisieren Sie andernfalls ein [**Progress**](/dotnet/api/system.progress-1)-Objekt, um einen Statusrückruf bereitzustellen, der den Wert des Steuerelements für die Statusanzeige aktualisiert. Registrieren Sie einen Handler für das [**ItemImported**](/uwp/api/windows.media.import.photoimportfinditemsresult.itemimported)-Ereignis von [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult), das vom Suchvorgang zurückgegeben wird. Dieses Ereignis wird ausgelöst, wenn ein Element importiert wird, und gibt in diesem Beispiel den Namen jeder importierten Datei in der Debugkonsole aus.

Rufen Sie [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) auf, um den Importvorgang zu starten. Genau wie bei dem Suchvorgang wird die [**AsTask**](/dotnet/api/system)-Erweiterungsmethode verwendet, um den zurückgegebenen Vorgang in eine Ausgabe zu konvertieren, auf die gewartet werden kann, die den Status meldet und abgebrochen werden kann.

Nach Abschluss des Importvorgangs kann der Vorgangsstatus aus dem [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult)-Objekt abgerufen werden, das von [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) zurückgegeben wird. In diesem Beispiel werden die Statusinformationen an die Debugkonsole ausgegeben und abschließend das Abbruchtoken auf Null festlegt.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>Löschen von importierten Elementen
Um erfolgreich importierte Elemente aus der Quelle zu löschen, aus der sie importiert wurden, initialisieren Sie zuerst das Abbruchtoken so, dass der Löschvorgang abgebrochen werden kann, und legen Sie den Wert der Statusanzeige auf 0 fest. Stellen Sie sicher, dass das [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult), das von [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) zurückgegeben wurde, nicht Null ist. Erstellen Sie andernfalls erneut ein [**Progress**](/dotnet/api/system.progress-1)-Objekt, um einen Statusrückruf für den Löschvorgang bereitzustellen. Rufen Sie [**DeleteImportedItemsFromSourceAsync**](/uwp/api/windows.media.import.photoimportimportitemsresult.deleteimporteditemsfromsourceasync) auf, um den Löschvorgang für die importierten Elemente zu starten. Verwenden Sie **AsTask**, um das Ergebnis in eine awaitable-Aufgabe mit Status- und Abbruchfunktionen zu konvertieren. Nach dem Warten kann das zurückgegebene [**PhotoImportDeleteImportedItemsFromSourceResult**](/uwp/api/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult)-Objekt zum Abrufen und Anzeigen von Statusinformationen über den Löschvorgang verwendet werden.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








