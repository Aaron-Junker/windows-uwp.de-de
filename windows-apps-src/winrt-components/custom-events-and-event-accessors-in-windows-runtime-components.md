---
title: Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime
description: Die .NET-Unterstützung für Windows-Runtime-Komponenten erleichtert das Deklarieren von Ereignis Komponenten, indem die Unterschiede zwischen dem universelle Windows-Plattform (UWP)-Ereignis Muster und dem .NET-Ereignis Muster ausgeblendet werden.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4180fc0f3b949ddf231df8b839831c70ce44d634
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393716"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime

Die .NET-Unterstützung für Windows-Runtime-Komponenten erleichtert das Deklarieren von Ereignis Komponenten, indem die Unterschiede zwischen dem universelle Windows-Plattform (UWP)-Ereignis Muster und dem .NET-Ereignis Muster ausgeblendet werden. Wenn Sie jedoch benutzerdefinierte Ereignisaccessoren in einer Windows-Runtime Komponente deklarieren, müssen Sie dem in der UWP verwendeten Muster folgen.

## <a name="registering-events"></a>Registrieren von Ereignissen

Wenn Sie die Registrierung für die Behandlung eines Ereignisses in der UWP durchführen, gibt der Add-Accessor ein Token zurück. Um die Registrierung aufzuheben, übergeben Sie dieses Token an den Remove-Accessor. Dies bedeutet, dass sich die Signaturen der Add- und Remove-Accessoren für UWP-Ereignisse von den üblichen Accessoren unterscheiden.

Glücklicherweise vereinfachen der Visual Basic und C# die Compiler diesen Prozess: Wenn Sie ein Ereignis mit benutzerdefinierten Accessoren in einer Windows-Runtime Komponente deklarieren, verwenden die Compiler automatisch das UWP-Muster. Beispielsweise erhalten Sie einen Compilerfehler, wenn Ihr Add-Accessor kein Token zurückgibt. .Net bietet zwei Typen zur Unterstützung der-Implementierung:

