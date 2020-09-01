---
title: Auslösen von Ereignissen in Komponenten für Windows-Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Gibt an, wie ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrund Thread ausgegeben wird, damit JavaScript das Ereignis empfangen kann.
ms.date: 07/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b44e4ba86ab96474d4770c32024b8edc5641c396
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174284"
---
# <a name="raising-events-in-windows-runtime-components"></a>Auslösen von Ereignissen in Komponenten für Windows-Runtime

> [!NOTE]
> Weitere Informationen zum Erstellen von Ereignissen in einer [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) -Komponente Windows-Runtime finden Sie unter [Verfassen von Ereignissen in C++/WinRT](../cpp-and-winrt-apis/author-events.md).

Wenn die Windows-Runtime Komponente ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrund Thread (Arbeits Thread) auslöst und JavaScript in der Lage sein soll, das Ereignis zu empfangen, können Sie diese implementieren und/oder auf eine beliebige Weise auslösen.

-   (Option 1) Rufen Sie das-Ereignis über die [**Windows. UI. Core. coredispatcher**](/uwp/api/windows.ui.core.coredispatcher) -Methode auf, um das Ereignis in den JavaScript-Thread Kontext zu Mars Hallen. Dies ist in der Regel zwar die beste Option, erzielt in manchen Szenarien aber möglicherweise nicht die schnellste Leistung.
-   (Option 2) Verwenden Sie ** [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler-1) \<Object\> ** (verliert jedoch die Ereignistyp Informationen). Wenn Option 1 nicht möglich ist oder die Leistung nicht ausreichend ist, ist dies eine gute zweite Wahl, vorausgesetzt, dass der Verlust von Typinformationen akzeptabel ist. Wenn Sie eine c#-Windows-Runtime Komponente erstellen, ist der **Windows. Foundation. EventHandler \<Object\> ** -Typ nicht verfügbar. stattdessen wird dieser Typ in [**System. EventHandler**](/dotnet/api/system.eventhandler)projiziert, daher sollten Sie ihn stattdessen verwenden.
-   (Option 3) Erstellen Sie einen eigenen Proxy und Stub für die Komponente. Diese Option ist am schwierigsten zu implementieren, behält aber die Typinformationen bei und erzielt in anspruchsvollen Szenarien unter Umständen eine bessere Leistung als Option 1.

Wenn Sie nur ein Ereignis in einem Hintergrundthread ohne eine dieser Optionen auslösen, wird das Ereignis von einem JavaScript-Client nicht empfangen.

## <a name="background"></a>Hintergrund

Bei allen Komponenten und Apps für Windows-Runtime handelt es sich im Grunde um COM-Objekte, unabhängig davon, welche Sprache Sie bei ihrer Erstellung verwenden. In der Windows-API sind die meisten Komponenten agile COM-Objekte, die mit Objekten im Hintergrundthread und im UI-Thread gleichermaßen gut kommunizieren können. Wenn ein COM-Objekt nicht agil angelegt werden kann, sind Hilfsobjekte (auch als Proxys bzw. Stubs bezeichnet) erforderlich, um mit anderen COM-Objekten über die Threadgrenze des UI-Threadhintergrunds hinweg zu kommunizieren. (In der COM-Terminologie wird dies als Kommunikation zwischen Threadapartments bezeichnet.)

Die meisten Objekte in der Windows-API sind entweder agil oder verfügen über integrierte Proxys und Stubs. Allerdings können Proxys und Stubs nicht für generische Typen wie Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler) erstellt werden, da sie erst vollständige Typen sind, wenn das Typargument bereitgestellt ist. Fehlende Proxys oder Stubs stellen nur bei JavaScript-Clients ein Problem dar. Wenn die Komponente aber sowohl in JavaScript als auch in C++ oder einer .NET-Sprache verwendet werden soll, müssen Sie eine der drei folgenden Optionen verwenden.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>Option 1) Auslösen des Ereignisses mit dem CoreDispatcher-Ereignis

