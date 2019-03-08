---
title: Melden Sie sich mit den SignInManager in Unity
description: Übersicht über die Unity-Plug-in-Sign-In-Manager
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641725"
---
# <a name="scripting-sign-in"></a>Scripting-Anmeldung

Um die Anmeldung Ihre eigenen benutzerdefinierten Spielobjekte hinzufügen müssen Sie Skripts in einem "gameobject". Angenommen Sie, dass das Prefab PlayerAuthentication passt nicht Ihr Spiel und Ihren eigenen Bereich Anmeldung haben möchten, in diesem Artikel gelangen Sie durch die grundlegenden Schritte der Titel des Anmeldung Logik hinzugefügt.

## <a name="sign-in-with-the-signinmanager"></a>Melden Sie sich mit den SignInManager

Xbox Live Unity-Plug-in enthält ein Skript für die `SignInManager` unter dem Dateipfad **Assets >> XboxLive >> Skripts >> SignInManager.cs**. Der Manager ist eine Singletonklasse, die Formular an eine beliebige Stelle im Titel aufgerufen werden kann durch einen Verweis auf des Titels des *Instanz* von der `SignInManager`. Dies *Instanz* ist nicht initialisiert werden müssen und Sie können die It, sobald das Spiel startet. Sie können Zugriff auf alle die dessen öffentliche Eigenschaften und Funktionen, die durch einen Verweis auf die *Instanz* als `SignInManager.Instance`.

Die `SignInManager` enthält alle erforderlichen Code für die Verwaltung der Authentifizierung für den Titel, dazu gehören, Anmeldung, Abmeldung, und Abrufen von Informationen über die Benutzer angemeldet sind, als die Player.

### <a name="calls-and-results"></a>Aufrufe und Ergebnisse

