---
title: XBL In Unity prefabs (Vorlagen), und melden Sie sich
description: Behandelt die sozialen Prefabs und Skriptbeispiele für soziale Dienste auf Xbox Live
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660035"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity prefabs (Vorlagen) und Skript-Anmeldung

Dieser Artikel führt Sie durch das Xbox Live-Anmeldung für Ihr Unity-Projekte hinzufügen. Es gibt zwei Möglichkeiten, erreichen Sie Anmeldung, wenn Sie heruntergeladen haben die [Xbox Live Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin). Sie können den prefabs (Vorlagen) innerhalb des Plug-Ins verwenden oder verwenden Sie die Skripts und die enthaltenen Bibliotheken, Xbox Live-Anmeldung in Ihre eigenen benutzerdefinierten "gameobjects" Skript für.

> [!IMPORTANT]
> Dieser Artikel bezieht sich auf eine Version des Plug-Ins vor einem Update im Mai 2018 (Version 1804). Wenn Sie die Xbox Live-Plug-in nach dieser Zeit installiert haben oder noch nicht heruntergeladen müssen Sie eine neuere Version möglicherweise die wesentliche Unterschiede, wie die Anmeldung erfolgt ist. Darüber hinaus werden Sie feststellen, dass die Screenshots in diesem-Plug-in die von der neuesten Version nicht übereinstimmen. Stattdessen finden Sie in der [Artikel für die aktualisierte Anmeldung Prefab](playerauthentication-prefab-sign-in.md) sowie [im Artikel über die aktualisierte Methoden für die Anmeldung Skripterstellung](sign-in-manager.md).

## <a name="before-you-begin"></a>Vorbemerkungen

