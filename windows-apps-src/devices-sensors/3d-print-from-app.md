---
title: 3D-Druck über Ihre App
description: Erfahren Sie, wie Sie Ihrer universellen Windows-App 3D-Druckfunktionen hinzufügen. In diesem Thema wird erläutert, wie das 3D-Drucken-Dialogfeld aufgerufen wird, nachdem Sie sich vergewissert haben, dass das 3D-Modell gedruckt werden kann und im richtigen Format vorliegt.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 3dprinting, 3D--Druck
ms.localizationpriority: medium
ms.openlocfilehash: b89fb14b8e554452674e0c7b0bc31b6314cce253
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175494"
---
# <a name="3d-printing-from-your-app"></a>3D-Druck über Ihre App

**Wichtige APIs**

-   [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D)

Erfahren Sie, wie Sie Ihrer universellen Windows-App 3D-Druckfunktionen hinzufügen. In diesem Thema wird erläutert, wie Sie 3D-Geometriedaten in Ihre App laden und das 3D-Druckdialogfeld starten, nachdem Sie sichergestellt haben, dass Ihr 3D-Modell druckbar ist und das richtige Format hat. Ein funktionierendes Beispiel für diese Prozeduren finden Sie unter Beispiel für die [3D-Druck-UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> Im Beispielcode in diesem Handbuch wird die Fehlerberichterstattung und-Behandlung aus Gründen der Einfachheit erheblich vereinfacht.

## <a name="setup"></a>Einrichten


Fügen Sie in Ihrer Anwendungsklasse, die eine 3D-Druck Funktionalität hat, den [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D) -Namespace hinzu.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

In diesem Handbuch werden die folgenden zusätzlichen Namespaces verwendet.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Legen Sie als nächstes die Klasse hilfreiche Element Felder ab. Deklarieren Sie ein [**Print3DTask**](/uwp/api/Windows.Graphics.Printing3D.Print3DTask) -Objekt, das die Druck Aufgabe darstellt, die an den Druckertreiber übermittelt werden soll. Deklarieren Sie ein [**storagefile**](/uwp/api/Windows.Storage.StorageFile) -Objekt, um die ursprüngliche 3D-Datendatei zu speichern, die in die App geladen wird. Deklarieren Sie ein [**Printing3D3MFPackage**](/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage)-Objekt, das ein 3D-Modell mit allen erforderlichen Metadaten darstellt und gedruckt werden kann.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Erstellen einer einfachen UI

Dieses Beispiel enthält drei Benutzer Steuerelemente: eine Schaltfläche "Laden", die eine Datei in den Programmspeicher bringt, eine Fix Schaltfläche, die die Datei nach Bedarf ändert, und eine Schaltfläche "Drucken", mit der der Druckauftrag initiiert wird. Der folgende Code erstellt diese Schaltflächen (mit Ihren on-Click-Ereignis Handlern) in der entsprechenden XAML-Datei Ihrer cs-Klasse.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Hinzufügen eines **TextBlock**-Elements für UI-Feedback:

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Abrufen der 3D-Daten


Die Methode, mit der Ihre App 3D-Geometriedaten abruft, kann variieren. Ihre APP kann Daten aus einem 3D-Scan abrufen, Modelldaten aus einer Webressource herunterladen oder ein 3D-Mesh mithilfe mathematischer Formeln oder Benutzereingaben Programm gesteuert generieren. Der Einfachheit halber wird in dieser Anleitung gezeigt, wie Sie eine 3D-Datendatei (mit einem beliebigen gängigen Dateityp) aus dem Gerätespeicher in den Programmspeicher laden. Die [Modellbibliothek für 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) enthält verschiedene Modelle, die Sie leicht auf Ihr Gerät herunterladen können.

Verwenden Sie in `OnLoadClick` der-Methode die [**fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) -Klasse, um eine einzelne Datei in den Arbeitsspeicher Ihrer APP zu laden.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Konvertieren in das 3D Manufacturing Format (3MF) mithilfe von 3D Builder

An dieser Stelle können Sie eine 3D-Datendatei in den Speicher der App laden. 3D-Geometriedaten können aber in den unterschiedlichsten Formaten vorliegen, und hinsichtlich des 3D-Drucks sind nicht alle effizient. In Windows 10 wird der 3MF-Dateityp (3D Manufacturing Format) für alle 3D-Druckaufgaben verwendet.

> [!NOTE]  
> Der 3MF-Dateityp bietet mehr Funktionen als in diesem Tutorial behandelt werden. Weitere Informationen zu 3MF und den Features, die es den Herstellern und Consumern von 3D-Produkten bietet, finden Sie in der [3MF-Spezifikation](https://3mf.io/what-is-3mf/3mf-specification/). Informationen dazu, wie Sie diese Features mit Windows 10-APIs nutzen, finden Sie im Tutorial [Generieren eines 3MF-Pakets](./generate-3mf.md) .

Die [3D](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) -Generator-App kann Dateien der beliebtesten 3D-Formate öffnen und als 3MF-Dateien speichern. In diesem Beispiel, in dem der Dateityp variieren könnte, besteht eine sehr einfache Lösung darin, die 3D-Generator-APP zu öffnen und den Benutzer aufzufordern, die importierten Daten als 3MF-Datei zu speichern und dann neu zu laden.

> [!NOTE]  
> Neben dem Konvertieren von Dateiformaten bietet 3D Builder einfache Tools zum Bearbeiten von Modellen, Hinzufügen von Farbdaten und Ausführen anderer druckspezifischer Vorgänge. Daher empfiehlt sich häufig die Integration in eine App, mit der 3D-Druckvorgänge ausgeführt werden sollen.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Reparieren von Modelldaten für den 3D-Druck

Nicht alle 3D-Modelldaten sind druckbar. Dies gilt auch für den 3MF-Typ. Damit der Drucker richtig erkennen kann, welcher Raum zu füllen ist und welcher Raum frei gelassen werden muss, müssen die Modelle jeweils ein einzelnes nahtloses Gitter darstellen sowie nach außen weisende Oberflächennormalen und eine facettenreiche Geometrie aufweisen. Probleme in diesen Bereichen können in einer Vielzahl von verschiedenen Formularen auftreten und in komplexen Formen schwer zu erkennen sein. Moderne Softwarelösungen sind jedoch oft ausreichend, um die rohgeometrie in druckbare 3D-Formen umzuwandeln. Die wird als *Reparatur* des Modells bezeichnet und mit der `OnFixClick`-Methode erreicht.

Die 3D-Datendatei muss konvertiert werden, um [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) zu implementieren, womit anschließend ein [**Printing3DModel**](/uwp/api/Windows.Graphics.Printing3D.Printing3DModel)-Objekt generiert werden kann.

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Das **Printing3DModel**-Objekt wurde repariert und kann jetzt gedruckt werden. Weisen Sie das Modell mithilfe von [**SaveModelToPackageAsync**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) dem **Printing3D3MFPackage**-Objekt zu, das Sie beim Erstellen der Klasse deklariert haben.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Ausführen des Druckvorgangs: Erstellen eines TaskRequested-Handlers


Später, wenn das 3D-Druckdialogfeld für den Benutzer angezeigt wird und dieser einen Druckvorgang startet, muss die App die gewünschten Parameter an die 3D-Druckpipeline übergeben. Die 3D-Druck-API gibt das **[taskrequessierte](/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** -Ereignis aus. Sie müssen eine Methode schreiben, mit der dieses Ereignis richtig behandelt wird. Die Handlermethode muss wie üblich den gleichen Typ wie das zugeordnete Ereignis aufweisen: Das **TaskRequested**-Ereignis verfügt über [**Print3DManager**](/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (Verweis auf das Absenderobjekt) und ein [**Print3DTaskRequestedEventArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs)-Objekt als Parameter, in dem die meisten relevanten Informationen enthalten sind.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

Der grundlegende Zweck dieser Methode besteht darin, mit dem *args*-Parameter ein **Printing3D3MFPackage** entlang der Pipeline zu senden. Der **Print3DTaskRequestedEventArgs**-Typ verfügt über eine Eigenschaft: [**Request**](/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Sie ist vom Typ [**Print3DTaskRequest**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) und stellt eine Anforderung für den Druckauftrag dar. Die zugehörige Methode "up- [**eTask**](/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) " ermöglicht es dem Programm, die korrekten Informationen für den Druckauftrag zu übermitteln, und gibt einen Verweis auf das **Print3DTask** -Objekt zurück, das über die 3D-Druck Pipeline gesendet wurde.

" **Kreatetask** " verfügt über die folgenden Eingabeparameter: eine Zeichenfolge für den Druckauftrags Namen, eine Zeichenfolge für die ID des zu verwendenden Druckers und ein [**Print3DTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler) -Delegat. Der Delegat wird automatisch aufgerufen, wenn das **3DTaskSourceRequested**-Ereignis ausgelöst wird (dies erfolgt durch die API selbst). Zu beachten ist unbedingt, dass dieser Delegat beim Initiieren eines Druckauftrags aufgerufen wird. Er ist dafür zuständig, das richtige 3D-Druckpaket bereitzustellen.

**Print3DTaskSourceRequestedHandler** übernimmt einen Parameter, ein [**Print3DTaskSourceRequestedArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) -Objekt, das die zu sendenden Daten bereitstellt. Die öffentliche Methode dieser Klasse, [**SetSource**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), akzeptiert das zu druckende Paket. Implementieren Sie einen **Print3DTaskSourceRequestedHandler**-Delegaten wie folgt:

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Rufen Sie anschließend **CreateTask** mit dem neu definierten Delegaten `sourceHandler` auf:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

Der zurückgegebene **Print3DTask** wird der Klassenvariablen zugewiesen, die zu Beginn deklariert wurde. Mit diesem Verweis können Sie nun (optional) bestimmte Ereignisse behandeln, die von der Aufgabe ausgelöst wurden.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Sie müssen eine `Task_Submitting`-Methode und eine `Task_Completed`-Methode implementieren, wenn Sie sie für diese Ereignisse registrieren möchten.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Ausführen des Druckvorgangs: Öffnen des Dialogfelds für den 3D-Druck


Schließlich wird noch der Teil des Codes benötigt, mit dem das Dialogfeld für den 3D-Druck geöffnet wird. Wie bei einem herkömmlichen Druck Dialogfeld bietet das Dialogfeld 3D-Drucken eine Reihe von Druckoptionen in der letzten Minute und ermöglicht dem Benutzer die Auswahl des zu verwendenden Druckers (unabhängig davon, ob er über USB oder das Netzwerk verbunden ist).

Registrieren Sie die `MyTaskRequested`-Methode mit dem **TaskRequested**-Ereignis.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Nachdem Sie den **taskrequessierten** Ereignishandler registriert haben, können Sie die [**showprintuiasync**](/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync)-Methode aufrufen, die das 3D-Dialogfeld "Drucken" im aktuellen Anwendungsfenster aufruft.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Außerdem empfiehlt es sich noch, die Registrierung der Ereignishandler aufzuheben, nachdem die Steuerung von der App fortgesetzt wurde.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Zugehörige Themen

[Generieren eines 3MF-Pakets](./generate-3mf.md)  
[Beispiel für 3D-Druck – UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 