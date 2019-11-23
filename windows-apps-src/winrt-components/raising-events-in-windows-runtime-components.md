---
title: Auslösen von Ereignissen in Komponenten für Windows-Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Vorgehensweise zum Auslösen eines Ereignisses eines benutzerdefinierten Delegattyps in einem Hintergrundthread, sodass JavaScript das Ereignis empfangen kann.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 78bc43c26a73a6184e5788ad7d003813e567a8d0
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052018"
---
# <a name="raising-events-in-windows-runtime-components"></a>Auslösen von Ereignissen in Komponenten für Windows-Runtime
> [!NOTE]
> Informationen zum Erstellen von Ereignissen in einer [ C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) -Windows-Runtime Komponente finden Sie unter [Verfassen von C++Ereignissen in/WinRT](../cpp-and-winrt-apis/author-events.md).

Wenn die Komponente für Windows-Runtime ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrundthread (Arbeitsthread) auslöst und JavaScript in der Lage sein soll, das Ereignis zu empfangen, können Sie es auf eine der folgenden Weisen implementieren und/oder auslösen:

-   (Option 1) Lösen Sie das Ereignis mit dem [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher) aus, um das Ereignis an den JavaScript-Threadkontext zu marshallen. Dies ist in der Regel zwar die beste Option, erzielt in manchen Szenarien aber möglicherweise nicht die schnellste Leistung.
-   (Option 2) Verwenden Sie [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt;, allerdings mit Verlust von Ereignistypinformationen. Falls Option 1 nicht möglich oder die Leistung nicht ausreichend ist, bietet diese Option eine gute Alternative, wenn der Verlust von Typinformationen akzeptabel ist.
-   (Option 3) Erstellen Sie einen eigenen Proxy und Stub für die Komponente. Diese Option ist am schwierigsten zu implementieren, behält aber die Typinformationen bei und erzielt in anspruchsvollen Szenarien unter Umständen eine bessere Leistung als Option 1.

Wenn Sie nur ein Ereignis in einem Hintergrundthread ohne eine dieser Optionen auslösen, wird das Ereignis von einem JavaScript-Client nicht empfangen.

## <a name="background"></a>Hintergrund

Bei allen Komponenten und Apps für Windows-Runtime handelt es sich im Grunde um COM-Objekte, unabhängig davon, welche Sprache Sie bei ihrer Erstellung verwenden. In der Windows-API sind die meisten Komponenten agile COM-Objekte, die mit Objekten im Hintergrundthread und im UI-Thread gleichermaßen gut kommunizieren können. Wenn ein COM-Objekt nicht agil angelegt werden kann, sind Hilfsobjekte (auch als Proxys bzw. Stubs bezeichnet) erforderlich, um mit anderen COM-Objekten über die Threadgrenze des UI-Threadhintergrunds hinweg zu kommunizieren. (In der COM-Terminologie wird dies als Kommunikation zwischen Threadapartments bezeichnet.)

Die meisten Objekte in der Windows-API sind entweder agil oder verfügen über integrierte Proxys und Stubs. Allerdings können Proxys und Stubs nicht für generische Typen wie Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) erstellt werden, da sie erst vollständige Typen sind, wenn das Typargument bereitgestellt ist. Fehlende Proxys oder Stubs stellen nur bei JavaScript-Clients ein Problem dar. Wenn die Komponente aber sowohl in JavaScript als auch in C++ oder einer .NET-Sprache verwendet werden soll, müssen Sie eine der drei folgenden Optionen verwenden.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>Option 1) Auslösen des Ereignisses mit dem CoreDispatcher-Ereignis

Sie können Ereignisse eines benutzerdefinierten Delegattyps mit [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher) senden, und JavaScript kann diese Ereignisse empfangen. Wenn Sie nicht sicher sind, welche Option Sie verwenden sollen, versuchen Sie es zunächst mit dieser ersten Option. Wenn die Wartezeit zwischen dem Auslösen von Ereignissen und der Ereignisbehandlung ein Problem darstellt, versuchen Sie es mit einer der anderen Optionen.

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