Sie können Ereignisse eines benutzerdefinierten Delegattyps mit [Windows.UI.Core.CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) senden, und JavaScript kann diese Ereignisse empfangen. Wenn Sie nicht sicher sind, welche Option Sie verwenden sollen, versuchen Sie es zunächst mit dieser ersten Option. Wenn die Wartezeit zwischen dem Auslösen von Ereignissen und der Ereignisbehandlung ein Problem darstellt, versuchen Sie es mit einer der anderen Optionen.

Im folgenden Beispiel wird gezeigt, wie der CoreDispatcher verwendet wird, um ein stark typisiertes Ereignis auszulösen. Beachten Sie, dass es sich bei dem Typargument um „Toast“, und nicht um „Object“ handelt.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(Option 2) Verwenden von EventHandler&lt;Object&gt;, allerdings mit Verlust von Typinformationen

> [!NOTE]
> Wenn Sie eine c#-Windows-Runtime Komponente erstellen, ist der **Windows. Foundation. EventHandler \<Object\> ** -Typ nicht verfügbar. stattdessen wird dieser Typ in [**System. EventHandler**](/dotnet/api/system.eventhandler)projiziert, daher sollten Sie ihn stattdessen verwenden.

Eine weitere Möglichkeit zum Senden eines Ereignisses aus einem Hintergrund Thread besteht darin, das [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler)- &lt; Objekt &gt; als Typ des Ereignisses zu verwenden. Windows instanziiert den generischen Typ konkret und stellt dafür einen Proxy und einen Stub bereit. Der Nachteil besteht darin, dass die Typinformationen der Ereignisargumente und des Senders verloren gehen. C++- und .NET-Clients müssen aus der Dokumentation entnehmen können, welcher Typ in den ursprünglichen Typ umgewandelt werden muss, wenn das Ereignis empfangen wird. JavaScript-Clients benötigt keine Informationen über die ursprünglichen Typen. Sie finden die Argumenteigenschaften anhand ihrer Namen in den Metadaten.

In diesem Beispiel wird gezeigt, wie Windows.Foundation.EventHandler&lt;Object&gt; in C# verwendet wird:

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Sie verwenden dieses Ereignis auf der JavaScript-Seite wie folgt:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(Option 3) Erstellen eines eigenen Proxys und Stubs

Um bei benutzerdefinierten Ereignistypen mit vollständig beibehaltenen Typinformationen potenzielle Leistungszuwächse zu erreichen, müssen Sie eigene Proxy- und Stub-Objekte erstellen und diese in das App-Paket einbetten. In der Regel wird diese Option nur selten verwendet und zwar, wenn keine der beiden anderen Optionen geeignet ist. Außerdem ist nicht gewährleistet, dass mit dieser Option eine bessere Leistung erzielt wird als mit den anderen beiden Optionen. Die tatsächliche Leistung hängt von vielen Faktoren ab. Verwenden Sie den Visual Studio-Profiler oder andere Profilerstellungstools, um die tatsächliche Leistung der Anwendung zu messen und um festzustellen, ob das Ereignis tatsächlich einen Engpass darstellt.

Im weiteren Verlauf dieses Artikels wird gezeigt, wie eine grundlegende Komponente für Windows-Runtime in C# und anschließend eine DLL für Proxy und Stub in C++ erstellt werden, sodass JavaScript ein Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;-Ereignis nutzen kann, das von der Komponente in einem asynchronen Vorgang ausgelöst wird. (Sie können die Komponente auch in C++ oder Visual Basic erstellen. Die Schritte zum Erstellen der Proxys und Stubs sind identisch.) Diese exemplarische Vorgehensweise basiert auf dem Artikel „Erstellen einer prozessinternen Komponente für Windows-Runtime (C++/CX) – Beispiel”, dessen Zielsetzungen näher erläutert werden.

Diese exemplarische Vorgehensweise enthält diese Teile.

