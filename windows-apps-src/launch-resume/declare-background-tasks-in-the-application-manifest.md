---
title: Deklarieren von Hintergrundaufgaben im Anwendungsmanifest
description: Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 32472f698381f4b109f280f0b964f00cdbcec66a
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606199"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>Deklarieren von Hintergrundaufgaben im Anwendungsmanifest




**Wichtige APIs**

-   [**BackgroundTasks-Schema**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**Windows. applicationmodel. Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.

> [!Important]
>  Dieser Artikel befasst sich speziell mit Out-of-Process-Hintergrundaufgaben. In-Process-Hintergrundaufgaben werden nicht im Manifest deklariert.

Out-of-Process-Hintergrundaufgaben müssen im App-Manifest deklariert werden. andernfalls kann Ihre APP Sie nicht registrieren (es wird eine Ausnahme ausgelöst). Zudem müssen Out-of-Process-Hintergrundaufgaben im Anwendungsmanifest deklariert werden, um zertifiziert werden zu können.

In diesem Thema wird davon ausgegangen, dass Sie eine oder mehrere Hintergrundaufgabenklassen erstellt haben und dass Ihre App die Hintergrundaufgabe so registriert, dass sie als Reaktion auf mindestens einen Auslöser ausgeführt wird.

## <a name="add-extensions-manually"></a>Manuelles Hinzufügen von Erweiterungen


Öffnen Sie das Anwendungsmanifest (Package.appxmanifest), und wechseln Sie zum „Application“-Element. Erstellen Sie ein "Extensions"-Element (sofern nicht bereits eines vorhanden ist).

Der folgende Ausschnitt stammt aus dem [Hintergrundaufgabenbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask):

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>Hinzufügen einer Erweiterung für eine Hintergrundaufgabe  

Deklarieren Sie Ihre erste Hintergrundaufgabe.

Kopieren Sie diesen Code in das "Extensions"-Element (Attribute werden in den folgenden Schritten hinzugefügt).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Ändern Sie das EntryPoint-Attribut so, dass diese Zeichenfolge von Ihrem Code bei der Registrierung Ihrer Hintergrundaufgabe als Einstiegspunkt verwendet wird (**namespace.classname**).

    In diesem Beispiel ist „ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName“ der Einstiegspunkt:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Ändern Sie die Liste der Aufgabentypenattribute, um den für diese Hintergrundaufgabe verwendeten Typ der Aufgabenregistrierung anzugeben. Wenn die Hintergrundaufgabe mit mehreren Triggertypen registriert wird, fügen Sie für jeden Typ zusätzliche Task-Elemente und Type-Attribute hinzu.

    **Hinweis**  stellen Sie sicher, dass Sie jeden der von Ihnen verwendeten Auslösertypen auflisten oder wenn die Hintergrundaufgabe nicht mit den nicht deklarierten Auslösertypen registriert wird (die [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) -Methode schlägt fehl und löst eine Ausnahme aus).

    Dieses Beispiel für einen Codeausschnitt gibt die Verwendung von Systemereignistriggern und Pushbenachrichtigungen an:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>Hinzufügen mehrerer Hintergrundaufgaben Erweiterungen

Wiederholen Sie Schritt 2 für alle weiteren, von Ihrer App registrierten Hintergrundaufgabenklassen.

Das folgende Beispiel zeigt das vollständige "Application"-Element aus dem [Hintergrundaufgabenbeispiel]( https://code.msdn.microsoft.com/windowsapps/Background-Task-Sample-9209ade9): Es zeigt die Verwendung von zwei Hintergrundaufgabenklassen mit insgesamt drei Triggertypen. Kopieren Sie den Abschnitt „Extensions“ aus diesem Beispiel, und ändern Sie ihn nach Bedarf, um Hintergrundaufgaben im Anwendungsmanifest zu deklarieren.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>Deklarieren, wo Ihre Hintergrundaufgabe ausgeführt wird

Sie können angeben, wo Ihre Hintergrundaufgaben ausgeführt werden:

* Standardmäßig werden Sie im Prozess backgroundtaskhost. exe ausgeführt.
* In demselben Prozess wie Ihre Vordergrund Anwendung.
* Verwenden `ResourceGroup` Sie, um mehrere Hintergrundaufgaben in denselben Host Prozess zu platzieren oder um Sie in verschiedene Prozesse aufzuteilen.
* Verwenden `SupportsMultipleInstances` Sie, um den Hintergrundprozess in einem neuen Prozess auszuführen, der beim Auslösen eines neuen Auslösers seine eigenen Ressourcen Limits (Arbeitsspeicher, CPU) abruft.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>Ausführen in demselben Prozess wie Ihre Vordergrund Anwendung

Hier finden Sie ein XML-Beispiel, mit dem eine Hintergrundaufgabe deklariert wird, die im gleichen Prozess wie die Anwendung im Vordergrund ausgeführt wird.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

Wenn Sie **entryPoint**angeben, erhält die Anwendung einen Rückruf für die angegebene Methode, wenn der-Triggertyp ausgelöst wird. Wenn Sie keinen **entryPoint**angeben, empfängt Ihre Anwendung den Rückruf über [onbackgroundaktivierte ()](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated).  Ausführliche Informationen finden Sie [unter Erstellen und Registrieren eines in-Process-Hintergrund](create-and-register-an-inproc-background-task.md) Tasks.

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>Geben Sie an, wo die Hintergrundaufgabe mit dem Attribut ResourceGroup ausgeführt wird.

Hier finden Sie ein XML-Beispiel, mit dem eine Hintergrundaufgabe deklariert wird, die zwar in einem „BackgroundTaskHost.exe“-Prozess ausgeführt wird, der jedoch von anderen Hintergrundaufgabeninstanzen derselben App getrennt ist. Beachten Sie das Attribut `ResourceGroup`, das festlegt, welche Hintergrundaufgaben gemeinsam ausgeführt werden.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>Ausführung bei jedem Auslösen eines Triggers mit dem supportsmultipleinhaltungen-Attribut in einem neuen Prozess

In diesem Beispiel wird ein Hintergrund Task deklariert, der in einem neuen Prozess ausgeführt wird, der bei jedem Auslösen eines neuen Auslösers seine eigenen Ressourcen Limits (Arbeitsspeicher und CPU) abruft. Beachten Sie die Verwendung `SupportsMultipleInstances` von, die dieses Verhalten ermöglicht. Um dieses Attribut zu verwenden, müssen Sie die SDK-Version "10.0.15063" (Windows 10 Creators Update) oder höher als Ziel verwenden.

```xml
<Package
    xmlns:uap4="https://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances="True">
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> Sie können oder `ResourceGroup` `ServerName` nicht in Verbindung mit `SupportsMultipleInstances`angeben.

## <a name="related-topics"></a>Zugehörige Themen

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