Die `SignInManager` hat drei Funktionen der Async-Co-Routine `SignInPlayer(int playerNumber)`, `SignOutPlayer(int playerNumber)`, und `SwitchUser(int playerNumber)`, diese Triggerereignis Funktionen zum Erfassen der Ergebnisse des Aufrufs und entsprechend agiert. Sie können die entsprechende Funktionen hinzufügen, um Ihr Skript und weisen sie Sie der `SignInManager.Instance`die Rückrufliste. Die Ereignisfunktionen sind `OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, und `OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`. Jeweils in der die Ereignisfunktionen hört das Ereignis, das in ihrem Namen beschrieben. Sie können Ihre eigenen Funktionen Rückrufliste des Managers in der Titelleiste Ihres hinzufügen `Start()` -Funktion mit den folgenden Code.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

Dieser Codeausschnitt fügt an- und Abmeldung Listener für den Player, die diese "gameobject" PlayerNumber zugeordnet. Diese "gameobject" `OnPlayerSignIn` Funktion wird aufgerufen, wenn die `SignInManager` erkennt ein Anmeldeversuch wurde abgeschlossen und die zugehörige `OnPlayerSignOut` Funktion wird aufgerufen, wenn die SignInManager eine Abmeldung erkennt. Die Ereignisfunktionen in Ihrer "gameobject" müssen einen Rückgabetyp und Parameter entsprechend den Typ der Funktion wird von der SignInManager aufgerufen haben. Sowohl die `OnPlayerSignIn` und `OnPlayerSignOut` gibt "void" Funktionen müssen eine `XboxLiveUser`, `XboxLiveAuthStatus`, und eine Zeichenfolge als ihre Parameter. Die Shell Ihre Funktionen kann wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

Überprüfen Sie bei beiden Funktionen der `XboxLiveAuthStatus` sicherstellen, dass den Aufruf der `SignInManager.Instance` war erfolgreich. Bei einem erfolgreichen Aufruf der `XboxLiveUser` werden die `XboxLiveUser`, die in unserer Out von signiert wurde `SignInManager`. Wenn der Aufruf nicht erfolgreich ist die `errorMessage` Zeichenfolge enthält Details zum Grund für Fehler.

Fügen Sie einige Zeilen Code für einen erfolgreichen Aufruf überprüft würde Code wie dem folgenden führen:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

Anmeldung aufrufen und das resultierende Ereignis für das Ergebnis zu erfassen, können Sie an- und Abmelden für Ihre Titel behandeln.

## <a name="get-signed-in-player-information"></a>Player-Informationen angemeldet

Zusätzlich zur Anmeldung beim Dienst zu Spieler verfolgt des der SignInManager alle angemeldeten Benutzer. Auf PC wird dies auf eine einzelne Anmeldung Player beschränkt werden, und auf der Xbox auf 16 beschränkt ist. Sie können überprüfen, wie diesen Grenzwert geht durch Vergleich der Ergebnisse der `SignInManager.Instance.GetCurrentNumberOfPlayers()` auf das Ergebnis des `SignInManager.Instance.GetMaximumNumberOfPlayers()`. Die SignInManager verfügt über ein Wörterbuch von angemeldeten Spieler nach des Spielers indiziert *PlayerNumber*. Hiermit können Sie einige grundlegende Informationen zu den Player zugegriffen werden kann über zugehörige abrufen `XboxLiveUser`.

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

Diese wenig Code überprüft, um festzustellen, ob ein Spieler angemeldet, um den Player-Number-Slot für diese "gameobject" und speichert dann diese Benutzer Gamertag, angezeigt werden, wenn sie angemeldet sind. Während der `XboxLiveUser` enthält den angemeldeten Benutzer Gamertag und Xbox-Benutzer-ID (Xuid), die Sie benötigen zum Aufrufen von anderen Diensten wie der `SocialManager` den Zugriff auf Informationen wie Gamerpic und Gamerscore.

## <a name="destroying-your-sign-in-gameobject"></a>Zerstören von Ihrer Anmeldung "gameobject"

Wie bei einem spielobjekt zu zerstören, die eines der Xbox Live-Plug-In-Manager verwendet die `SignInManager` oder `SocialManager`, in der Regel eine neue Szene laden, es ist wichtig, alle Funktionen hinzugefügt, um die Liste der Ereignislistener für den Manager zu entfernen. Im Beispielcode für diesen Artikel müssen wir die Funktionen zu entfernen, die wir, um die Ereignislistener für an- und Abmelden hinzugefügt. Entfernen wir diese Funktionen aus der `SignInManager` in die `OnDestroy()` Funktion unserer "gameobject".

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

Dieser Code entfernt die an- und Abmeldung Rückruffunktionen für den Player, die diese "gameobject" zugeordnet.

## <a name="testing-you-code-in-visual-studio"></a>Testen Sie den Code in Visual Studio

Zusätzlich zu den [Schritte erforderlich, um Ihr Spiel in Visual Studio erstellen](configure-xbox-live-in-unity.md#build-and-test-the-project), aufgelistet in der [konfigurieren Ihre Xbox Live-Titel für Unity](configure-xbox-live-in-unity.md) Artikel, es ist ein zusätzlicher Schritt erforderlich, um Ihr Spiel ordnungsgemäß in testen Visual Studio. Sie müssen eine Eigenschaft der Datei "Package.appxmanifest.xml" zu aktualisieren. Gehen Sie dazu wie folgt vor:

1. Suchen Sie im Projektmappen-Explorer für die Datei "Package.appxmanifest.xml"
2. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie Code anzeigen
3. Unter den `<Properties><\/Properties>` Abschnitt, fügen Sie die folgende Zeile hinzu: "< Uap:SupportedUsers > mehrere <\/Uap:SupportedUsers >.
4. Stellen Sie das Spiel auf der Xbox durch Starten eines remote Debuggen-Builds in Visual Studio bereit. Finden Sie Anweisungen zum Einrichten Ihrer Titels auf eine Xbox in die [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](../../xbox-apps/development-environment-setup.md) Artikel.

> [!NOTE]
> Der Teil der Konfiguration wurde geändert sieht möglicherweise wie es mehrere Spieler ermöglicht, aber es weiterhin erforderlich ist ist, Ihr Spiel einzelner Player für Szenarien mit.

## <a name="policies-and-limitations"></a>Richtlinien und Einschränkungen

Es gibt einige Anmelderichtlinien und Einschränkungen des Titels der, die Sie berücksichtigen sollten ggf., wenn Sie Ihre Anmeldung zu entwickeln.

- Nach dem Titel des ersten Anmeldung müssen Sie mindestens einen Player angemeldeten beibehalten. Die `SignInManager` ein Fehler ausgelöst wird, und den Aufruf fehl, wenn Sie versuchen, den letzten angemeldete Benutzer abmelden. Es ist auch wichtig zu beachten, dass Sie nicht aufrufen können `SignInManager.Instance.SwitchUser(int playerNumber)`auf der letzten angemeldet Player aus, wenn versucht wird, melden Sie sich der Spieler vor der Anmeldung eines neuen Players.

- PC können nur Anmeldung einen Benutzer zu einem Zeitpunkt Konsole kann bis zu 16 Spieler gleichzeitig Anmeldung verwendet wird.

- Der Titel tatsächlich keine Berechtigung zum Abmelden von eines Players aus dem Betriebssystem, aufgrund dieser SignOut funktioniert möglicherweise nicht wie erwartet. Die SignInManager kann einen Benutzer anmelden, ein- und ausgehende betrifft, wo der Titel aber nicht abmelden jeder auf dem Computer, die, dem der Titel in bereitgestellt wird.

- Mehrere Benutzeranmeldung ist nur auf eine Xbox-Konsole verfügbar.

- Gastkonten sind nicht für den Titel des Xbox Live Creators-Programm verfügbar.