-   Hier erstellen Sie zwei Windows-Runtime-Basisklassen. Die eine Klasse macht ein Ereignis vom Typ [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler-2) verfügbar, und die andere Klasse ist der Typ, der als Argument für TValue an JavaScript zurückgegeben wird. Diese Klassen können erst mit JavaScript kommunizieren, wenn Sie die nachfolgenden Schritten ausgeführt haben.
-   Diese App aktiviert das Hauptklassenobjekt, ruft eine Methode auf und behandelt ein Ereignis, das von der Komponente für Windows-Runtime ausgelöst wird.
-   Diese werden von den Tools benötigt, die die Proxy- und Stubklassen generieren.
-   Dann wird mithilfe der IDL-Datei der C-Quellcode für den Proxy und den Stub generiert.
-   Registrieren Sie die Proxy-Stub-Objekte, damit die COM-Laufzeitumgebung sie finden kann, und verweisen Sie im App-Projekt auf die Proxy-Stub-DLL-Datei.

## <a name="to-create-the-windows-runtime-component"></a>So erstellen Sie die Komponente für Windows-Runtime

Wählen Sie in Visual Studio in der Menüleiste **Datei &gt; neu Projekt**aus. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **JavaScript &gt; Universal Windows**, und wählen Sie dann **Leere App** aus. Geben Sie als Projektnamen „ToasterApplication“ ein, und klicken Sie dann auf die Schaltfläche **OK**.

Fügen Sie der Projektmappe eine C#-Komponente für Windows-Runtime hinzu: Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Hinzufügen &gt; Neues Projekt** aus. Erweitern Sie **Visual c# &gt; Microsoft Store** , und wählen Sie dann **Windows-Runtime Komponente**aus. Geben Sie als Projektnamen „ToasterComponent“ ein, und klicken Sie dann auf die Schaltfläche **OK**. ToasterComponent ist der Stammnamespace für die Komponenten, die Sie in späteren Schritten erstellen.

Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Eigenschaften** aus. Wählen Sie im Dialogfeld **Eigenschaftenseiten** im linken Bereich **Konfigurationseigenschaften** aus, und legen Sie dann oben im Dialogfeld die **Konfiguration** auf **Debuggen** und die **Plattform** auf „x86”, „x64” oder „ARM” fest. Klicken Sie auf die Schaltfläche **OK** .

**Wichtig**   Platform = Any CPU funktioniert nicht, da Sie für die Win32-DLL mit System eigenem Code, die Sie später der Projekt Mappe hinzufügen, nicht gültig ist.

Benennen Sie im Projektmappen-Explorer die Datei „class1.cs” in „ToasterComponent.cs” um, sodass sie dem Namen des Projekts entspricht. Visual Studio benennt die Klasse in der Datei entsprechend dem neuen Dateinamen automatisch um.

Fügen Sie der CS-Datei eine using-Direktive für den Namespace Windows.Foundation hinzu, um TypedEventHandler in den Gültigkeitsbereich einzubinden.

Wenn Sie Proxys und Stubs benötigen, muss die Komponente mit Schnittstellen ihre öffentlichen Member verfügbar machen. Definieren Sie in „ToasterComponent.cs” eine Schnittstelle für den Toaster und eine andere für den „Toast” (Popup), den der Toaster erzeugt.

**Hinweis**   In c# können Sie diesen Schritt überspringen. Erstellen Sie stattdessen zuerst eine Klasse, öffnen Sie das Kontextmenü, und wählen Sie **Umgestalten &gt; Schnittstelle extrahieren** aus. Ordnen Sie den Schnittstellen im generierten Code manuell öffentlichen Zugriff zu.

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

Die IToast-Schnittstelle hat eine Zeichenfolge, die abgerufen werden kann, um den Typ des Popups zu beschreiben. Die IToaster-Schnittstelle verfügt eine Methode, um das Popup zu erstellen, und über ein Ereignis, das angibt, dass das Popup erstellt wird. Da dieses Ereignis einen bestimmten Teil (d. h. den Typ) des Popups zurückgibt, wird es als typisiertes Ereignis bezeichnet.

