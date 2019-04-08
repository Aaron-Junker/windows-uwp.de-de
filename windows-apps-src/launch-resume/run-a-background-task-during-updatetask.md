---
title: Ausführen einer Hintergrundaufgabe beim Aktualisieren der UWP-App
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die ausgeführt wird, wenn die Store-App Ihrer Universellen Windows-Plattform (UWP) aktualisiert wird.
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10 "," Uwp "," Update "," Hintergrundaufgabe "," Updatetask "," Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 8cd7d4494340d1c5e617361f2e3d750b35ebabb9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603525"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ausführen einer Hintergrundaufgabe beim Aktualisieren der UWP-App

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die ausgeführt wird, nachdem Ihre universelle Windows-Plattform-Store-App (UWP) aktualisiert wurde.

The Hintergrundaufgabe „Update-Aufgabe“ wird vom Betriebssystem aufgerufen, nachdem der Benutzer ein Update einer auf dem Gerät installierten App durchgeführt hat. Dadurch kann Ihre App Initialisierungsaufgaben durchführen, etwa die Initialisierung eines neuen Push-Benachrichtigungskanals, die Aktualisierung des Datenbankschemas usw., bevor der Benutzer Ihre aktualisierte App startet.

Die Update-Aufgabe unterscheidet sich vom Starten einer Hintergrundaufgabe mit dem [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)-Auslöser, da Ihre App in diesem Fall mindestens einmal ausgeführt werden muss, bevor sie aktualisiert wird, um die Hintergrundaufgabe zu registrieren, die durch den **ServicingComplete**-Auslöser aktiviert wird.  Die Update-Aufgabe wird nicht registriert, und daher wird bei einer App, die noch nicht ausgeführt wurde, die aber aktualisiert wurde, dennoch die Update-Aufgabe ausgelöst.

## <a name="step-1-create-the-background-task-class"></a>Schritt 1: Erstellen Sie die Hintergrund-Task-Klasse

Wie bei anderen Arten von Hintergrundaufgaben implementieren Sie die Hintergrundaufgabe „Update-Aufgabe“ als eine Komponente für Windows-Runtime. Befolgen Sie zum Erstellen dieser Komponente die Schritte im Abschnitt **Erstellen der Hintergrundaufgabenklasse** im Artikel [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Die Schritte sind folgende:

- Hinzufügen einer Komponente für Windows-Runtime zu Ihrer Lösung
- Erstellen einer Referenz von einer App zur Komponente
- Erstellen einer öffentlichen, versiegelten Klasse in der Komponente, die [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) implementiert.
- Implementieren der [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode, die den erforderlichen Einstiegspunkt darstellt, der aufgerufen wird, wenn die Update-Aufgabe ausgeführt wird. Wenn Sie beabsichtigen, von Ihrer Hintergrundaufgabe asynchrone Aufrufe auszuführen, finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) Erläuterungen dazu, wie Sie in Ihrer **Run**-Methode eine Verzögerung verwenden.

Sie müssen diese Hintergrundaufgabe nicht registrieren (der Abschnitt „Registrieren der auszuführenden Hintergrundaufgabe“ im Thema **Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen**), um die Update-Aufgabe zu verwenden. Dies ist der Hauptgrund für die Verwendung einer Update-Aufgabe, da Sie Ihrer App zum Registrieren der Aufgabe keinen Code hinzufügen müssen, und die App muss zum Registrieren der Hintergrundaufgabe nicht mindestens einmal ausgeführt werden, bevor sie aktualisiert wird.

Der folgende Beispielcode zeigt einen sehr einfachen Startpunkt für eine Update-Aufgaben-Hintergrundaufgabenklasse in C#. Die Hintergrundaufgabenklasse selbst sowie alle anderen Klassen im Hintergrundaufgabenprojekt müssen **öffentlich** und **versiegelt** sein. Ihre Hintergrundaufgabenklasse muss sich von **IBackgroundTask** ableiten und eine öffentliche **Run()**-Methode mit der folgenden Signatur besitzen:

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Schritt 2: Die Hintergrundaufgabe im Paketmanifest deklarieren

Klicken Sie im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf die Datei **Package.appxmanifest**, und klicken Sie auf **Code anzeigen**, um das Paketmanifest anzuzeigen. Fügen Sie den folgenden `<Extensions>`-XML-Code hinzu, um Ihre Update-Aufgabe zu deklarieren:

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

Stellen Sie beim obigen XML-Code sicher, dass das `EntryPoint`-Attribut auf den namespace.class-Namen Ihrer Update-Klassenaufgabe festgelegt ist. Beim Namen wird Groß- und Kleinschreibung beachtet.

## <a name="step-3-debugtest-your-update-task"></a>Schritt 3: Debuggen/Test die Update-Aufgabe

Stellen Sie sicher, dass Sie Ihre App auf Ihrem Computer bereitgestellt haben, sodass etwas aktualisiert werden kann.

Legen Sie einen Haltepunkt in der Run()-Methode Ihrer Hintergrundaufgabe fest.

![Haltepunkt festlegen](images/run-func-breakpoint.png)

Klicken Sie als Nächstes im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt Ihrer App (nicht auf das Hintergrundaufgabenprojekt), und klicken Sie dann auf **Eigenschaften**. Klicken Sie im Eigenschaftenfenster der Anwendung auf **Debuggen** auf der linken Seite, und wählen Sie dann **Eigenen Code zunächst nicht starten, sondern debuggen** aus:

![Debugeinstellungen festlegen](images/do-not-launch-but-debug.png)

Um sicherzustellen, dass die UpdateTask ausgelöst wird, erhöhen Sie als Nächstes die Versionsnummer des Pakets. Doppelklicken Sie im Projektmappen-Explorer auf die Datei **Package.appxmanifest** Ihrer App, um den Paket-Designer zu öffnen, und aktualisieren Sie dann die **Build**-Nummer:

![Version aktualisieren](images/bump-version.png)

Wenn Sie jetzt in Visual Studio 2017 F5 drücken, wird Ihre App aktualisiert, und das System aktiviert Ihre UpdateTask-Komponente im Hintergrund. Der Debugger wird automatisch an den Hintergrundprozess angehängt. Der Haltepunkt wird erreicht, und Sie können Ihre Update-Code-Logik durchlaufen.

Wenn die Hintergrundaufgabe abgeschlossen ist, können Sie in der gleichen Debug-Sitzung über das Windows-Startmenü die Vordergrund-App starten. Der Debugger wird wieder automatisch angehängt, diesmal an den Vordergrundprozess, und Sie können die Logik Ihrer App durchlaufen.

> [!NOTE]
> Visual Studio 2015-Benutzer: Die oben genannten Schritte gelten für Visual Studio 2017. Wenn Sie Visual Studio 2015 verwenden, können Sie dieselben Techniken zum Auslösen und Tasten der UpdateTask verwenden, wobei Visual Studio aber nicht daran angefügt wird. Alternativ können Sie in VS 2015 einen [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) einrichten, der die UpdateTask als Einstiegspunkt festlegt, und die Ausführung direkt in der Vordergrund-App auslösen.

## <a name="see-also"></a>Siehe auch

[Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
