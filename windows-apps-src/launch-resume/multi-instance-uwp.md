---
title: Erstellen einer universellen Windows-App mit mehreren Instanzen
description: In diesem Thema wird beschrieben, wie Sie UWP-apps schreiben, die multiinstanziierung unterstützen.
keywords: UWP mit mehreren Instanzen
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e19425222459bb3bd56a5fe406ec76c0b7fb6b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164774"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Erstellen einer universellen Windows-App mit mehreren Instanzen

In diesem Thema wird beschrieben, wie Sie UWP-Apps (Multi-instance universelle Windows-Plattform) erstellen.

Ab Windows 10, Version 1803 (10,0; Build 17134): Ihre UWP-App kann sich für die Unterstützung mehrerer Instanzen entscheiden. Wenn eine Instanz einer UWP-App mit mehreren Instanzen ausgeführt wird und eine nachfolgende Aktivierungsanforderung eingeht, wird die vorhandene Instanz von der Plattform nicht aktiviert. Stattdessen wird eine neue Instanz erstellt, die in einem separaten Prozess ausgeführt wird.

> [!IMPORTANT]
> Multiinstanziierung wird für JavaScript-Anwendungen unterstützt, aber die Umleitung mit mehreren Instanzen ist nicht. Da die Umleitung mit mehreren Instanzen für JavaScript-Anwendungen nicht unterstützt wird, ist die [**appInstance**](/uwp/api/windows.applicationmodel.appinstance) -Klasse für solche Anwendungen nicht nützlich.

## <a name="opt-in-to-multi-instance-behavior"></a>Verhalten beim multiinstanzverhalten