Als Nächstes müssen Klassen angelegt werden, die diese Schnittstellen implementieren. Diese Klassen müssen öffentlich und versiegelt sein, damit die JavaScript-App, die Sie später programmieren, darauf zugreifen kann.

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

Im vorhergehenden Code wird das Popup erstellt und ein Arbeitselement im Threadpool ausgeführt, um die Benachrichtigung auszulösen. Auch wenn die IDE vorschlagen sollte, das „await“-Schlüsselwort dem asynchronen Aufruf zuzuweisen, ist dies in diesem Fall nicht nötig, da die Methode keine Aktionen ausführt, die von den Ergebnissen des Vorgangs abhängig sind.

**Hinweis**   Der Async-Befehl im vorangehenden Code verwendet Thread Pool. runasync ausschließlich, um eine einfache Möglichkeit zum Auslösen des Ereignisses in einem Hintergrund Thread zu veranschaulichen. Sie könnten diese spezielle Methode auch schreiben wie im folgenden Beispiel gezeigt. Dies funktioniert, da der .NET-Taskplaner automatisch „async/await“-Aufrufe zurück an den UI-Thread marshallt.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Wenn Sie das Projekt jetzt erstellen, sollte es ordnungsgemäß erstellt werden.

## <a name="to-program-the-javascript-app"></a>So programmieren Sie die JavaScript-App

Jetzt können Sie eine Schaltfläche zur JavaScript-App hinzufügen, sodass diese die Klasse verwendet, die soeben zum Erstellen des Toasts definiert wurde. Vorher muss ein Verweis zum ToasterComponent-Projekt hinzugefügt werden, das soeben erstellt wurde. Öffnen Sie in Projektmappen-Explorer das Kontextmenü für das toasterapplication-Projekt, wählen Sie ** &gt; Verweise hinzufügen**aus, und wählen Sie dann die Schaltfläche **neuen Verweis hinzufügen** aus. Wählen Sie im Dialogfeld Verweis hinzufügen links unter Projektmappe das Komponentenprojekt aus, und wählen Sie dann in der Mitte "ToasterComponent" aus. Klicken Sie auf die Schaltfläche **OK** .

Öffnen Sie in Projektmappen-Explorer das Kontextmenü für das toasterapplication-Projekt, und wählen Sie dann **als Startprojekt festlegen**aus.

Fügen Sie am Ende der default.js-Datei einen Namespace hinzu, der die Funktionen zum Aufrufen der Komponente und zum Zurückrufen durch diese Komponente enthält. Der Namespace hat zwei Funktionen: eine zum Erstellen des Toasts und eine zum Behandeln des Ereignisses zum Abschließen des Toasts. Die Implementierung von maketoast erstellt ein Toaster-Objekt, registriert den Ereignishandler und erstellt den Toast. Bisher hatte der Ereignishandler nicht viele Funktionen, wie hier gezeigt:

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

Die maketoast-Funktion muss mit einer Schaltfläche verknüpft werden. Aktualisieren Sie „default.html“, sodass eine Schaltfläche und Leerzeichen eingefügt werden, um das Ergebnis der Toasterstellung auszugeben:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Wenn wir keinen typedeventhandler verwendet haben, können wir die App nun auf dem lokalen Computer ausführen und auf die Schaltfläche klicken, um den Toast zu erstellen. In dieser App passiert jedoch nichts. Um herauszufinden, warum Sie den verwalteten Code Debuggen, der das "deastcompletedevent" auslöst. Wählen Sie das Projekt aus, und wählen Sie dann in der Menüleiste **Debuggen von &gt; Toaster-Anwendungseigenschaften**aus. Ändern Sie den **Debuggertyp** in **nur verwaltet**. Klicken Sie in der Menüleiste auf ** &gt; Ausnahmen Debuggen**und dann auf **Common Language Runtime-Ausnahmen**.

