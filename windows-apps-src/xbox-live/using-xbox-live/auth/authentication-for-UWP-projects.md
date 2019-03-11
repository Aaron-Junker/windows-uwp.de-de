---
title: Authentifizierung für UWP-Projekte
author: aablackm
description: Erfahren Sie, wie Xbox Live-Benutzer in einen Titel für die universelle Windows-Plattform (UWP) anmelden.
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Authentifizierung,-Anmeldung
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628385"
---
# <a name="authentication-for-uwp-projects"></a>Authentifizierung für UWP-Projekte

Um Xbox Live-Funktionen in Spielen nutzen zu können, muss ein Benutzer ein Xbox Live-Profil selbst identifizieren, in der Xbox Live-Community erstellen.  Xbox Live Services mitverfolgen Spiel bezogene Aktivitäten, die mithilfe dieses Profils Xbox Live, z. B. Gamertag des Benutzers und der Spieler Bild, die die Freunde des Benutzers Spiele sind, was den Benutzer Spiele gespielt hat, welche Erfolge, die der Benutzer entsperrt hat, in denen der Benutzer auf steht die die Bestenliste für ein bestimmtes Spiels, usw.

Wenn ein Benutzer den Zugriff auf Xbox Live-Dienste in einem bestimmten Spiel auf einem bestimmten Gerät möchte, muss der Benutzer zunächst authentifiziert.  Das Spiel kann Xbox Live-APIs zum Initiieren des Authentifizierungsprozesses aufrufen.  In einigen Fällen erhält der Benutzer mit einer Schnittstelle, geben Sie zusätzliche Informationen wie die Eingabe von Benutzername und Kennwort für die Microsoft Account zu verwenden, werden Berechtigungen die Zustimmung zum Spiel, Konto Problembehebung, neue Nutzungsbedingungen akzeptiert, usw.

Nach der Authentifizierung ist der Benutzer das Gerät zugeordnet, bis sie explizit bei Xbox Live über die Xbox App anmelden.  Nur ein einziger Player auf einem Gerät nicht über die Konsole zu einem Zeitpunkt (für alle Xbox Live Spiele); authentifiziert werden können  für einen neuen Player auf einem Gerät nicht über die Konsole authentifiziert werden muss die vorhandene authentifizierte Player zuerst anmelden.

## <a name="steps-to-sign-in"></a>Schritte zur Anmeldung

Im Allgemeinen verwenden Sie die Xbox Live-APIs mit folgenden Schritten:

1. Erstellen Sie ein XboxLiveUser-Objekt, um den Benutzer darzustellen.
2. Melden Sie sich im Hintergrund beim Xbox Live beim Start
3. Versucht, sich mit UX Anmelden bei Bedarf
4. Erstellen Sie einen Xbox Live-Kontext basierend auf dem interagierenden Benutzer
5. Verwenden Sie den Xbox Live-Kontext, den Zugriff auf Xbox Live-Dienste
6. Wenn das Spiel beendet wird oder der Benutzer meldet-Ausgang, lassen Sie die XboxLiveUser-Objekt und die XboxLiveContext-Objekt auf null festlegen

### <a name="creating-an-xboxliveuser-object"></a>Erstellen eines Objekts XboxLiveUser

Die meisten Aktivitäten Xbox Live beziehen sich auf der Xbox Live-Benutzer.  Als Entwickler von Spielen müssen Sie zuerst ein XboxLiveUser-Objekt zur Darstellung des lokalen Benutzers zu erstellen.