Bevor Sie beginnen, Ihr Unity-Spiel Xbox Live Sozialdienste hinzugefügt stehen einige Schritte, die Sie abgeschlossen ist, bevor Sie sich damit beschäftigen müssen. Stellen Sie zunächst sicher, dass Sie heruntergeladen und integriert die [Xbox Live Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin). Andererseits sollten Sie damit Ihre Titel reserviert und veröffentlicht über die [Microsoft Development Center](https://developer.microsoft.com/en-us/games/uwp). Lesen [Erstellen eines neuen Titels für die Xbox Live Creators-Programm](../get-started-with-creators/create-and-test-a-new-creators-title.md) Anweisungen zum Veröffentlichen des Titels.
Lesen Sie schließlich [In Unity Live Xbox konfigurieren](../get-started-with-creators/configure-xbox-live-in-unity.md) Ihre Unity-Umgebung ordnungsgemäß eingerichtet, und konfigurieren Ihre Titel ein, der Xbox Live-Dienste verwenden. Sobald Ihr Unity-Projekt ordnungsgemäß eingerichtet ist, ist es Zeit, erfahren Sie mehr über die Tools können Sie in der Titelleiste Ihres Xbox Live aktiviert als auch die beiden Hauptverfahren, Sie können einen Xbox Live in Unity implementieren: prefabs (Vorlagen) und Skripts.

## <a name="prefabs"></a>Prefabs (Vorlagen)

Unity verfügt über ein prefab Asset-Typ, der Ihnen ermöglicht, ein "gameobject" mit Komponenten und Eigenschaften zu speichern. Die prefab fungiert als Vorlage aus der Sie neue Instanzen in Ihrer Unity-Szene erstellen können.
[Erfahren Sie mehr über prefabs (Vorlagen) von der Unity-Website](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage).

Die Xbox Live Unity-Plug-in bietet einige prefabs (Vorlagen), die Sie in Ihrem Projekt verwenden können, verwenden Xbox Live-Funktionen. Die in diesem Artikel beschriebenen prefabs (Vorlagen) können Sie zur Anmeldung [Hinzufügen von Unterstützung für mehrere Benutzer](../get-started-with-creators/add-multi-user-support.md) Profil zum Titel oder Anzeigen der Freundesliste von einem angemeldeten Xbox Live. Sie finden diese und andere prefabs (Vorlagen) unter den **Projekt** Registerkarte anhand des Pfads: **Bestand > Xbox Live > Prefabs**.

### <a name="the-userprofile-prefab"></a>Das UserProfile-prefab

Die erste und wichtigste social-Prefab stammt die **UserProfile** prefab. Die **UserProfile** Prefab bietet alles, was erforderlich, um eine Xbox Live-Anmeldung zu ermöglichen. Dies ist besonders wichtig, da Sie einem Benutzer anmelden müssen vor der Verwendung von Xbox Live-Dienste. Das Prefab enthält die Schaltfläche "Anmelden" und einem "gameobject" So stellen Sie eine Protokollierung im Spieler durch ihre Gamertag, Gamerpic und Gamerscore dar.

> [!NOTE]
> Um eines der anderen Xbox Live prefabs (Vorlagen) zu verwenden, müssen Sie enthalten eine **UserProfile** prefab oder manuell aufrufen, die API-Anmeldung.

![UserProfile-Prefab in Objekte und Hierarchie](../images/unity/unity-userprofile-views.png)

Erweitern der **UserProfile** prefab in die **Projekt** Bereich oder in der **Hierarchie** nachdem er in einer Szene hinzugefügt wurde, sehen Sie, dass die **UserProfile**  Prefab enthält zwei "gameobjects" darin. Das erste Objekt dem **SignInPanel** enthält die Oberfläche der Schaltfläche "Anmelden". Das zweite Objekt dem **ProfileInfo** der enthält die Informationen über den Benutzer nach der Anmeldung. Die **UserProfile** Prefab stammt, was Sie verwendet werden, um die Informationen eines Benutzers angemeldet lokal auf dem Titel des Xbox Live.

### <a name="the-xboxliveuser-prefab"></a>Das Prefab XboxLiveUser

Die **UserProfile** Prefab verwendet ein zweiten soziale prefabs im Code wird aufgerufen, die **XboxLiveUser**. Diese Prefab Verwendung ist nicht sofort ersichtlich, da dies nicht die szenenhierarchie hinzugefügt werden, da es einfach im Code instanziiert werden kann. Die **XboxLiveUser** verfügt über keine visuelle Darstellung, es enthält lediglich die Details, die auf der Xbox Live-Benutzer beziehen. Sie benötigen eine Instanz von der **XboxLiveUser** für jede Instanz des der **UserProfile**. Dies ist wichtig, wenn [Hinzufügen von Unterstützung für mehrere Benutzer](../get-started-with-creators/add-multi-user-support.md) auf den Titel. Zusätzlich zu enthält Informationen über den Benutzer nach Anmeldung ist diese Prefab auch einen Wrapper für den Code, mit einem Xbox Live-Benutzer anmelden.

## <a name="sign-in-with-the-userprofile-prefab"></a>Melden Sie sich mit den UserProfile-prefab

Die Xbox Live Unity-Plug-Ins prefabs (Vorlagen) vorhanden sein, um bestimmte Aufgaben bei der Entwicklung zu vereinfachen. Xbox Live-Anmeldung für Ihr Unity-Projekt aktivieren Sie einfach zu verwenden, müssen die **UserProfile** und **XboxLiveServices** prefabs (Vorlagen), zusammen mit einer Unity **EventSystem**.

Ziehen Sie zunächst die **UserProfile** prefab in einer Szene. Im Idealfall die **UserProfile** platziert werden soll, auf dem Bildschirm anfängliche Menü Ihres Projekts.

![Ziehen Sie Userprofile Hierarchie](../images/unity/drag-userprofile.gif)

Zusätzlich zu den **UserProfile** Prefab, Sie auch sicherstellen, dass müssen, die **XboxLiveServices** Prefab in mindestens die erste Szene Ihres Projekts vorhanden ist.
Die **XboxLiveServices** Prefab ermöglicht es Ihnen zu wechseln, und zwar unabhängig davon, ob bestimmte prefabs (Vorlagen) das Protokollieren von Informationen für das Debuggen zu werden. Dies ist hilfreich, um auf das prefab Verhalten sicherzustellen.

![Überprüfen Sie für das prefab xboxliveservices](../images/unity/check-for-xboxliveservices.gif)

Zum Schluss die **UserProfile** erfordert auch eine **EventSystem** ordnungsgemäß ausgeführt. Dies kann hinzugefügt werden, indem Sie mit der rechten Maustaste auf die Szene-Anmeldung, und klicken Sie dann durch Klicken **"gameobject"--> UI--> EventSystem**.

![Hinzufügen von Ereignissen](../images/unity/add_event_system.gif)

Wenn Sie den Spielmodus eingeben, wird der Dienst automatisch in einem vom Benutzer anmelden. In Unity das Xbox Live SDK simuliert Aufrufe des Xbox Live-Diensts und sendet wieder gefälschte Daten für die Arbeit mit. Um live-Daten anzuzeigen, müssen Sie das Projekt als UWP-Anwendung erstellen, und führen Sie sie aus Visual Studio. Finden Sie unter [konfigurieren Xbox Live in Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) für Weitere Informationen. Bei der Eingabe Spielmodus im Unity, das Prefab wird mit falschen Daten, die Informationen, z. B. Gamertag, Gamerpic und Gamerscore, der ein Spieler zu simulieren, werden aufgefüllt. Dies sind die Informationen, die von angezeigt werden soll, sollten die **UserProfile** prefab.

Eine erfolgreiche Anmeldung wird wie folgt aussehen: ![erfolgreich Userprofile Play](../images/unity/correct-user-profile-play.gif)

Wenn Sie Ihr Projekt für die Verbindung zu Xbox Live nicht konfiguriert haben wird ordnungsgemäß Eingabe Spielmodus deaktivieren die Schaltfläche "Anmelden" und eine Fehlermeldung angezeigt.

Folgendes ist ein Beispiel einer fehlerhaften Anmeldung aufgrund einer ungültigen Xbox Live-app-Konfiguration.
![Fehler bei Userprofile play](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>Scripting-Anmeldung

Nun, dass Sie wissen, wie Sie mit der **UserProfile** prefab es am besten wäre, sich das zugrunde liegende Skript zu suchen, die die Funktionalität der prefabs bestimmt. Bei Betrachtung der **UserProfile** in die **Inspektor** sehen Sie, dass sie verfügt über eine **UserProfile.cs** Skript angefügt ist. Dieses Skript enthält alles, was Sie benötigen, melden Sie sich ein Benutzer, und laden die Profilinformationen, die Sie bei Anmeldung anzeigen möchten. Allerdings werden anstatt der gesamten Prefab (die im Laufe der Zeit aktualisiert werden kann) wir sehen uns an, einige Beispielzeilen des Codes zu verstehen, was benötigt wird, eine Xbox Live-Benutzer anmelden.

### <a name="the-xboxliveuser-class"></a>Die XboxLiveUser-Klasse

Die Aufrufe benötigt ein Benutzer anmelden umschlossen sind die `XboxLiveUserInfo` Klasse. In der **UserProfile.cs** Skript sehen Sie, dass es eine Instanz ist der `XboxLiveUserInfo` Klasse mit dem Namen `XboxLiveUser`. Wir verwenden den gleichen Variablennamen in unserem Beispiel. Die `XboxLiveUserInfo` -Klasse enthält eine Instanz von der `XboxLiveUser` Klasse mit dem Namen `User` als eine der zugehörigen Membervariablen. Die `XboxLiveUser` Klasse enthält, die für die Anmeldung erforderlichen Funktionen für Anmeldung die `XboxLiveUser`. Verwenden Sie die Instanz von der `XboxLiveUser` Klasse `User` anmelden auch Benutzer, Informationen zu erhalten, die sie beschreibt, wie ihre Gamertag, Gamerpic und Gamerscore. Zu diesem Zweck müssen Sie eine Instanz von Initialisieren der `XboxLiveUserInfo` Klasse, und verwenden Sie die resultierende `XboxLiveUserInfo.User` zu Aufruf-Anmeldung.

### <a name="initialize-the-xboxliveuser"></a>Initialisieren der XboxLiveUser

Initialisieren das Xbox Live-Benutzer ist der erste Schritt vor dem Signieren sie tatsächlich in Xbox Live. Dies geschieht sehr einfach in Code, indem Sie mit der `XboxLiveUserInfo.Initialize()` Funktion.
In unserem Beispiel verwenden wir die `XboxLiveUser` Membervariablen als unsere `XboxLiveUserInfo` Instanz, und initialisieren, die für die Anmeldung verwenden.

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

Dieser Beispielcode betrachten Sie sehen, die die `XboxLiveUserInfo.Initialize()` Funktion wird aufgerufen, als Reaktion auf einen Klick auf. Die vollständige **UserProfile.cs** prefab Skript enthält Code, der für die automatische Anmeldung ermöglicht es, in denen `XboxLiveUserInfo.Initialize()` ohne Interaktion aufgerufen wird.
Die `XboxLiveUserInfo.Initialize()` Funktion erstellt eine neue `XboxLiveUserInfo.User` das können wir die Anmeldung Funktionen in Aufrufen der `XboxLiveUserInfo.User` Klasse.

### <a name="call-sign-in"></a>Aufruf-Anmeldung

Sobald die XboxLiveUser initialisiert wurde Zeit es zu Aufruf-Anmeldung. In **UserProfile.cs** -Anmeldung wird aufgerufen, der `SignInAsync()` UserProfile.cs Funktion. Im vorherigen Beispielcode wir einfach warten, die `XboxLiveUser` initialisiert werden, vor dem Aufruf der `SignInAsync()` Funktion.

> [!NOTE]
> Es ist notwendig, warten, bis die `XboxLiveUser` initialisiert werden, bevor Anmeldung aufgerufen werden, da die `XboxLiveUser` enthält die `XboxLiveUser.User` Eigenschaft, die zu Aufruf-Anmeldung verwendet wird.

In **UserProfile.cs** der `SignInAsync()` -Funktion enthält zwei Sign-in Funktionen, die zum Anmelden des Benutzers verwendet werden können. `XboxLiveUser.User.SignInSilentlyAsync()` und `XboxLiveUser.User.SignInAsync()` Hierbei handelt es sich um die Funktionen, die der Benutzer sich anmelden. Die `SignInAsync()` Funktion ist ein gutes Beispiel dafür, wie Sie diese Funktionen ordnungsgemäß zu verwenden. Das folgende Codebeispiel zeigt eine geeignete Methode zum Aufrufen der beiden Funktionen Anmeldung:

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

In diesem Beispiel werden die Ergebnisse der Anmeldung ruft in der Variablen gespeichert `signInStatus`. Dadurch können wir überprüfen, ob die Anmeldung erfolgreich war, und entsprechend agiert. In diesem Beispiel wird die Funktion zuerst versucht, sich anmelden im Hintergrund, wenn die Funktion für die automatische Anmeldung schlägt fehl Ruft die normale Zeichen in der Funktion. Nachdem Sie einen erfolgreichen Aufruf auf eine der Funktionen-Anmeldung, dass Sie in der Benutzer sich angemeldet haben werden haben. Sie können nun die `XboxLiveUser.User` zum Abrufen und Anzeigen von Details zu den angemeldeten Benutzer. Sehen Sie sich die `LoadProfileInfo()` -Funktion in **UserProfile.cs** ein Beispiel zur Verwendung der `XboxLiveUser.User` zur Anzeige von Informationen über einen angemeldeten Benutzer.

## <a name="build-and-test-sign-in"></a>Erstellen und Testen von-Anmeldung

Wenn Ihre Titel im Editor ausgeführt wird, sehen Sie die gefälschte Daten, wenn Sie versuchen, Xbox Live-Funktionalität verwenden. Melden Sie sich mit einem echten Profil, und Testen Sie das Xbox Live-Funktionalität in den Titel, müssen Sie zum Erstellen einer UWP-Projektmappe, und führen Sie es in Visual Studio.  Sie können das UWP-Projekt in Unity erstellen, mit folgenden Schritten:

1. Öffnen der **Buildeinstellungen** Fenster durch Auswahl **Datei** > **Buildeinstellungen**.
2. Hinzufügen aller die Szenen, die in Ihrem Build unter enthalten sein sollen die **Szenen In Build** Abschnitt.
3. Wechseln Sie zu der **universelle Windows-Plattform** dazu **universelle Windows-Plattform** unter **Plattform** und auf **Plattform wechseln**.
4. Legen Sie **SDK** zu **10.0.15063.0** oder höher.
5. So aktivieren Sie Skripts, die debugüberprüfung **Unity C# Projekte**.
6. Klicken Sie auf **erstellen** und geben Sie den Speicherort des Projekts.

Nachdem der Build abgeschlossen wurde, hat Unity wird eine neue UWP-Projektmappendatei generiert das müssen Sie in Visual Studio ausführen:

1. Öffnen Sie im Ordner, den Sie angegeben haben,  **&lt;ProjectName&gt;sln** in Visual Studio.
2. Wählen Sie in der Symbolleiste am oberen **X64** und zum Bereitstellen der **lokalen Computer**.

Wenn Sie aktiviert **Skriptdebugging** beim Erstellen der Lösung für die UWP aus Unity erstellt haben, klicken Sie dann Ihre Skripts befinden sich unter der **(Universal Windows) Assembly-CSharp** Projekt.

> [!NOTE]
> Führen Sie vor dem verwenden Ihre Visual Studio-Builds, um Ihr Spiel mit echten Daten getestet, [dieser Prüfliste](test-visual-studio-build.md) , um sicherzustellen, Ihre Titel werden auf den Xbox Live-Dienst zugreifen.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme haben, versuchen bei der Anmeldung bei Xbox Live lesen [Problembehandlung bei Xbox Live-Anmeldung](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md).