Wenn Sie eine neue Anwendung mit mehreren Instanzen erstellen, können Sie die APP- [Projektvorlagen für mehrere Instanzen installieren. vsix](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps), verfügbar über das [Visual Studio Marketplace](https://marketplace.visualstudio.com/). Nachdem Sie die Vorlagen installiert haben, sind Sie im Dialogfeld " **Neues Projekt** " unter **Visual c# > Windows Universal** (oder **anderen Sprachen > Visual C++ > Windows Universal**) verfügbar.

Zwei Vorlagen sind installiert: eine **UWP-App mit mehreren Instanzen**, die die Vorlage zum Erstellen einer Multi-instance-APP und eine **UWP-App mit mehreren Instanzen**bereitstellt, die zusätzliche Logik bereitstellt, die Sie erstellen können, um entweder eine neue Instanz zu starten oder eine Instanz, die bereits gestartet wurde, selektiv zu aktivieren. Wenn Sie z. b. nur eine Instanz gleichzeitig das gleiche Dokument bearbeiten möchten, können Sie die Instanz, für die diese Datei geöffnet ist, im Vordergrund öffnen, anstatt eine neue Instanz zu starten.

Beide Vorlagen fügen `SupportsMultipleInstances` der `package.appxmanifest` Datei hinzu. Beachten Sie das Namespace Präfix `desktop4` und `iot2` : nur Projekte, die auf die Desktop-oder Internet der Dinge (IOT)-Projekte abzielen, unterstützen die mehrfach Instanziierung.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>Aktivierungs Umleitung für mehrere Instanzen

 Die Unterstützung mehrerer Instanzen für UWP-apps geht über die Möglichkeit hinaus, mehrere Instanzen der APP zu starten. Sie ermöglicht eine Anpassung in Fällen, in denen Sie auswählen möchten, ob eine neue Instanz der APP gestartet oder eine Instanz, die bereits ausgeführt wird, aktiviert ist. Wenn die APP beispielsweise gestartet wird, um eine Datei zu bearbeiten, die bereits in einer anderen Instanz bearbeitet wird, können Sie die Aktivierung an diese Instanz umleiten, anstatt eine andere Instanz zu öffnen, die die Datei bereits bearbeitet.

Sehen Sie sich dieses Video zum Erstellen von UWP-apps mit mehreren Instanzen an, um es in Aktion zu sehen.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

Die **UWP-App-Vorlage für multiinstanzumleitung** fügt `SupportsMultipleInstances` der Datei "Package. appxmanifest" (wie oben gezeigt) hinzu und fügt auch eine **Program.cs** (oder **Program. cpp**, wenn Sie die C++-Version der Vorlage verwenden) zu dem Projekt hinzu, das eine `Main()` Funktion enthält. Die Logik für das Umleiten der Aktivierung erfolgt in der `Main` Funktion. Die Vorlage für **Program.cs** ist unten dargestellt.

Die [**appInstance. Empfehlungs**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) Debug-Eigenschaft stellt die von der Shell bereitgestellte bevorzugte Instanz für diese Aktivierungs Anforderung dar (falls vorhanden `null` ). Wenn die Shell eine bevorzugte Einstellung bereitstellt, können Sie die Aktivierung an diese Instanz umleiten, oder Sie können Sie ignorieren, wenn Sie Sie auswählen.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` ist der erste Schritt, der ausgeführt wird. Sie wird vor [**OnStart**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) und [**onaktiviert**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Auf diese Weise können Sie bestimmen, ob diese oder eine andere Instanz aktiviert werden soll, bevor ein anderer Initialisierungs Code in der app ausgeführt wird.

Der obige Code bestimmt, ob eine vorhandene oder eine neue Instanz der Anwendung aktiviert ist. Ein Schlüssel wird verwendet, um zu bestimmen, ob eine Instanz vorhanden ist, die Sie aktivieren möchten. Wenn Ihre APP beispielsweise zum [Verarbeiten der Datei Aktivierung](./handle-file-activation.md)gestartet werden kann, können Sie den Dateinamen als Schlüssel verwenden. Anschließend können Sie überprüfen, ob eine Instanz Ihrer APP bereits mit diesem Schlüssel registriert ist, und Sie aktivieren, anstatt eine neue Instanz zu öffnen. Dies ist die Idee hinter dem Code: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Wenn eine Instanz gefunden wird, die mit dem Schlüssel registriert ist, wird diese Instanz aktiviert. Wenn der Schlüssel nicht gefunden wird, erstellt die aktuelle Instanz (die Instanz, die derzeit ausgeführt wird `Main` ) ihr Anwendungs Objekt und wird gestartet.

## <a name="background-tasks-and-multi-instancing"></a>Hintergrundaufgaben und multiinstanziierung

- Out-of-proc-Hintergrundaufgaben unterstützen multiinstanziierung. In der Regel führt jeder neue-Auslösers zu einer neuen Instanz der Hintergrundaufgabe (Obwohl technisch gesehen mehrere Hintergrundaufgaben im gleichen Host Prozess ausgeführt werden können). Dennoch wird eine andere Instanz der Hintergrundaufgabe erstellt.
- In-proc-Hintergrundaufgaben unterstützen keine multiinstanziierung.
- Hintergrund-Audioaufgaben unterstützen keine multiinstanziierung.
- Wenn eine APP eine Hintergrundaufgabe registriert, prüft sie in der Regel zunächst, ob die Aufgabe bereits registriert ist, und löscht Sie anschließend entweder und registriert Sie erneut, oder es wird keine Aktion durchgeführt, um die vorhandene Registrierung beizubehalten. Dies ist immer noch das typische Verhalten bei apps mit mehreren Instanzen. Allerdings kann eine APP mit mehreren Instanzen einen anderen Hintergrundaufgaben Namen auf instanzbasis registrieren. Dies führt zu mehreren Registrierungen für denselben-Triggern, und mehrere Hintergrund Task Instanzen werden aktiviert, wenn der-Triggervorgang ausgelöst wird.
- App-Dienste starten eine separate Instanz der APP-Service-Hintergrundaufgabe für jede Verbindung. Dies bleibt für apps mit mehreren Instanzen unverändert, wobei jede Instanz einer APP mit mehreren Instanzen eine eigene Instanz der APP-Service-Hintergrundaufgabe erhält. 

## <a name="additional-considerations"></a>Weitere Überlegungen

- Multiinstancing wird von UWP-Apps unterstützt, die auf Desktop-und Internet der Dinge Projekte (IOT) abzielen.
- Um Racebedingungen und Konfliktprobleme zu vermeiden, müssen apps mit mehreren Instanzen Schritte ausführen, um den Zugriff auf Einstellungen, den lokalen APP-Speicher und andere Ressourcen (z. b. Benutzer Dateien, einen Datenspeicher usw.) zu partitionieren bzw. zu synchronisieren, die von mehreren Instanzen gemeinsam genutzt werden können. Standard mäßige Synchronisierungs Mechanismen, z. b. Mutexen, Semaphore, Ereignisse usw., sind verfügbar.
- Wenn sich die APP `SupportsMultipleInstances` in der Datei "Package. appxmanifest" befindet, müssen die Erweiterungen nicht deklariert werden `SupportsMultipleInstances` . 
- Wenn Sie `SupportsMultipleInstances` zu einer anderen Erweiterung neben Hintergrundaufgaben oder App-Dienste hinzufügen und die APP, die die Erweiterung hostet, nicht auch `SupportsMultipleInstances` in der Datei "Package. appxmanifest" deklariert wird, wird ein Schema Fehler generiert.
- Apps können die Deklaration von [**ResourceGroup**](./declare-background-tasks-in-the-application-manifest.md) in ihrem Manifest verwenden, um mehrere Hintergrundaufgaben auf demselben Host zu gruppieren. Dies steht in Konflikt mit multiinstanziierung, wobei jede Aktivierung in einen separaten Host übergeht. Daher kann eine APP nicht sowohl `SupportsMultipleInstances` als auch `ResourceGroup` in ihrem Manifest deklarieren.

## <a name="sample"></a>Beispiel

Ein Beispiel für eine Aktivierungs Umleitung mit mehreren Instanzen finden Sie unter Beispiel für [mehrere](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) Instanzen.

## <a name="see-also"></a>Weitere Informationen

[AppInstance. findorregisterinstanceforkey](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_) 
 [AppInstance. getactivatedeventargs](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs) 
 [AppInstance. redirectactivationto](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo) 
 [Behandeln der APP-Aktivierung](./activate-an-app.md)