---
author: PatrickFarley
title: 3D-Drucken in der App
description: Erfahren Sie, wie Sie Ihrer universellen Windows-App 3D-Druckfunktionen hinzufügen. In diesem Thema wird erläutert, wie das 3D-Drucken-Dialogfeld aufgerufen wird, nachdem Sie sich vergewissert haben, dass das 3D-Modell gedruckt werden kann und im richtigen Format vorliegt.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, 3dprinting, 3d drucken
ms.localizationpriority: medium
ms.openlocfilehash: ae573fe87e6821555509467336e9a425fb082811
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "3985931"
---
# <a name="3d-printing-from-your-app"></a>3D-Druck über Ihre App

**Wichtige APIs**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Erfahren Sie, wie Sie Ihrer Universellen Windows-App 3D-Druckfunktionen hinzufügen. In diesem Thema wird erläutert, wie Sie 3D-Geometriedaten in Ihre App laden und das 3D-Druckdialogfeld starten, nachdem Sie sichergestellt haben, dass Ihr 3D-Modell druckbar ist und das richtige Format hat. Ein Beispiel wie dieses Verfahren funktioniert, finden Sie unter [Beispiel für 3D-Druck– UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> Im Beispielcode in dieser Anleitung wurde das Melden und Verarbeiten von Fehlern stark vereinfacht, um den Code möglichst einfach zu halten.

## <a name="setup"></a>Setup


Fügen Sie in der Anwendungsklasse, der 3D-Druckfunktionen zugewiesen werden sollen, den [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)-Namespace hinzu.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

In dieser Anleitung werden die folgenden zusätzlichen Namespaces verwendet.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Weisen Sie der Klasse anschließend hilfreiche Memberfelder zu. Deklarieren Sie ein [**Print3DTask**](https://msdn.microsoft.com/library/windows/apps/dn998044)-Objekt so, dass es die Druckaufgabe repräsentiert, die an den Druckertreiber übergeben werden soll. Deklarieren Sie ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt für die ursprüngliche 3D-Datendatei, die in die App geladen werden soll. Deklarieren Sie ein [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/dn998063)-Objekt, das ein 3D-Modell mit allen erforderlichen Metadaten darstellt und gedruckt werden kann.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Erstellen einer einfachen UI

In diesem Beispiel werden drei Benutzersteuerelemente verwendet: eine Schaltfläche zum Laden, die eine Datei im Programmspeicher ablegt, eine Schaltfläche zum Optimieren, mit der die Datei ggf. geändert wird, sowie eine Schaltfläche zum Drucken, über die der Druckauftrag initiiert wird. Mit dem folgenden Code werden diese Schaltflächen (mit den zugehörigen per-Click-Ereignishandlern) in der XAML-Datei Ihrer Klasse erstellt:

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Hinzufügen eines **TextBlock**-Elements für UI-Feedback:

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Abrufen der 3D-Daten


Die Methode, mit der Ihre App 3D-Geometriedaten abruft, kann variieren. Die App kann Daten aus einem 3D-Scan abrufen, Modelldaten in einem Pull-Vorgang aus einer Webressource herunterladen oder programmgesteuert mithilfe von mathematischen Gleichungen oder Benutzereingaben ein 3D-Gitter generieren. Der Einfachheit halber wird in dieser Anleitung gezeigt, wie Sie eine 3D-Datendatei (mit einem beliebigen gängigen Dateityp) aus dem Gerätespeicher in den Programmspeicher laden. Die [Modellbibliothek für 3D Builder](https://developer.microsoft.com/windows/hardware/3d-builder-model-library) enthält verschiedene Modelle, die Sie leicht auf Ihr Gerät herunterladen können.

Laden Sie in der `OnLoadClick`-Methode mit der [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse eine einzelne Datei in den Speicher der App.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Konvertieren in das 3D Manufacturing Format (3MF) mithilfe von 3D Builder

An dieser Stelle können Sie eine 3D-Datendatei in den Speicher der App laden. 3D-Geometriedaten können aber in den unterschiedlichsten Formaten vorliegen, und hinsichtlich des 3D-Drucks sind nicht alle effizient. In Windows 10 wird der 3MF-Dateityp (3D Manufacturing Format) für alle 3D-Druckaufgaben verwendet.

> [!NOTE]  
> Der 3MF-Dateityp unterstützt viele Funktionen, die in dieser Anleitung behandelt werden. Weitere Informationen zu 3MF und den Features des Formats für Hersteller und Verbraucher von 3D-Produkten finden Sie in der [3MF-Spezifikation](http://3mf.io/what-is-3mf/3mf-specification/). Wie Sie diese Features mithilfe von Windows 10-APIs nutzen, finden Sie im Lernprogramm [Generieren ein 3MF-Pakets](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

Die [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6)-App Dateien kann gängigste 3D-Formate öffnen und diese als 3MF-Dateien speichern. In diesem Beispiel mit variierendem Dateityp besteht eine sehr einfache Lösung darin, dass 3D Builder-App geöffnet und der Benutzer aufgefordert wird, die importierten Daten als 3MF-Datei zu speichern und anschließend neu zu laden.

> [!NOTE]  
> Neben dem Konvertieren von Dateiformaten bietet 3D Builder einfache Tools zum Bearbeiten von Modellen, Hinzufügen von Farbdaten und Ausführen anderer druckspezifischer Vorgänge. Daher empfiehlt sich häufig die Integration in eine App, mit der 3D-Druckvorgänge ausgeführt werden sollen.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Reparieren von Modelldaten für den 3D-Druck

Nicht alle 3D-Modelldaten sind druckbar. Dies gilt auch für den 3MF-Typ. Damit der Drucker richtig erkennen kann, welcher Raum zu füllen ist und welcher Raum frei gelassen werden muss, müssen die Modelle jeweils ein einzelnes nahtloses Gitter darstellen sowie nach außen weisende Oberflächennormalen und eine facettenreiche Geometrie aufweisen. Probleme in diesen Bereichen können in den unterschiedlichsten Formen auftreten und sind bei komplexen Formen unter Umständen nur schwer zu erkennen. Moderne Softwarelösungen sind häufig in der Lage, Geometrie-Rohdaten in druckbare 3D-Formen zu konvertieren. Die wird als *Reparatur* des Modells bezeichnet und mit der `OnFixClick`-Methode erreicht.

Die 3D-Datendatei muss konvertiert werden, um [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) zu implementieren, womit anschließend ein [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/mt203679)-Objekt generiert werden kann.

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Das **Printing3DModel**-Objekt wurde repariert und kann jetzt gedruckt werden. Weisen Sie das Modell mithilfe von [**SaveModelToPackageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) dem **Printing3D3MFPackage**-Objekt zu, das Sie beim Erstellen der Klasse deklariert haben.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Ausführen des Druckvorgangs: Erstellen eines TaskRequested-Handlers


Später, wenn das 3D-Druckdialogfeld für den Benutzer angezeigt wird und dieser einen Druckvorgang startet, muss die App die gewünschten Parameter an die 3D-Druckpipeline übergeben. Die 3D-Druck-API löst das **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)**-Ereignis aus. Sie müssen eine Methode schreiben, mit der dieses Ereignis richtig behandelt wird. Die Handlermethode muss wie üblich den gleichen Typ wie das zugeordnete Ereignis aufweisen: Das **TaskRequested**-Ereignis verfügt über [**Print3DManager**](https://msdn.microsoft.com/library/windows/apps/dn998029) (Verweis auf das Absenderobjekt) und ein [**Print3DTaskRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn998051)-Objekt als Parameter, in dem die meisten relevanten Informationen enthalten sind.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

Der grundlegende Zweck dieser Methode besteht darin, mit dem *args*-Parameter ein **Printing3D3MFPackage** entlang der Pipeline zu senden. Der **Print3DTaskRequestedEventArgs**-Typ verfügt über eine Eigenschaft: [**Request**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequestedeventargs.request.aspx). Diese hat den Typ [**Print3DTaskRequest**](https://msdn.microsoft.com/library/windows/apps/dn998050) und stellt eine Druckauftrag-Anforderung dar. Die zugehörige [**CreateTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequest.createtask.aspx)-Methode ermöglicht es dem Programm, die richtigen Informationen für den Druckauftrag zu senden. Es wird ein Verweis auf das **Print3DTask**-Objekt zurückgegeben, das an die 3D-Druckpipeline gesendet wurde.

**CreateTask** weist die folgenden Eingabeparameter auf: einen string für den Namen des Druckauftrags, einen string für die ID des zu verwendenden Druckers sowie einen [**Print3DTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedhandler.aspx)-Delegaten. Der Delegat wird automatisch aufgerufen, wenn das **3DTaskSourceRequested**-Ereignis ausgelöst wird (dies erfolgt durch die API selbst). Zu beachten ist unbedingt, dass dieser Delegat beim Initiieren eines Druckauftrags aufgerufen wird. Er ist dafür zuständig, das richtige 3D-Druckpaket bereitzustellen.

**Print3DTaskSourceRequestedHandler** akzeptiert einen Parameter, und zwar ein [**Print3DTaskSourceRequestedArgs**](https://msdn.microsoft.com/library/windows/apps/dn998056)-Objekt, das die zu sendenden Daten angibt. Die öffentliche Methode dieser Klasse, [**SetSource**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource.aspx), akzeptiert das zu druckende Paket. Implementieren Sie einen **Print3DTaskSourceRequestedHandler**-Delegaten wie folgt:

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Rufen Sie anschließend **CreateTask** mit dem neu definierten Delegaten `sourceHandler` auf:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

Der zurückgegebene **Print3DTask** wird der Klassenvariablen zugewiesen, die zu Beginn deklariert wurde. Mit diesem Verweis können Sie nun (optional) bestimmte Ereignisse behandeln, die von der Aufgabe ausgelöst wurden.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Sie müssen eine `Task_Submitting`-Methode und eine `Task_Completed`-Methode implementieren, wenn Sie sie für diese Ereignisse registrieren möchten.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Ausführen des Druckvorgangs: Öffnen des Dialogfelds für den 3D-Druck


Schließlich wird noch der Teil des Codes benötigt, mit dem das Dialogfeld für den 3D-Druck geöffnet wird. Wie bei einem herkömmlichen Dialogfeld für das Drucken enthält ein Dialogfeld für den 3D-Druck eine Reihe von Druckoptionen, die direkt vor dem Druckvorgang festgelegt werden können. Zudem kann der Benutzer den gewünschten Drucker auswählen (einen per USB angeschlossenen Drucker oder einen Netzwerkdrucker).

Registrieren Sie die `MyTaskRequested`-Methode mit dem **TaskRequested**-Ereignis.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Nach dem Registrieren des **TaskRequested**-Ereignishandlers können Sie die [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dmanager.showprintuiasync.aspx)-Methode aufrufen, die das Dialogfeld für den 3D-Druck im aktuellen Anwendungsfenster öffnet.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Außerdem empfiehlt es sich noch, die Registrierung der Ereignishandler aufzuheben, nachdem die Steuerung von der App fortgesetzt wurde.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Verwandte Themen

[Generieren eines 3MF-Pakets](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Beispiel für 3D-Druck– UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
