---
title: Ausführen einer Hintergrundaufgabe beim Aktualisieren der UWP-App
description: Erfahren Sie, wie Sie einen Hintergrund Task erstellen, der ausgeführt wird, wenn ihre universelle Windows-Plattform Store-App (UWP) aktualisiert wird.
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, UWP, Update, Hintergrundaufgabe, Updatetask, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: ff4dd5487357728b6a79f4c4d31a437075fcd006
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164804"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ausführen einer Hintergrundaufgabe beim Aktualisieren der UWP-App

Erfahren Sie, wie Sie eine Hintergrundaufgabe schreiben, die ausgeführt wird, nachdem die universelle Windows-Plattform (UWP) Store-APP aktualisiert wurde.

Der Task "Task Hintergrund aktualisieren" wird vom Betriebssystem aufgerufen, nachdem der Benutzer ein Update für eine APP installiert hat, die auf dem Gerät installiert ist. Dadurch kann Ihre APP Initialisierungs Aufgaben ausführen, wie z. b. das Initialisieren eines neuen pushbenachrichtigungskanals, das Aktualisieren des Datenbankschemas usw., bevor der Benutzer die aktualisierte APP aufruft.

Der Aktualisierungs Task unterscheidet sich vom Starten einer Hintergrundaufgabe mithilfe des [servicingcomplete](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) -Auslösers, da die app in diesem Fall mindestens einmal ausgeführt werden muss, bevor Sie aktualisiert wird, um die Hintergrundaufgabe zu registrieren, die durch den **servicingcomplete** -Befehl aktiviert wird.  Der Aktualisierungs Task ist nicht registriert, und eine APP, die noch nicht ausgeführt wurde, für die jedoch ein Upgrade ausgeführt wurde, wird weiterhin die Aktualisierungs Aufgabe auslösen.

## <a name="step-1-create-the-background-task-class"></a>Schritt 1: Erstellen der Hintergrundaufgaben Klasse

Wie bei anderen Arten von Hintergrundaufgaben implementieren Sie den Task Hintergrundaufgabe aktualisieren als Windows-Runtime Komponente. Um diese Komponente zu erstellen, führen Sie die Schritte im Abschnitt erstellen **der Hintergrundaufgaben Klasse** unter [Erstellen und Registrieren eines Out-of-Process-Hintergrund Tasks aus](./create-and-register-a-background-task.md). Dazu müssen folgende Schritte ausgeführt werden:

- Fügen Sie der Projekt Mappe ein Windows-Runtime Komponenten Projekt hinzu.
- Erstellen eines Verweises von Ihrer APP auf die Komponente.
- Erstellen einer öffentlichen, versiegelten Klasse in der Komponente, die [**ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)implementiert.
- Implementieren der [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) -Methode, bei der es sich um den erforderlichen Einstiegspunkt handelt, der beim Ausführen der Update Aufgabe aufgerufen wird. Wenn Sie asynchrone Aufrufe von ihrer Hintergrundaufgabe ausführen möchten, [Erstellen und registrieren Sie eine Out-of-Process-Hintergrundaufgabe](./create-and-register-a-background-task.md) , die die Verwendung einer Verzögerung in der Ausführungs **Methode erläutert** .

Sie müssen diese Hintergrundaufgabe nicht registrieren (den Abschnitt "Hintergrundaufgabe zum Ausführen registrieren" im Thema **Erstellen und Registrieren eines Out-of-Process-Hintergrund** Tasks), um den Aktualisierungs Task zu verwenden. Dies ist der Hauptgrund für die Verwendung einer Aktualisierungs Aufgabe, da Sie Ihrer APP keinen Code hinzufügen müssen, um die Aufgabe zu registrieren, und die APP muss nicht mindestens einmal ausgeführt werden, bevor Sie aktualisiert wird, um die Hintergrundaufgabe zu registrieren.

Der folgende Beispielcode zeigt einen grundlegenden Ausgangspunkt für eine Aufgaben Klasse des Hintergrund Tasks für Aktualisierungs Aufgaben in c#. Die Hintergrundaufgaben Klasse selbst und alle anderen Klassen im Hintergrundaufgaben Projekt müssen **öffentlich** und **versiegelt**sein. Ihre Hintergrundaufgaben Klasse muss von **ibackgroundtask** abgeleitet werden und eine Public **Run ()** -Methode mit der folgenden Signatur aufweisen:

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Schritt 2: Deklarieren der Hintergrundaufgabe im Paket Manifest

Klicken Sie in der Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf **Package. appxmanifest** , und klicken Sie auf **Code anzeigen** , um das Paket Manifest anzuzeigen. Fügen Sie den folgenden XML-Code hinzu `<Extensions>` , um den Aktualisierungs Task zu

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

Stellen Sie im obigen XML-Code sicher, dass das- `EntryPoint` Attribut auf den Namespace. Class-Namen Ihrer Update-Aufgaben Klasse festgelegt ist. Bei dem Namen wird die Groß- und Kleinschreibung berücksichtigt.

## <a name="step-3-debugtest-your-update-task"></a>Schritt 3: Debuggen/testen des Aktualisierungs Tasks

Stellen Sie sicher, dass Sie die APP auf Ihrem Computer bereitgestellt haben, damit etwas zu aktualisieren ist.

Legen Sie einen Haltepunkt in der Run ()-Methode Ihrer Hintergrundaufgabe fest.

![Haltepunkt festlegen](images/run-func-breakpoint.png)

Klicken Sie anschließend im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt Ihrer APP (nicht auf das Projekt "Hintergrundaufgabe") und dann auf **Eigenschaften**. Klicken Sie im Anwendungs Eigenschaftenfenster auf der linken Seite auf **Debuggen** , und wählen Sie dann **nicht starten, sondern eigenen Code Debuggen**aus:

![Festlegen der Debugeinstellungen](images/do-not-launch-but-debug.png)

Um sicherzustellen, dass Updatetask ausgelöst wird, erhöhen Sie als nächstes die Versionsnummer des Pakets. Doppelklicken Sie im Projektmappen-Explorer auf die Datei " **Package. appxmanifest** " Ihrer APP, **um den Paket** -Designer zu öffnen, und aktualisieren Sie dann die Buildnummer:

![Aktualisieren der Version](images/bump-version.png)

Wenn Sie jetzt F5 drücken, wird Ihre APP in Visual Studio 2019 aktualisiert, und das System aktiviert die Updatetask-Komponente im Hintergrund. Der Debugger wird automatisch an den Hintergrundprozess angefügt. Der Breakpoint wird angezeigt, und Sie können die Update Code Logik schrittweise durchlaufen.

Wenn die Hintergrundaufgabe abgeschlossen ist, können Sie die Vordergrund-App im Windows-Startmenü innerhalb derselben Debugsitzung starten. Der Debugger wird automatisch an den Vordergrund Prozess angehängt, und Sie können die Logik der APP schrittweise durchlaufen.

> [!NOTE]
> Visual Studio 2015-Benutzer: die oben genannten Schritte gelten für Visual Studio 2017 oder Visual Studio 2019. Wenn Sie Visual Studio 2015 verwenden, können Sie die gleichen Techniken verwenden, um Updatetask zu aktivieren und zu testen, mit der Ausnahme, dass Visual Studio nicht an Sie angefügt wird. Eine alternative Vorgehensweise in vs 2015 besteht darin, einen [applicationtrigger](./trigger-background-task-from-app.md) einzurichten, der Updatetask als Einstiegspunkt festlegt und die Ausführung direkt aus der Vordergrund-App auslöst.

## <a name="see-also"></a>Weitere Informationen

[Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](./create-and-register-a-background-task.md)