Eine weitere Möglichkeit, ein Ereignis aus einem Hintergrundthread zu senden, ist die Verwendung von [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt; als Typ des Ereignisses. Windows instanziiert den generischen Typ konkret und stellt dafür einen Proxy und einen Stub bereit. Der Nachteil besteht darin, dass die Typinformationen der Ereignisargumente und des Senders verloren gehen. C++- und .NET-Clients müssen aus der Dokumentation entnehmen können, welcher Typ in den ursprünglichen Typ umgewandelt werden muss, wenn das Ereignis empfangen wird. JavaScript-Clients benötigt keine Informationen über die ursprünglichen Typen. Sie finden die Argumenteigenschaften anhand ihrer Namen in den Metadaten.

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

Diese exemplarische Vorgehensweise besteht aus folgenden Teilen:

-   Hier erstellen Sie zwei Windows-Runtime-Basisklassen. Die eine Klasse macht ein Ereignis vom Typ [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) verfügbar, und die andere Klasse ist der Typ, der als Argument für TValue an JavaScript zurückgegeben wird. Diese Klassen können erst mit JavaScript kommunizieren, wenn Sie die nachfolgenden Schritten ausgeführt haben.
-   Diese App aktiviert das Hauptklassenobjekt, ruft eine Methode auf und behandelt ein Ereignis, das von der Komponente für Windows-Runtime ausgelöst wird.
-   Diese werden von den Tools benötigt, die die Proxy- und Stubklassen generieren.
-   Dann wird mithilfe der IDL-Datei der C-Quellcode für den Proxy und den Stub generiert.
-   Registrieren Sie die Proxy-Stub-Objekte, damit die COM-Laufzeitumgebung sie finden kann, und verweisen Sie im App-Projekt auf die Proxy-Stub-DLL-Datei.

## <a name="to-create-the-windows-runtime-component"></a>So erstellen Sie die Komponente für Windows-Runtime

Klicken Sie in Visual Studio auf der Menüleiste auf **Datei &gt; Neues Projekt**. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **JavaScript &gt; Universal Windows**, und wählen Sie dann **Leere App** aus. Geben Sie als Projektnamen „ToasterApplication“ ein, und klicken Sie dann auf die Schaltfläche **OK**.

Fügen Sie der Projektmappe eine C#-Komponente für Windows-Runtime hinzu: Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Hinzufügen &gt; Neues Projekt** aus. Erweitern **Sie C# Visual &gt; Microsoft Store** , und wählen Sie dann **Windows-Runtime Komponente**aus. Geben Sie als Projektnamen „ToasterComponent“ ein, und klicken Sie dann auf die Schaltfläche **OK**. ToasterComponent ist der Stammnamespace für die Komponenten, die Sie in späteren Schritten erstellen.

Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Eigenschaften** aus. Wählen Sie im Dialogfeld **Eigenschaftenseiten** im linken Bereich **Konfigurationseigenschaften** aus, und legen Sie dann oben im Dialogfeld die **Konfiguration** auf **Debuggen** und die **Plattform** auf „x86”, „x64” oder „ARM” fest. Klicken Sie auf die Schaltfläche **OK**.

**Wichtig** Platform = Any CPU funktioniert nicht, da Sie für die Win32-DLL mit System eigenem Code, die Sie später der Projekt Mappe hinzufügen, nicht gültig ist.

Benennen Sie im Projektmappen-Explorer die Datei „class1.cs” in „ToasterComponent.cs” um, sodass sie dem Namen des Projekts entspricht. Visual Studio benennt die Klasse in der Datei entsprechend dem neuen Dateinamen automatisch um.

Fügen Sie der CS-Datei eine using-Direktive für den Namespace Windows.Foundation hinzu, um TypedEventHandler in den Gültigkeitsbereich einzubinden.

Wenn Sie Proxys und Stubs benötigen, muss die Komponente mit Schnittstellen ihre öffentlichen Member verfügbar machen. Definieren Sie in „ToasterComponent.cs” eine Schnittstelle für den Toaster und eine andere für den „Toast” (Popup), den der Toaster erzeugt.

**Hinweis** in C# können Sie diesen Schritt überspringen. Erstellen Sie stattdessen zuerst eine Klasse, öffnen Sie das Kontextmenü, und wählen Sie **Umgestalten &gt; Schnittstelle extrahieren** aus. Ordnen Sie den Schnittstellen im generierten Code manuell öffentlichen Zugriff zu.

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

Im vorhergehenden Code wird das Popup erstellt und eine Arbeitsaufgabe im Threadpool ausgeführt, um die Benachrichtigung auszulösen. Auch wenn die IDE vorschlagen sollte, das „await“-Schlüsselwort dem asynchronen Aufruf zuzuweisen, ist dies in diesem Fall nicht nötig, da die Methode keine Aktionen ausführt, die von den Ergebnissen des Vorgangs abhängig sind.

**Beachten Sie** der Async-Aufrufe im vorangehenden Code verwendet Thread Pool. runasync ausschließlich, um eine einfache Möglichkeit zum Auslösen des Ereignisses in einem Hintergrund Thread zu veranschaulichen. Sie könnten diese spezielle Methode auch schreiben wie im folgenden Beispiel gezeigt. Dies funktioniert, da der .NET-Taskplaner automatisch „async/await“-Aufrufe zurück an den UI-Thread marshallt.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Wenn Sie das Projekt jetzt erstellen, sollte dies ordnungsgemäß ausgeführt werden.

## <a name="to-program-the-javascript-app"></a>So programmieren Sie die JavaScript-App

Jetzt können Sie der JavaScript-App eine Schaltfläche hinzufügen, damit sie die Klasse verwendet, die Sie gerade zum Erstellen des Popups definiert haben. Vorher müssen Sie einen Verweis auf das ToasterComponent-Projekt hinzufügen, das Sie gerade erstellt haben. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für das ToasterApplication-Projekt, wählen Sie **Hinzufügen &gt; Verweise** und anschließend die Schaltfläche **Neuen Verweis hinzufügen** aus. Wählen Sie im Dialogfeld „Verweis hinzufügen” im linken Bereich unter „Projektmappe” das Komponentenprojekt aus, und wählen Sie dann im mittleren Bereich „ToasterComponent” aus. Klicken Sie auf die Schaltfläche **OK**.

Öffnen Sie im Projektmappen-Explorer das Kontextmenü für das ToasterApplication-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

Fügen Sie am Ende der Datei „default.js” einen Namespace hinzu, der die Funktionen zum Aufrufen der Komponente und zum Zurückrufen durch die Komponente enthält. Der Namespace hat zwei Funktionen: eine zum Erstellen des Popups und eine zum Behandeln des Abschlussereignisses des Popups. Die Implementierung von makeToast erstellt ein Toaster-Objekt, registriert den Ereignishandler und erstellt das Popup. Bisher führt der Ereignishandler nicht viel aus, wie hier gezeigt:

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

Die makeToast-Funktion muss mit einer Schaltfläche verknüpft werden. Aktualisieren Sie die Datei „default.HTML” so, dass eine Schaltfläche und Leerzeichen eingefügt werden, um das Ergebnis der Popuperstellung auszugeben:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Ohne TypedEventHandler könnte die App schon auf dem lokalen Computer ausgeführt und auf die Schaltfläche zum Erstellen des Popups geklickt werden. In dieser App passiert aber nichts. Um herauszufinden, woran das liegt, wird der verwaltete Code gedebuggt, der ToastCompletedEvent auslöst. Beenden Sie das Projekt, und wählen Sie dann in der Menüleiste **Debuggen &gt; Toaster-Anwendungseigenschaften** aus. Ändern Sie den **Debuggertyp** in **Nur verwaltet**. Wählen Sie nun in der Menüleiste **Debuggen &gt; Ausnahmen** und dann **Common Language Runtime-Ausnahmen** aus.

Führen Sie die App jetzt aus, und klicken Sie auf die Schaltfläche zum Erstellen des Popups. Der Debugger fängt die Ausnahme „ungültige Umwandlung” ab. Obwohl dies nicht aus der Meldung ersichtlich ist, wird diese Ausnahme ausgelöst, weil Proxys für diese Schnittstelle fehlen.

![Fehlender Proxy](./images/debuggererrormissingproxy.png)

Der erste Schritt zum Erstellen eines Proxys und Stubs für eine Komponente besteht darin, den Schnittstellen eine eindeutige ID oder GUID hinzufügen. Das zu verwendende GUID-Format hängt davon ab, ob Sie in C#, Visual Basic, einer anderen .NET-Sprache oder in C++ programmieren.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>So generieren Sie GUIDs für die Schnittstellen der Komponente (C#- oder andere .NET-Sprachen)

Wählen Sie in der Menüleiste „Extras &gt; GUID erstellen” aus. Wählen Sie im Dialogfeld „5. \[GUID ("xxxxxxxx-xxxx... xxxx ")\]. Klicken Sie auf die Schaltfläche „Neue GUID” und dann auf die Schaltfläche „Kopieren”.

![GUID-Generator-Tool](./images/guidgeneratortool.png)

Wechseln Sie zurück zur Schnittstellendefinition, und fügen Sie die neue GUID direkt vor der IToaster-Schnittstelle ein, wie im folgenden Beispiel gezeigt. (Verwenden Sie nicht die GUID aus dem Beispiel. Jede eindeutige Schnittstelle sollte eine eigene GUID haben.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Fügen Sie eine using-Direktive für den System.Runtime.InteropServices-Namespace hinzu.

Wiederholen Sie diese Schritte für die IToast-Schnittstelle.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>So generieren Sie GUIDs für die Schnittstellen der Komponente (C++)

Wählen Sie in der Menüleiste „Extras &gt; GUID erstellen” aus. Wählen Sie im Dialogfeld „3. Struktur-GUID mit statischem Konstruktor = { ... }” aus. Klicken Sie auf die Schaltfläche „Neue GUID” und dann auf die Schaltfläche „Kopieren”.

Fügen Sie die GUID direkt vor der Definition der IToaster-Schnittstelle ein. Nach dem Einfügen sollte die GUID dem folgenden Beispiel ähneln: (Verwenden Sie nicht die GUID aus dem Beispiel. Jede eindeutige Schnittstelle sollte eine eigene GUID haben.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Fügen Sie eine using-Direktive für Windows.Foundation.Metadata hinzu, um GuidAttribute in den Gültigkeitsbereich einzubinden.

Konvertieren Sie nun die GUID mit dem Konstruktor manuell in ein GuidAttribute, damit die GUID wie im folgenden Beispiel gezeigt formatiert wird. Beachten Sie, dass die geschweiften Klammern durch eckige und runde Klammern ersetzt werden und das nachfolgende Semikolon entfernt wird.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Wiederholen Sie diese Schritte für die IToast-Schnittstelle.

Da die Schnittstellen nun über eindeutige IDs verfügen, kann eine IDL-Datei erstellt werden, indem die WINMD-Datei in das winmdidl-Befehlszeilentool eingefügt wird. Anschließend wird der C-Quellcode für den Proxy und den Stub generiert, indem diese IDL-Datei in das MIDL-Befehlszeilentool eingefügt wird. Visual Studio führt dies automatisch aus, wenn Postbuildereignisse erstellt werden, wie in den nachfolgenden Schritten gezeigt.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>So generieren Sie den Proxy- und Stubquellcode

Um ein benutzerdefiniertes Postbuildereignis hinzuzufügen, öffnen Sie im Projektmappen-Explorer das Kontextmenü für das ToasterComponent-Projekt, und wählen Sie dann „Eigenschaften” aus. Wählen Sie im linken Bereich der Eigenschaftenseiten „Buildereignisse” aus, und klicken Sie dann auf die Schaltfläche „Postbuild bearbeiten”. Fügen Sie der Postbuildbefehlszeile die folgenden Befehle hinzu. (Die Batchdatei muss zuerst aufgerufen werden, um die Umgebungsvariablen zum Suchen des winmdidl-Tools festzulegen.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Wichtig**  für eine Arm-oder x64-Projekt Konfiguration ändern Sie den Parameter "/env" in "x64" oder "arm32".

Um sicherzustellen, dass die IDL-Datei bei jeder Änderung der WINMD-Datei neu generiert wird, ändern Sie **Postbuildereignis ausführen** in **Wenn der Build die Projektausgabe aktualisiert**.
Die Eigenschaften Seite Buildereignisse sollte in etwa wie folgt aussehen: ![Build-Ereignisse](./images/buildevents.png)

Erstellen Sie die Projektmappe neu, um die IDL zu generieren und zu kompilieren.

Sie können überprüfen, ob MIDL die Projektmappe ordnungsgemäß kompiliert hat, indem Sie im ToasterComponent-Projektverzeichnis nach ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c und dlldata.c suchen.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>So kompilieren Sie den Proxy- und Stubcode in eine DLL-Datei

Nachdem Sie nun über die erforderlichen Dateien verfügen, können Sie diese kompilieren, um eine DLL (C++-Datei) zu erstellen. Um dies so einfach wie möglich zu machen, fügen Sie ein neues Projekt hinzu, um das Erstellen der Proxys zu unterstützen. Öffnen Sie das Kontextmenü für die ToasterApplication-Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus. Erweitern Sie im linken Bereich des Dialog Felds **Neues Projekt** den Eintrag **Visual C++ &gt; Fenster &gt; Univeral Windows**, und wählen Sie dann im mittleren Bereich **dll (UWP-Apps)** aus. (Beachten Sie, dass dies KEIN C++-Komponentenprojekt für Windows-Runtime ist.) Geben Sie als Projektnamen „Proxies” ein, und klicken Sie dann auf die Schaltfläche **OK**. Diese Dateien werden bei Änderungen der C#-Klasse von den Postbuildereignissen aktualisiert.

Standardmäßig generiert das Proxies-Projekt Headerdateien (.h) und C++-Dateien (.cpp). Da die DLL aus den Dateien erstellt wird, die von MIDL generiert werden, sind die H- und CPP-Dateien nicht erforderlich. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für diese Dateien, wählen Sie **Entfernen**, und bestätigen Sie dann die Löschung.

Da das Projekt nun leer ist, können Sie die von MIDL generierten Dateien wieder hinzufügen. Öffnen Sie das Kontextmenü für das Proxies-Projekt, und wählen Sie dann **Hinzufügen > Vorhandenes Element** aus. Navigieren Sie im Dialogfeld zum ToasterComponent-Projektverzeichnis, und wählen Sie die folgenden Dateien aus: ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c, und dlldata.c. Klicken Sie auf die Schaltfläche **Hinzufügen**.

Erstellen Sie im Proxies-Projekt eine DEF-Datei, um die DLL-Exporte zu definieren, die in dlldata.c beschrieben sind. Öffnen Sie das Kontextmenü für das Projekt, und wählen Sie dann **Hinzufügen > Neues Element**. Wählen Sie im linken Bereich des Dialogfelds „Code” aus, und wählen Sie dann im mittleren Bereich „Moduldefinitionsdatei” aus. Nennen Sie die Datei „proxies.def”, und klicken Sie dann auf die Schaltfläche **Hinzufügen**. Öffnen Sie diese DEF-Datei und bearbeiten Sie sie, um die EXPORTS einzuschließen, die in dlldata.c definiert sind:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Wenn Sie das Projekt jetzt erstellen, tritt ein Fehler auf. Damit dieses Projekt ordnungsgemäß kompiliert wird, müssen Sie die Einstellungen für die Kompilierung und Verknüpfung des Projekts ändern. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für das Proxies-Projekt, und wählen Sie dann **Eigenschaften** aus. Ändern Sie die Eigenschaftenseiten wie folgt:

Wählen Sie im linken Bereich **C/C++ > Präprozessor** und dann im rechten Bereich **Präprozessordefinitionen** aus, klicken Sie auf die Schaltfläche mit dem Pfeil nach unten und dann auf **Bearbeiten**. Fügen Sie diese Definitionen in das Feld ein:

```cpp
WIN32;_WINDOWS
```
Ändern Sie unter **C/C++ > Vorkompilierte Header** die Option **Vorkompilierter Header** in **Vorkompilierte Header nicht verwenden**, und klicken Sie dann auf die Schaltfläche **Übernehmen**.

Ändern Sie unter **Linker > Allgemein** die Option **Importbibliothek ignorieren** in **Ja**, und klicken Sie dann auf die Schaltfläche **Übernehmen**.

Wählen Sie unter **Linker > Eingabe** die Option **Zusätzliche Abhängigkeiten** aus, klicken Sie auf die Schaltfläche mit dem Pfeil nach unten, und klicken Sie dann auf **Bearbeiten**. Fügen Sie diesen Text in das Feld ein:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Fügen Sie diese Bibliotheken nicht direkt in die Listenzeile ein. Verwenden Sie das **Eingabefeld**, um sicherzustellen, dass MSBuild in Visual Studio die richtigen zusätzlichen Abhängigkeiten beibehält.

Wenn Sie diese Änderungen vorgenommen haben, klicken Sie im Dialogfeld **Eigenschaftenseiten** auf **OK**.

Legen Sie als Nächstes eine Abhängigkeit vom ToasterComponent-Projekt fest. Dadurch wird sichergestellt, dass der Toaster vor dem Proxies-Projekt erstellt wird. Dies ist erforderlich, da das Toaster-Projekt für das Generieren der Dateien zum Erstellen des Proxys zuständig ist.

Öffnen Sie das Kontextmenü für das Proxies-Projekt, und wählen Sie dann „Projektabhängigkeiten” aus. Aktivieren Sie die Kontrollkästchen, um anzugeben, dass das Proxies-Projekt vom ToasterComponent-Projekt abhängig ist, damit Visual Studio die Projekte in der richtigen Reihenfolge erstellt.

Stellen Sie sicher, dass die Projektmappe ordnungsgemäß erstellt wird, indem Sie in Visual Studio in der Menüleiste **Erstellen > Projektmappe neu erstellen** auswählen.


## <a name="to-register-the-proxy-and-stub"></a>So registrieren Sie den Proxy und den Stub

Öffnen Sie im ToasterApplication-Projekt das Kontextmenü für „package.appxmanifest”, und wählen Sie dann **Öffnen mit** aus. Wählen Sie im Dialogfeld „Öffnen mit” die Option **XML-Text-Editor** aus, und klicken Sie dann auf die Schaltfläche **OK**. Um eine windows.activatableClass.proxyStub-Erweiterungsregistrierung bereitzustellen, wird XML-Code eingefügt, der auf den GUIDs im Proxy basiert. Öffnen Sie „ToasterComponent_i.c”, um die GUIDs für die .appxmanifest-Datei zu suchen. Suchen Sie Einträge, die denen im folgenden Beispiel ähneln Beachten Sie außerdem die Definitionen für IToast, IToaster sowie für eine dritte Schnittstelle – einen typisierten Ereignishandler mit den beiden Parametern: Toaster und Toast. Dies entspricht dem Ereignis, das in der Toaster-Klasse definiert ist. Die GUIDs für IToast und IToaster stimmen mit den GUIDs überein, die für die Schnittstellen in der C#-Datei definiert sind. Da die typisierte Ereignishandlerschnittstelle automatisch generiert wird, wird die GUID für diese Schnittstelle ebenfalls automatisch generiert.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Die GUIDs werden jetzt kopiert, in einen hinzugefügten Knoten mit Namen „Extensions” in „package.appxmanifest” eingefügt und anschließend neu formatiert. Der Manifesteintrag ähnelt dem folgenden Beispiel – denken Sie aber auch hier daran, eigene GUIDs zu verwenden. Beachten Sie, dass die ClassId-GUID in XML mit „ITypedEventHandler2” identisch ist. Dies liegt daran, dass diese GUID als erste in ToasterComponent_i.c aufgeführt wird. Bei diesen GUIDs wird zwischen Groß-/Kleinschreibung unterschieden. Anstatt die GUIDs für IToast und IToaster manuell neu zu formatieren, können Sie zurück zur Schnittstellendefinitionen wechseln und den GuidAttribute-Wert suchen, der das richtige Format aufweist. In C++ ist eine richtig formatierte GUID im Kommentar enthalten. Auf jeden Fall müssen Sie die GUID, die für die ClassId und für den Ereignishandler verwendet wird, manuell neu formatieren.

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

Fügen Sie den „Extensions”-XML-Knoten als direkt dem Package-Knoten untergeordnetes Element und einen Peer, z. B. den „Resources”-Knoten, ein.

Bevor Sie fortfahren, muss Folgendes sichergestellt sein:

-   Die ProxyStub ClassID wird auf die erste GUID in der Datei toastercomponent\_i. c festgelegt. Verwenden Sie die erste GUID, die in dieser Datei für die ClassId definiert ist. (Diese ist möglicherweise mit der GUID für „ITypedEventHandler2” identisch.)
-   „Path” ist der relative Pfad des Pakets der Proxybinärdatei. (In dieser exemplarischen Vorgehensweise befindet sich „proxies.dll” im gleichen Ordner wie „ToasterApplication.winmd”.)
-   Die GUIDs haben das richtige Format. (Hier können leicht Fehler passieren.)
-   Die Schnittstellen-IDs im Manifest entsprechen den IIDs in der Datei "deastercomponent\_i. c".
-   Die Namen der Schnittstellen sind im Manifest eindeutig. Da diese nicht vom System verwendet werden, können Sie die Werte festlegen. Es sollten Schnittstellennamen gewählt werden, die eindeutig mit Schnittstellen übereinstimmen, die Sie definiert haben. Bei generierten Schnittstellen sollten die Namen auf die generierten Schnittstellen schließen lassen. Mithilfe der Datei toastercomponent\_i. c können Sie Schnittstellennamen generieren.

Wenn Sie die Projektmappe jetzt auszuführen versuchen, erhalten Sie eine Fehlermeldung, die angibt, dass „proxies.dll” nicht Teil der Nutzlast ist. Öffnen Sie im ToasterApplication-Projekt das Kontextmenü für den Ordner **Verweise**, und wählen Sie **Verweis hinzufügen** aus. Aktivieren Sie das Kontrollkästchen neben dem Proxies-Projekt. Stellen Sie außerdem sicher, dass auch das Kontrollkästchen neben „ToasterComponent” aktiviert ist. Klicken Sie auf die Schaltfläche **OK**.

Das Projekt sollte nun erstellt werden. Führen Sie das Projekt aus, und überprüfen Sie, ob Sie ein Popup erstellen können.

## <a name="related-topics"></a>Verwandte Themen

* [Komponenten für Windows-Runtime mit C++/CX](creating-windows-runtime-components-in-cpp.md)