C++:

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C++/CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C# (WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **WindowsSystemUser** das Benutzerobjekt des Windows-System verwendet werden, Xbox live-Benutzer zugeordnet werden soll. "Nullptr" kann sein, wenn die app auf einem einzelnen Benutzer application(SUA) ist.
  * Überprüfen Sie weitere Informationen zu einzelnen Benutzer Application(SUA) und mehrere Benutzer Application(MUA) [Einführung in Anwendungen für mehrere Benutzer](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * Weitere Informationen zum Abrufen von Windows::System::User ^ von Windows, prüfen Sie [Abrufen von Windows-System-Benutzer auf UWP](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>Melden Sie sich im Hintergrund beim Xbox Live beim Start ###

Zum Authentifizieren des Benutzers zu Xbox Live so früh wie möglich nach dem Starten, noch bevor Sie die Benutzeroberfläche zum Abrufen von Daten von Xbox Live-Dienste bereits vorhanden, sollten Sie ins Spiel kommen.

Rufen Sie zur Authentifizierung des lokalen Benutzers im Hintergrund

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C++/CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  -Threadverteiler wird für die Kommunikation zwischen Threads verwendet. Obwohl die API für die automatische Anmeldung nicht mehr wird keine Benutzeroberfläche angezeigt, benötigt XSAPI immer noch den Verteiler der UI-Thread zum Abrufen von Informationen zu Ihrer Appx Gebietsschema. Erhalten Sie die statische Dispatcher-UI-Thread-Verteiler durch Aufrufen von Windows::UI::Core::CoreWindow::GetForCurrentThread() im UI-Thread >. Oder wenn Sie sicher sind, dass diese API im UI-Thread aufgerufen wird, können Sie übergeben "nullptr" (z. B. in JS UWA).


Es gibt 3 mögliche Ergebnisse aus der automatischen Anmeldung Versuch

* **Erfolg**

  Wenn das Gerät online ist, dies bedeutet, dass des Benutzers erfolgreich authentifiziert wurde, Xbox Live, und wir konnten ein gültiges Token zu erhalten.

  Wenn das Gerät offline ist, bedeutet dies, der Benutzer wurde zuvor für Xbox Live erfolgreich authentifiziert und ist nicht explizit signiert ausgehend von diesen Titel.  Beachten Sie in diesem Fall, dass keine Garantie dafür, die Titel auf ein gültiges Token zugreifen, es ist nur garantiert, dass die Identität des Benutzers bekannt ist, und überprüft wurde.    Die Identität des Benutzers wird auf den Titel über ihre Xbox-Benutzer-Id (Xuid) und das Gamertag bezeichnet.

* **UserInteractionRequired**

  Dies bedeutet, dass die Laufzeit des Benutzers im Hintergrund anmelden konnte.  Das Spiel sollten Aufrufen `xbox_live_user::sign_in` die Ruft die Xbox-Identitätsanbieter, um den erforderlichen UX-Datenfluss für den Benutzer zur Anmeldung-Registrierung/Anmeldung veranschaulichen.  Häufige Probleme sind:

  * Benutzer hat keine Microsoft-Account
  * Der Benutzer wurde eine bevorzugte Microsoft-Account-Spiele nicht festgelegt werden.
  * Die ausgewählte Microsoft Account kein Xbox Live-Profils
  * Der Benutzer muss Microsoft Account Zustimmung akzeptieren

* **Andere Fehler**

  Die Laufzeit konnte keine anderen Gründen Anmeldung.  Diese Probleme sind in der Regel nicht von das Spiel oder der Benutzer. Wenn Sie c++-API verwenden, müssen Sie Fehler überprüfen Sie anhand von Xbox_live_result <> .err(); für WinRT, müssen Sie zum Abfangen von Platform:: Exception ^.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>Versucht, sich mit UX Anmelden bei Bedarf ###

Ihr Spiel sollte den Benutzer, Xbox Live mit UX aktiviert, wenn die automatische Anmeldung nicht erfolgreich war, und Sie bereit sind, die Benutzeroberfläche authentifizieren.

Rufen Sie zur Authentifizierung des lokalen Benutzers mit UX

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C++/CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  -Threadverteiler wird für die Kommunikation zwischen Threads verwendet. Melden Sie sich die API erfordert den UI-Verteiler, sodass die Anmeldung auf Benutzeroberfläche anzeigen kann und der Informationen zu Ihrer Appx Gebietsschema abrufen. Erhalten Sie die statische Dispatcher-UI-Thread-Verteiler durch Aufrufen von Windows::UI::Core::CoreWindow::GetForCurrentThread() im UI-Thread >. Oder wenn Sie sicher sind, dass diese API im UI-Thread aufgerufen wird, können Sie übergeben "nullptr" (z. B. in JS UWA).

Es gibt 3 mögliche Ergebnisse aus der Anmeldeversuch mit UX ein:

* **Erfolg**

  Wenn das Gerät online ist, dies bedeutet, dass des Benutzers erfolgreich authentifiziert wurde, Xbox Live, und wir konnten ein gültiges Token zu erhalten.

  Wenn das Gerät offline ist, bedeutet dies, der Benutzer wurde zuvor für Xbox Live erfolgreich authentifiziert und ist nicht explizit signiert ausgehend von diesen Titel.  Beachten Sie in diesem Fall, dass keine Garantie dafür, die Titel auf ein gültiges Token zugreifen, es ist nur garantiert, dass die Identität des Benutzers bekannt ist, und überprüft wurde.    Die Identität des Benutzers ist der Titel Xbox-Benutzer-Id (Xuid) und das Gamertag bekannt.

* **UserCancel**

  Dies bedeutet, dass der Benutzer den Anmeldevorgang vor Abschluss abgebrochen.  In diesem Fall sollte das Spiel nicht automatisch melden Sie sich mit UX wiederholen  Stattdessen sollten sie in-Game-UX darstellen, die dem Benutzer ermöglicht, die Wiederholung des Vorgangs-Anmeldung.  (Z. B. eine Schaltfläche "Anmelden")

* **Andere Fehler**

  Die Laufzeit konnte keine anderen Gründen Anmeldung.  Diese Probleme sind in der Regel nicht von das Spiel oder der Benutzer. Wenn Sie c++-API verwenden, müssen Sie Fehler überprüfen Sie anhand von Xbox_live_result <> .err(); für WinRT, müssen Sie zum Abfangen von Platform:: Exception ^.

## <a name="sign-in-code-examples"></a>Codebeispiele für Anmeldung

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>Melden Sie sich ab

### <a name="handling-user-sign-out-completed-event"></a>Behandlung von Benutzer abmelden abgeschlossene Ereignis

Der Benutzer wird von einem Titel abmelden, eine der folgenden Fall:

1. Der Benutzer registriert das horizontale hochskalieren von der Xbox-App (Windows 10) oder die Konsole Shell (Xbox One). Beim Abmelden wirkt sich auf alle apps im Xbox Live-aktiviert für diesen Benutzer installiert aus.
2. Der Benutzer zu einem anderen Microsoft-Account gewechselt
3. In diesem Titel von einem anderen Gerät angemeldeten Benutzers

In allen diesen Fällen erhält der Titel ein Ereignisses aus der `xbox_live_user::add_sign_out_completed_handler` oder `XboxLiveUser::SignOutCompleted` Handler.  Das Spiel muss die Abmeldung abgeschlossene Ereignis entsprechend verarbeiten:

1. Das Spiel sollte deutlichen visuellen Hinweis darauf, die dem Benutzer angezeigt werden, die erkennen signiert von Xbox Live Skalierung hat.
2. Das Spiel kann keine Xbox Live-Dienst-APIs im Ereignishandler, aufrufen, da der Benutzer hat bereits angemeldet ist und keine Autorisierungstoken, das verfügbar ist.

## <a name="sign-out-handler-code-samples"></a>Melden Sie sich die Ereignishandler-Codebeispiele

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>Bestimmen, ob das Gerät offline ist

Melden Sie sich die APIs ist immer noch erfolgreich bei offline, wenn der Benutzer einmal angemeldet hat, und das letzte Konto angemeldet wird zurückgegeben.  

Wenn kein Benutzer angemeldet ist, bevor offline-Anmeldung nicht erreicht werden kann.

Wenn der Titel offline wiedergegeben werden kann (Kampagne Modus usw.), der Titel kann dem Benutzer ermöglichen, wiedergeben und aufzeichnen Spiel läuft über WriteInGameEvent-API und Speicherverwaltungs-API verbunden, beide ordnungsgemäß ausgeführt, während das Gerät offline ist.

Wenn der Titel nicht offline wiedergegeben werden kann (Multiplayer-Spiels oder Server-basierten spielen usw.) der Titel sollte die GetNetworkConnectivityLevel-API aufrufen, um herauszufinden, ob das Gerät offline ist, und informieren Sie Benutzer über den Status und mögliche Lösungen (z. B. "müssen Sie Verbinden mit dem Internet, um fortzufahren... ").

## <a name="online-status-code-samples"></a>Online-Status-Codebeispiele

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```