Führen Sie nun die App aus, und klicken Sie auf die Schaltfläche zum Erstellen des Toasts. Der Debugger fängt eine ungültige Umwandlungsausnahme ab. Auch wenn dies nicht aus der Meldung ersichtlich ist, wird diese Ausnahme ausgelöst, weil Proxys für diese Schnittstelle fehlen.

![fehlender Proxy](./images/debuggererrormissingproxy.png)

Der erste Schritt zum Erstellen eines Proxys und Stubs für eine Komponente besteht darin, eine eindeutige ID oder GUID zu den Schnittstellen hinzuzufügen. Das zu verwendende GUID-Format hängt jedoch davon ab, ob Sie in C#, Visual Basic, einer anderen .NET-Sprache oder in C++ codieren.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>So generieren Sie GUIDs für die Schnittstellen der Komponente (c# und andere .NET-Sprachen)

Wählen Sie in der Menüleiste Extras &gt; GUID erstellen aus. Wählen Sie im Dialogfeld die Option 5 aus. \[GUID ("xxxxxxxx-xxxx... xxxx ") \] . Wählen Sie die Schaltfläche Neue GUID aus, und wählen Sie dann die Schaltfläche Kopieren aus.

![GUID Generator-Tool](./images/guidgeneratortool.png)

Wechseln Sie zurück zur Schnittstellen Definition, und fügen Sie die neue GUID direkt vor der itoaster-Schnittstelle ein, wie im folgenden Beispiel gezeigt. (Verwenden Sie nicht die im Beispiel genannte GUID. Jede eindeutige Schnittstelle sollte eine eigene GUID aufweisen.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Fügen Sie eine using-Direktive für den System. Runtime. InteropServices-Namespace hinzu.

Wiederholen Sie diese Schritte für die itoast-Schnittstelle.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>So generieren Sie GUIDs für die Schnittstellen der Komponente (C++)

Wählen Sie in der Menüleiste Extras &gt; GUID erstellen aus. Wählen Sie im Dialogfeld 3 aus. statische Konstante Struktur-Guid = {...}. Wählen Sie die Schaltfläche Neue GUID aus, und wählen Sie dann die Schaltfläche Kopieren aus.

Fügen Sie die GUID direkt vor der itoaster-Schnittstellen Definition ein. Nach dem Einfügen sollte die GUID dem folgenden Beispiel entsprechen. (Verwenden Sie nicht die im Beispiel genannte GUID. Jede eindeutige Schnittstelle sollte eine eigene GUID aufweisen.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Fügen Sie eine using-Direktive für Windows. Foundation. Metadata hinzu, um GuidAttribute in den Bereich zu bringen.

Konvertieren Sie nun die GUID mit Konstruktor manuell in ein GuidAttribute, sodass diese wie im folgenden Beispiel gezeigt formatiert wird. Beachten Sie, dass die geschweiften Klammern durch runde und eckige Klammern ersetzt werden und das nachfolgende Semikolon entfernt wird.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Wiederholen Sie diese Schritte für die itoast-Schnittstelle.

Da die Schnittstellen nun über eindeutige IDs verfügen, können Sie eine IDL-Datei erstellen, indem Sie die winmd-Datei in das winmdidl-Befehlszeilen Tool einreihen und dann den C-Quellcode für den Proxy und den Stub generieren, indem Sie diese IDL-Datei in das Befehlszeilen Tool "Mittel l" übertragen. Visual Studio führt diese Aktion aus, wenn wie in den nachfolgenden Schritten Postbuildereignisse erstellt werden.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>So generieren Sie den Proxy- und Stubquellcode

Um ein benutzerdefiniertes Postbuildereignis hinzuzufügen, öffnen Sie im Projektmappen-Explorer das Kontextmenü des ToasterComponent-Projekts, und wählen Sie dann Eigenschaften aus. Wählen Sie links auf den Eigenschaftenseiten Buildereignisse aus, und wählen Sie dann die Schaltfläche Postbuild bearbeiten aus. Fügen Sie die folgenden Befehle zur Postbuildbefehlszeile hinzu. (Die Batchdatei muss zuerst aufgerufen werden, um die Umgebungsvariablen für die Suche nach dem winmdidl-Tool festzulegen.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Wichtig**    Ändern Sie für eine Arm-oder x64-Projekt Konfiguration den Parameter "/env" in "x64" oder "arm32".

Um sicherzustellen, dass die IDL-Datei bei jeder Änderung der winmd-Datei neu generiert wird, ändern Sie **das Postbuildereignis ausführen** in, **Wenn der Build die Projekt Ausgabe aktualisiert.**
Die Eigenschaften Seite Buildereignisse sollte diesem ähneln: ![ Buildereignisse](./images/buildevents.png)

Erstellen Sie die Projektmappe neu, um die IDL zu generieren und zu kompilieren.

Sie können überprüfen, ob die Projektmappe ordnungsgemäß von der MIDL kompiliert wurde, indem Sie im ToasterComponent-Projektverzeichnis nach ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c und dlldata.c suchen.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>So kompilieren Sie Proxy- und Stubcode in eine DLL

Nachdem Sie nun über die erforderlichen Dateien verfügen, können Sie diese kompilieren, um eine DLL in Form einer C++-Datei zu erstellen. Um dies so einfach wie möglich zu machen, fügen Sie ein neues Projekt hinzu, um das Erstellen der Proxys zu unterstützen. Öffnen Sie das Kontextmenü für die toasterapplication-Projekt Mappe, und wählen Sie dann **> neues Projekt hinzufügen**aus. Erweitern Sie im linken Bereich des Dialog Felds **Neues Projekt** den Eintrag **Visual C++ &gt; Fenster &gt; Univeral Windows**, und wählen Sie dann im mittleren Bereich **dll (UWP-Apps)** aus. (Beachten Sie, dass es sich hierbei nicht um ein C++ Windows-Runtime Component-Projekt handelt.) Benennen Sie die Projekt Proxys, und wählen Sie dann die Schaltfläche **OK** Diese Dateien werden bei Änderungen der C#-Klasse von den Postbuildereignissen aktualisiert.

Standardmäßig generiert das Proxyprojekt Headerdateien (.h) und CPP-Dateien in C++. Da die DLL aus den Dateien erstellt wird, die von der MIDL erstellt werden, sind die H- und CPP-Dateien nicht erforderlich. Öffnen Sie in Projektmappen-Explorer das Kontextmenü für Sie, wählen Sie **Entfernen**aus, und bestätigen Sie dann den Löschvorgang.

Da das Projekt nun leer ist, können Sie die MIDL-generierten Dateien wieder hinzufügen. Öffnen Sie das Kontextmenü für das Proxy Projekt, und wählen Sie dann **> vorhandenes Element hinzufügen aus.** Navigieren Sie im Dialogfeld zum ToasterComponent-Projektverzeichnis und wählen Sie die folgenden Dateien aus: ToasterComponent.h-, ToasterComponent_i.c-, ToasterComponent_p.c- und dlldata.c-Dateien. Wählen Sie die Schaltfläche **Hinzufügen** aus.

Erstellen Sie im Proxyprojekt eine DEF-Datei, um die DLL-Exporte zu definieren, die in „dlldata.c“ beschrieben werden. Öffnen Sie das Kontextmenü für das Projekt, und wählen Sie dann **> neues Element hinzufügen**aus. Wählen Sie links im Dialogfeld Code aus, und wählen Sie dann in der Mitte Moduldefinitionsdatei aus. Benennen Sie die Datei Proxies. def, und wählen Sie dann die Schaltfläche **Hinzufügen** aus. Öffnen Sie diese DEF-Datei und ändern Sie sie, um die EXPORTS einzuschließen, die in "dlldata.c" definiert werden:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Wenn Sie das Projekt jetzt erstellen, tritt ein Fehler auf. Um dieses Projekt ordnungsgemäß zu kompilieren, muss die Projektkompilierung und -verknüpfung geändert werden. Öffnen Sie in Projektmappen-Explorer das Kontextmenü für das Proxy Projekt, und wählen Sie dann **Eigenschaften**aus. Ändern Sie die Eigenschaften Seiten wie folgt.

Wählen Sie im linken Bereich **C/C++ > Präprozessor**aus, und wählen Sie dann im rechten Bereich **Präprozessordefinitionen**aus, klicken Sie auf die Schaltfläche mit dem Pfeil nach unten, und wählen Sie dann **Bearbeiten**aus. Fügen Sie diese Definitionen im Feld hinzu:

```cpp
WIN32;_WINDOWS
```
Ändern Sie unter **C/C++ > vorkompilierte**Header die Option **vorkompilierter Header** in **nicht verwendete vorkompilierte**Header, und wählen Sie **dann die Schalt** Fläche übernehmen aus.

Ändern Sie unter **Linker > allgemein**die Option **Import Bibliothek ignorieren** in **Ye**s, und wählen Sie **dann die Schalt** Fläche übernehmen aus.

Wählen Sie unter **Linker > Eingabe**die Option **Zusätzliche Abhängigkeiten**aus, klicken Sie auf die Schaltfläche mit dem Pfeil nach unten, und wählen Sie dann **Bearbeiten** Fügen Sie diesen Text im Feld hinzu:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Fügen Sie diese Bibliotheken nicht direkt in die Listenzeile ein. Verwenden Sie das **Bearbeitungs** Feld, um sicherzustellen, dass MSBuild in Visual Studio die richtigen zusätzlichen Abhängigkeiten beibehält.

Wenn Sie diese Änderungen vorgenommen haben, klicken Sie im Dialogfeld **Eigenschaften Seiten** auf die Schaltfläche **OK** .

Übernehmen Sie danach eine Abhängigkeit vom ToasterComponent-Projekt. Dadurch wird sichergestellt, dass der Toaster vor Erstellen des Proxyprojektbuilds erstellt wird. Dies ist erforderlich, da das Toasterprojekt für das Generieren der Dateien zum Erstellen des Proxys zuständig ist.

Öffnen Sie das Kontextmenü für das Proxyprojekt, und wählen Sie dann Projektabhängigkeiten aus. Aktivieren Sie die Kontrollkästchen, um anzugeben, dass das Proxyprojekt vom ToasterComponent-Projekt abhängig ist, damit Visual Studio sie in der richtigen Reihenfolge erstellt.

Überprüfen Sie, ob die Projekt Mappe ordnungsgemäß erstellt wird, indem Sie auf der Visual Studio-Menüleiste die Option Projekt Mappe **> erstellen**


## <a name="to-register-the-proxy-and-stub"></a>So registrieren Sie den Proxy und Stub

Öffnen Sie im toasterapplication-Projekt das Kontextmenü für "Package. appxmanifest", und wählen Sie dann **Öffnen mit**aus. Wählen Sie im Dialogfeld Öffnen mit die Option **XML-Text-Editor** , und klicken Sie dann auf die Schaltfläche **OK** . Wir werden einige XML-Daten einfügen, die eine Windows. activatableclass. ProxyStub-Erweiterungs Registrierung bereitstellen, die auf den GUIDs im Proxy basiert. Öffnen Sie "ToasterComponent_i.c.", um die GUIDs für die .appxmanifest-Datei zu suchen. Suchen Sie Einträge, die den im folgenden Beispiel genannten Einträgen ähneln. Beachten Sie auch die Definitionen für itoast, itoaster und eine dritte Schnittstelle – einen typisierten Ereignishandler, der über zwei Parameter verfügt: einen Toaster und einen Toast. Dies entspricht dem Ereignis, das in der Toaster-Klasse definiert ist. Beachten Sie, dass die GUIDs für itoast und itoaster den GUIDs entsprechen, die für die Schnittstellen in der c#-Datei definiert sind. Da die typisierte Ereignishandlerschnittstelle automatisch generiert wird, wird die GUID für diese Schnittstelle ebenfalls automatisch generiert.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Nun kopieren Sie die GUIDs und fügen Sie in "Package. appxmanifest" in einem Knoten ein, den Sie hinzufügen und benennen. Anschließend formatieren Sie Sie neu. Der Manifesteintrag ähnelt dem folgenden Beispiel – denken Sie aber auch hier daran, eigene GUIDs zu verwenden. Beachten Sie, dass die ClassId-GUID in XML mit „ITypedEventHandler2“ identisch ist. Dies liegt daran, dass diese GUID als erstes in "ToasterComponent_i.c" aufgeführt wird. Bei den hier genannten GUIDs wird zwischen Groß-/Kleinschreibung unterschieden. Anstatt die GUIDs für itoast und itoaster manuell umzuformatieren, können Sie zurück in die Schnittstellendefinitionen wechseln und den GuidAttribute-Wert erhalten, der das richtige Format aufweist. In C++ ist eine korrekt formatierte GUID im Kommentar enthalten. Auf jeden Fall müssen Sie die GUID, die sowohl für die ClassId als auch für den Ereignishandler verwendet wird, manuell umformatieren.

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

Fügen Sie den XML-Erweiterungs Knoten als direkt untergeordnetes Element des Paket Knotens und einen Peer von ein, z. b. den Knoten Ressourcen.

Bevor Sie fortfahren, muss folgendes sichergestellt sein:

-   Die ProxyStub-ClassID ist auf die erste GUID in der Datei "toastercomponent \_ i. c" festgelegt. Verwenden Sie die erste GUID, die in dieser Datei für die classId definiert ist. (Diese ist mit der GUID für "ITypedEventHandler2" ggf. identisch.)
-   Der Pfad ist der relative Pfad des Pakets der Proxy Binärdatei. (In dieser exemplarischen Vorgehensweise befindet sich "proxies.dll" im gleichen Ordner wie "ToasterApplication.winmd".)
-   Die GUIDs haben das richtige Format. (Hier können leicht Fehler passieren.)
-   Die Schnittstellen-IDs im Manifest entsprechen den IIDs in der Datei "deastercomponent \_ i. c".
-   Die Schnittstellennamen sind im Manifest eindeutig. Da diese nicht vom System verwendet werden, können Sie die Werte auswählen. Es empfiehlt sich, Schnittstellennamen auszuwählen, die eindeutig mit den Schnittstellen übereinstimmen, die Sie definiert haben. Für generierte Schnittstellen sollten die Namen auf die generierten Schnittstellen schließen lassen. Mithilfe der Datei "toastercomponent \_ i. c" können Sie Schnittstellennamen generieren.

Wenn Sie die Projektmappe jetzt ausführen, erhalten Sie eine Fehlermeldung, die angibt, dass "proxies.dll" nicht Teil der Nutzlast ist. Öffnen Sie das Kontextmenü für den Ordner **Verweise** im toasterapplication-Projekt, und wählen Sie dann **Verweis hinzufügen**aus. Aktivieren Sie das Kontrollkästchen neben dem Proxyprojekt. Stellen Sie außerdem sicher, dass das Kontrollkästchen neben „ToasterComponent“ ebenfalls aktiviert ist. Klicken Sie auf die Schaltfläche **OK** .

Das Projekt sollte nun erstellt werden. Führen Sie das Projekt aus und überprüfen Sie, ob Sie einen Toast erstellen können.

## <a name="related-topics"></a>Zugehörige Themen

* [Komponenten für Windows-Runtime mit C++/CX](creating-windows-runtime-components-in-cpp.md)