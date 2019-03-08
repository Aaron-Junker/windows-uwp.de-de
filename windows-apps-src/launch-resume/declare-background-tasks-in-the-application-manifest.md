---
title: Deklarieren von Hintergrundaufgaben im Anwendungsmanifest
description: Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgaben
ms.localizationpriority: medium
ms.openlocfilehash: 4b30cd39a4440a1ade1ea0dda5a35d3f7c15f963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615675"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>Deklarieren von Hintergrundaufgaben im Anwendungsmanifest




**Wichtige APIs**

-   [**BackgroundTasks-Schema**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.

> [!Important]
>  Dieser Artikel befasst sich speziell mit Out-of-Process-Hintergrundaufgaben. In-Process-Hintergrundaufgaben werden nicht im Manifest deklariert.

Out-of-Process-Hintergrundaufgaben müssen im Anwendungsmanifest deklariert sein, da Ihre App diese ansonsten nicht registrieren kann (eine Ausnahme wird ausgelöst). Zudem müssen Out-of-Process-Hintergrundaufgaben im Anwendungsmanifest deklariert werden, um zertifiziert werden zu können.

In diesem Thema wird davon ausgegangen, dass Sie eine oder mehrere Hintergrundaufgabenklassen erstellt haben und dass Ihre App die Hintergrundaufgabe so registriert, dass sie als Reaktion auf mindestens einen Auslöser ausgeführt wird.

## <a name="add-extensions-manually"></a>Manuelles Hinzufügen von Erweiterungen


Öffnen Sie das Anwendungsmanifest (Package.appxmanifest), und wechseln Sie zum „Application“-Element. Erstellen Sie ein "Extensions"-Element (sofern nicht bereits eines vorhanden ist).

Der folgende Ausschnitt stammt aus dem [Hintergrundaufgabenbeispiel](https://go.microsoft.com/fwlink/p/?LinkId=618666):

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

    **Beachten Sie**  prüfen Sie zum Auflisten aller Triggertypen Sie verwenden, und die Hintergrundaufgabe wird nicht mit den nicht deklarierten Triggertypen registriert (die [ **registrieren** ](https://msdn.microsoft.com/library/windows/apps/br224772) wird fehl und löst eine Ausnahme).

    Dieses Beispiel für einen Codeausschnitt gibt die Verwendung von Systemereignistriggern und Pushbenachrichtigungen an:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>Hinzufügen von weiteren Hintergrundaufgabenerweiterungen

Wiederholen Sie Schritt 2 für alle weiteren, von Ihrer App registrierten Hintergrundaufgabenklassen.

Das folgende Beispiel zeigt das vollständige "Application"-Element aus dem [Hintergrundaufgabenbeispiel]( https://go.microsoft.com/fwlink/p/?linkid=227509): Es zeigt die Verwendung von zwei Hintergrundaufgabenklassen mit insgesamt drei Triggertypen. Kopieren Sie den Abschnitt „Extensions“ aus diesem Beispiel, und ändern Sie ihn nach Bedarf, um Hintergrundaufgaben im Anwendungsmanifest zu deklarieren.

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

## <a name="declare-where-your-background-task-will-run"></a>Deklarieren Sie die Position, an der Ihre Hintergrundaufgabe ausgeführt wird

Sie können angeben, wo Ihre Hintergrundaufgaben ausführt werden sollen:

* Standardmäßig werden sie im Prozess „BackgroundTaskHost.exe“ ausgeführt.
* Im gleichen Prozess wie Ihre Anwendung im Vordergrund.
* Verwenden Sie `ResourceGroup`, um mehrere Hintergrundaufgaben im gleichen Hostprozess einzufügen, oder diese in verschiedene Prozesse aufzuspalten.
* Verwenden Sie `SupportsMultipleInstances`, um den Hintergrundprozess in einem neuen Prozess auszuführen, der, jedes Mal, wenn ein neuer Trigger ausgelöst wird, eigene Ressourcenbeschränkungen (Arbeitsspeicher, CPU) erhält.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>Die Ausführung erfolgt im gleichen Prozess wie Ihre Anwendung im Vordergrund.

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

Wenn Sie den **Einsprungpunkt** festlegen, erhält Ihre Anwendung einen Rückruf an die angegebene Methode, sobald der Trigger ausgelöst wird. Wenn Sie den **Einsprungpunkt** nicht festlegen, erhält Ihre Anwendung den Rückruf über  [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).  Weitere Informationen finden Sie unter [Erstellen und Registrieren einer In-Process-Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>Legen Sie fest, wo Ihre Hintergrundaufgabe mit dem Attribut „ResourceGroup“ ausgeführt werden soll.

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

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>Ausführen in einem neuen Prozess, jedes Mal, wenn ein Trigger mit dem Attribut „SupportsMultipleInstances“ ausgelöst wird

In diesem Beispiel wird eine Hintergrundaufgabe deklariert, die in einem neuen Prozess ausgeführt wird, der, jedes Mal, wenn ein neuer Trigger ausgelöst wird, eigene Ressourcenbeschränkungen (Arbeitsspeicher und CPU) erhält. Beachten Sie die Verwendung von `SupportsMultipleInstances`, zur Aktivierung dieses Verhalten. Um dieses Attribut verwenden, müssen Sie als Ziel SDK, Version "10.0.15063" (Windows 10 Creators Update) oder höher.

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> `ResourceGroup` oder `ServerName` können in Verbindung mit `SupportsMultipleInstances` nicht festgelegt werden.

## <a name="related-topics"></a>Verwandte Themen

* [Eine Hintergrundaufgabe Debuggen](debug-a-background-task.md)
* [Eine Hintergrundaufgabe registrieren](register-a-background-task.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
