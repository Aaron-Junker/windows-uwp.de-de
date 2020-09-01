---
title: Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen
description: Erstellen Sie eine Out-of-Process-Hintergrundaufgabenklasse, und registrieren Sie sie für die Ausführung, wenn Ihre App nicht im Vordergrund ausgeführt wird.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 638d4e8b4d4bc074566c8b0a6c24d733a5769b32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164894"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen

**Wichtige APIs**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Erstellen Sie eine Hintergrundaufgabenklasse, und registrieren Sie diese für die Ausführung, wenn sich die App nicht im Vordergrund befindet. In diesem Thema wird gezeigt, wie Sie eine Hintergrundaufgabe erstellen und registrieren, die in einem vom App-Prozess getrennten Prozess ausgeführt wird. Informationen zum Ausführen von Hintergrundarbeiten direkt in der Vordergrundanwendung finden Sie unter [Erstellen und Registrieren einer In-Process-Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](../audio-video-camera/background-audio.md) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

## <a name="create-the-background-task-class"></a>Erstellen einer Hintergrundaufgabenklasse

Sie können Code im Hintergrund ausführen, indem Sie Klassen schreiben, die die [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)-Schnittstelle implementieren. Dieser Code wird ausgeführt, wenn ein bestimmtes Ereignis mithilfe von ausgelöst wird, z. b. [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) oder [**maintenancetrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

Die folgenden Schritte zeigen, wie Sie eine neue Klasse zum Implementieren der [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)-Schnittstelle schreiben.

1.  Erstellen Sie ein neues Projekt für Hintergrundaufgaben, und fügen Sie es zu Ihrer Projektmappe hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Projektmappenknoten im **Projektmappen-Explorer** , und wählen Sie **Add** \> **Neues Projekt**hinzufügen aus. Wählen Sie dann den Projekttyp **Windows-Runtime Komponente** aus, benennen Sie das Projekt, und klicken Sie auf OK.
2.  Verweisen Sie auf das Hintergrundaufgabenprojekt aus dem Projekt für UWP-Apps (Universelle Windows-Plattform). Klicken Sie für eine c#-oder C++-App im App-Projekt mit der rechten Maustaste auf **Verweise** , und wählen Sie **neuen Verweis hinzufügen**aus. Wählen Sie unter **Projektmappe** die Option **Projekte**. Wählen Sie dann den Namen Ihres Hintergrundaufgabenprojekts aus, und klicken Sie auf **OK**.
3.  Fügen Sie dem Projekt im Hintergrundaufgaben eine neue Klasse hinzu, die die [**ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) -Schnittstelle implementiert. Die [**ibackgroundtask. Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) -Methode ist ein erforderlicher Einstiegspunkt, der aufgerufen wird, wenn das angegebene Ereignis ausgelöst wird. Diese Methode ist in jeder Hintergrundaufgabe erforderlich.

> [!NOTE]
> Die Hintergrundaufgaben Klasse selbst &mdash; und alle anderen Klassen im Hintergrundaufgaben Projekt &mdash; müssen **öffentliche** Klassen sein, die **versiegelt** (oder **endgültig**) sind.

Der folgende Beispielcode zeigt einen grundlegenden Ausgangspunkt für eine Hintergrundaufgaben Klasse.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  Wenn asynchroner Code in der Hintergrundaufgabe ausgeführt wird, muss in der Hintergrundaufgabe eine Verzögerung verwendet werden. Wenn Sie keine Verzögerung verwenden, kann der Hintergrundaufgaben Prozess unerwartet beendet werden, wenn die **Run** -Methode zurückgegeben wird, bevor die asynchrone Arbeit vollständig ausgeführt wurde.

Fordern Sie die Verzögerung in der **Run** -Methode an, bevor Sie die asynchrone Methode aufrufen. Speichern Sie die Verzögerung in einem Klassendatenmember, damit Sie über die asynchrone Methode darauf zugreifen können. Deklarieren Sie das Objekt so, dass die Verzögerung nach Abschluss des asynchronen Codes abgeschlossen wird.

Der folgende Beispielcode ruft die Verzögerung ab, speichert Sie und gibt Sie frei, wenn der asynchrone Code fertig ist.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> In C# werden asynchrone Methoden für Hintergrundaufgaben mithilfe der Schlagwörter **async/await** aufgerufen. In C++/CX kann ein ähnliches Ergebnis durch die Verwendung einer Aufgaben Kette erreicht werden.

Weitere Informationen zu asynchronen Mustern finden Sie unter [Asynchrone Programmierung](../threading-async/asynchronous-programming-universal-windows-platform-apps.md). Weitere Beispiele zum Verhindern der vorzeitigen Beendigung einer Hintergrundaufgabe mithilfe von Verzögerungen finden Sie im [Beispiel zu Hintergrundaufgaben](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

Die folgenden Schritte werden in einer Ihrer App-Klassen durchgeführt (beispielsweise in "MainPage.xaml.cs").

> [!NOTE]
> Sie können auch eine Funktion erstellen, die dediziert ist, um Hintergrundaufgaben zu registrieren &mdash; . siehe [Registrieren eines Hintergrund](register-a-background-task.md)Tasks. In diesem Fall können Sie anstelle der nächsten drei Schritte einfach den-Triggern erstellen und für die Registrierungsfunktion zusammen mit dem Aufgaben Namen, dem Task Einstiegspunkt und (optional) einer Bedingung bereitstellen.

## <a name="register-the-background-task-to-run"></a>Registrieren der auszuführenden Hintergrundaufgabe

1.  Stellen Sie fest, ob der Hintergrund Task bereits durch Iteration der [**backgroundtaskregistration. AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) -Eigenschaft registriert ist. Dieser Schritt ist sehr wichtig. Sollte Ihre App nicht überprüfen, ob Hintergrundaufgaberegistrierungen vorhanden sind, könnte es passieren, dass sie die Aufgabe mehrere Male registriert, was zu Leistungseinbrüchen führt und die verfügbare CPU-Zeit der Aufgabe maximiert, bevor die Arbeit abgeschlossen werden kann.

Im folgenden Beispiel wird die **AllTasks** -Eigenschaft durchlaufen und eine Flag-Variable auf true festgelegt, wenn die Aufgabe bereits registriert ist.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  Wenn die Hintergrundaufgabe noch nicht registriert ist, verwenden Sie [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder), um eine Instanz Ihrer Hintergrundaufgabe zu generieren. Bei dem Einstiegspunkt der Aufgabe sollte es sich um den Namen der Hintergrundaufgabenklasse mit dem Namespace als Präfix handeln.

Der Hintergrundaufgabenauslöser bestimmt, ob die Hintergrundaufgabe ausgeführt wird. Eine Liste mit möglichen Auslösern erhalten Sie unter [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Mit diesem Code wird beispielsweise eine neue Hintergrundaufgabe erstellt und festgelegt, dass Sie beim Auftreten des **timezonechanged** -Auslösers ausgeführt wird:

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  Sie können eine Bedingung hinzufügen, die bestimmt, wann Ihre Aufgabe ausgeführt wird, nachdem das Auslöseereignis eintritt (optional). Wenn Sie zum Beispiel möchten, dass die Aufgabe erst ausgeführt wird, wenn der Benutzer anwesend ist, verwenden Sie die Bedingung **UserPresent**. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

Der folgende Beispielcode weist eine Bedingung zu, die die Anwesenheit des Benutzers voraussetzt:

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  Registrieren Sie die Hintergrundaufgabe, indem Sie die Register-Methode für das [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Objekt aufrufen. Speichern Sie das [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)-Ergebnis, sodass es im nächsten Schritt verwendet werden kann.

Der folgende Code registriert die Hintergrundaufgabe und speichert das Ergebnis:

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.

Verwenden Sie den **ServicingComplete**-Trigger (siehe [SystemTriggerType](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) zum Ausführen von Konfigurationsänderungen nach dem Update wie Migrieren der Datenbank der App und Registrieren von Hintergrundaufgaben, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Es empfiehlt sich, die Registrierung von Hintergrundaufgaben im Zusammenhang mit der vorherigen Version der App aufzuheben (siehe [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)) und die Hintergrundaufgaben für die neue Version der App zu registrieren (siehe [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)).

Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Behandeln des Abschlusses der Hintergrundaufgabe mithilfe von Ereignishandlern

Sie sollten eine Methode mit dem [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)-Element registrieren, damit Ihre App Ergebnisse von der Hintergrundaufgabe abrufen kann. Wenn die APP gestartet oder fortgesetzt wird, wird die markierte Methode aufgerufen, wenn die Hintergrundaufgabe abgeschlossen wurde, seit sich die APP zuletzt im Vordergrund befand. (Die OnCompleted-Methode wird sofort aufgerufen, wenn die Hintergrundaufgabe abgeschlossen wird, während sich Ihre App im Vordergrund befindet.)

1.  Schreiben Sie eine OnCompleted-Methode, um den Abschluss von Hintergrundaufgaben zu behandeln. Das Ergebnis der Hintergrundaufgabe kann z. B. eine Aktualisierung der Benutzeroberfläche auslösen. Das hier gezeigte Methodenprofil ist für die OnCompleted-Ereignishandlermethode erforderlich, obwohl dieses Beispiel nicht den *args*-Parameter verwendet.

Der folgende Beispielcode erkennt den Abschluss der Hintergrundaufgabe und ruft eine Beispielmethode zur Aktualisierung der Benutzeroberfläche auf, die eine Meldungszeichenfolge erfordert.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> Aktualisierungen der Benutzeroberfläche sollten asynchron durchgeführt werden, um den Benutzeroberflächenthread nicht zu blockieren. Ein Beispiel dazu finden Sie in der UpdateUI-Methode im [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

2.  Gehen Sie dorthin zurück, wo Sie die Hintergrundaufgabe registriert haben. Fügen Sie nach dieser Codezeile ein neues [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)-Objekt hinzu. Stellen Sie Ihre OnCompleted-Methode als Parameter für den **BackgroundTaskCompletedEventHandler**-Konstruktor zur Verfügung.

Der folgende Beispielcode fügt dem [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)-Element ein [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)-Element hinzu:

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Deklarieren im App-Manifest, dass Ihre APP Hintergrundaufgaben verwendet

Bevor Ihre App Hintergrundaufgaben ausführen kann, müssen Sie alle Hintergrundaufgaben im App-Manifest deklarieren. Wenn die APP versucht, eine Hintergrundaufgabe mit einem-Fehler zu registrieren, der nicht im Manifest aufgeführt ist, schlägt die Registrierung der Hintergrundaufgabe mit dem Fehler "Lauf Zeit Klasse nicht registriert" fehl.

1.  Öffnen Sie den Paketmanifest-Designer durch Öffnen der Datei namens „Package.appxmanifest“.
2.  Öffnen Sie die Registerkarte **Deklarationen**.
3.  Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Hintergrundaufgaben** aus, und klicken Sie auf **Hinzufügen**.
4.  Aktivieren Sie das Kontrollkästchen **Systemereignis**.
5.  Geben Sie im Textfeld **Einstiegspunkt:** den Namespace und den Namen der Hintergrund Klasse ein, die für dieses Beispiel "Tasks. examplebackgroundtask" lautet.
6.  Schließen Sie den Manifest-Designer.

Das folgende Erweiterungselement wird zur Datei „Package.appxmanifest“ hinzugefügt, um die Hintergrundaufgabe zu registrieren:

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Sie sollten jetzt über die Grundlagen verfügen, um eine Hintergrundaufgabenklasse zu schreiben, die Hintergrundaufgabe in Ihrer App zu registrieren und Ihre App so zu konfigurieren, dass Sie den Abschluss der Hintergrundaufgabe erkennt. Sie sollten auch mit der Aktualisierung des Anwendungsmanifests vertraut sein, damit Ihre App die Hintergrundaufgabe erfolgreich registrieren kann.

> [!NOTE]
> Laden Sie das [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) herunter, um ähnliche Codebeispiele im Kontext einer vollständigen und stabilen UWP-App (Universelle Windows-Plattform) mit Hintergrundaufgaben zu erhalten.

Eine API-Referenz, konzeptionelle Richtlinien zu Hintergrundaufgaben und ausführlichere Anweisungen zum Schreiben von Apps, die Hintergrundaufgaben verwenden, finden Sie unter den folgenden verwandten Themen:

## <a name="related-topics"></a>Zugehörige Themen

**Ausführliche Themen mit Anweisungen zu Hintergrundaufgaben**

* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Erstellen und registrieren Sie eine Prozess interne Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).
* [Konvertieren einer Out-of-Process-Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe](convert-out-of-process-background-task.md)  

**Ratschläge zu Hintergrundaufgaben**

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/previous-versions/hh974425(v=vs.110))

**Hintergrundaufgabe – API-Referenz**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)