-   Die [EventRegistrationToken](https://docs.microsoft.com/uwp/api/windows.foundation.eventregistrationtoken)-Struktur stellt das Token dar.
-   Die [EventRegistrationTokenTable&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1?redirectedfrom=MSDN)-Klasse erstellt Token und verwaltet eine Zuordnung zwischen Token und Ereignishandlern. Das generische Typargument ist der Ereignisargumenttyp. Sie erstellen eine Instanz dieser Klasse für jedes Ereignis, wenn ein Ereignishandler zum ersten Mal für dieses Ereignis registriert wird.

Der folgende Code für das Ereignis „NumberChanged” zeigt das grundlegende Muster für UWP-Ereignisse. In diesem Beispiel übernimmt der Konstruktor für das Ereignisargumentobjekt „NumberChangedEventArgs” einen einzelnen ganzzahligen Parameter, der den geänderten numerischen Wert darstellt.

> **Beachten Sie**  , dass dies das gleiche Muster ist, das die Compiler für normale Ereignisse verwenden, die Sie in einer Windows-Runtime Komponente deklarieren.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

Die statische („Shared” in Visual Basic) Methode „GetOrCreateEventRegistrationTokenTable” erstellt die Instanz des Ereignisses des EventRegistrationTokenTable&lt;T&gt;-Objekts verzögert. Übergeben Sie das Feld auf Klassenebene, das die Tokentabelleninstanz enthalten soll, an diese Methode. Wenn das Feld leer ist, erstellt die Methode die Tabelle, speichert einen Verweis auf die Tabelle in dem Feld und gibt einen Verweis auf die Tabelle zurück. Wenn das Feld bereits einen Verweis auf die Tokentabelle enthält, gibt die Methode einfach diesen Verweis zurück.

> Wichtig  um die Thread Sicherheit sicherzustellen, muss das Feld, das die Ereignis Instanz von eventregistrationdekentable&lt;T&gt; enthält, ein Feld auf Klassenebene sein. Wenn dies ein Feld auf Klassenebene ist, stellt die Methode „GetOrCreateEventRegistrationTokenTable” sicher, dass alle Threads dieselbe Instanz der Tabelle abrufen, wenn mehrere Threads versuchen, die Tokentabelle zu erstellen. Für ein bestimmtes Ereignis müssen alle Aufrufe der Methode „GetOrCreateEventRegistrationTokenTable” dasselbe Feld auf Klassenebene verwenden.

Durch Aufrufen der Methode „GetOrCreateEventRegistrationTokenTable” im Remove-Accessor und in der Methode [RaiseEvent](https://docs.microsoft.com/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) („OnRaiseEvent”-Methode in C#) wird sichergestellt, dass keine Ausnahmen ausgelöst werden, wenn diese Methoden aufgerufen werden, bevor Ereignishandlerdelegaten hinzugefügt wurden.

Zu den anderen Membern der EventRegistrationTokenTable&lt;T&gt;-Klasse, die im UWP-Ereignismuster verwendet werden, zählen:

-   Die [AddEventHandler](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler?redirectedfrom=MSDN#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_)-Methode generiert ein Token für den Ereignishandlerdelegaten, speichert den Delegaten in der Tabelle, fügt ihn der Aufrufliste hinzu und gibt das Token zurück.
-   Die [RemoveEventHandler(EventRegistrationToken)](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler?redirectedfrom=MSDN#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_)-Methodenüberladung entfernt den Delegaten aus der Tabelle und der Aufrufliste.

    >**Beachten Sie**  , dass die AddEventHandler-Methode und die RemoveEventHandler-Methode (eventregistrationtoken) die Tabelle sperren, um die Thread Sicherheit zu gewährleisten.

-   Die [InvocationList](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist?redirectedfrom=MSDN#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList)-Eigenschaft gibt einen Delegaten zurück, der alle Ereignishandler enthält, die derzeit zum Behandeln des Ereignisses registriert sind. Verwenden Sie diesen Delegaten zum Auslösen des Ereignisses, oder verwenden Sie die Methoden der Delegate-Klasse, um die Handler einzeln aufzurufen.

    >**Beachten Sie,** dass Sie das im Beispiel weiter oben in diesem Artikel angegebene Muster befolgen und den Delegaten vor dem Aufrufen in eine temporäre Variable kopieren.   Dadurch wird eine Racebedingung verhindert, in der ein Thread den letzten Ereignishandler entfernt, indem der Delegat auf null festlegt wird, kurz bevor ein anderer Thread versucht, den Delegaten aufzurufen. Delegaten sind unveränderlich, sodass die Kopie weiterhin gültig ist.

Nehmen Sie eigenen Code nach Bedarf in die Accessoren auf. Wenn Threadsicherheit wichtig ist, müssen Sie eigene Sperren für Ihren Code vorsehen.

C#Nutzers Wenn Sie benutzerdefinierte Ereignisaccessoren im UWP-Ereignis Musterschreiben, stellt der Compiler nicht die üblichen syntaktischen Verknüpfungen bereit. Es werden Fehler generiert, wenn Sie den Namen des Ereignisses im Code verwenden.

Visual Basic Benutzer: In .net ist ein Ereignis lediglich ein Multicast Delegat, der alle registrierten Ereignishandler darstellt. Das Auslösen des Ereignisses bedeutet nur, dass der Delegat aufgerufen wird. Visual Basic-Syntax blendet im Allgemeinen die Interaktionen mit dem Delegaten aus, und der Compiler kopiert den Delegaten, bevor er aufgerufen wird, wie im Hinweis zu Threadsicherheit beschrieben. Wenn Sie ein benutzerdefiniertes Ereignis in einer Windows-Runtime Komponente erstellen, müssen Sie den Delegaten direkt behandeln. Dies bedeutet auch, dass Sie beispielsweise mit der [MulticastDelegate.GetInvocationList](https://docs.microsoft.com/dotnet/api/system.multicastdelegate.getinvocationlist?redirectedfrom=MSDN#System_MulticastDelegate_GetInvocationList)-Methode ein Array mit einem separaten Delegaten für jeden Ereignishandler abrufen können, wenn Sie die Handler einzeln aufrufen möchten.

## <a name="related-topics"></a>Verwandte Themen

* [Ereignisse (Visual Basic)](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [Ereignisse (C# Programmier Handbuch)](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/events/index)
* [Übersicht über .net für UWP-apps](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [.Net für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Exemplarische Vorgehensweise zum C# Erstellen eines-oder-Visual Basic Windows-Runtime Komponente und zum Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
