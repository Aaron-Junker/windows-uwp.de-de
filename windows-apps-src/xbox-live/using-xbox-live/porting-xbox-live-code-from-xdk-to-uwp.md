---
title: Portieren von Xbox Live-Code aus xdk-Version auf UWP
description: Erfahren Sie, wie Xbox Live-Code von der Plattform Xbox Development Kit (xdk-Version) auf die universelle Windows-Plattform (UWP) portieren.
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, xdk-Version, Portieren
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590815"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Portieren von Xbox Live-Code aus der xdk (Xbox Developer Kit-Version), universelle Windows-Plattform (UWP)

## <a name="introduction"></a>Einführung

Dieser Artikel richtet sich an, die Entwicklern helfen, die die Xbox One XDK-verwendet haben, um zu beginnen, migrieren ihre Xbox Live-Code auf dem Windows 10 Universal Windows Platform (UWP).

Rahmen der Migration umfasst der Wechsel von XSAPI 1.0 (Xbox Live Services-API, in der Xbox One XDK-bis August 2015 enthalten), zu XSAPI 2.0 (enthalten in der Xbox One XDK ab November 2015, und auch in das Xbox Live SDK verfügbar. Die Funktionalität dieser APIs sind nahezu identisch, aber es gibt einige Implementierungsunterschiede wichtig.

Weitere Themen in diesem Artikel abgedeckt werden umfassen Vorbereiten Ihrer Windows-Entwicklungscomputer aus, und andere APIs in der Regel installieren, werden bei der Verwendung von Xbox Live-Dienste, z. B. die Secure Sockets-API als auch für die verbundene-Speicher-API für die Verwaltung von Cloud-Sicherung erforderlich Spiel speichert.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>Das Einrichten und konfigurieren das Projekt im Partner Center und XDP

Ein UWP-Titel, der Xbox Live verwendet services Anforderungen, die zu konfigurierenden [Partner Center](https://partner.microsoft.com/dashboard). Die neuesten Informationen finden Sie unter [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) im Xbox Live-Programmierhandbuch enthalten, mit der [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Themen auf dieser Seite sind diese Schritte für die Verwendung von Xbox Live-Dienste in Ihrem Titel:

-   Erstellen Sie das UWP-app-Projekt im Partner Center an.

-   Verwenden Sie zum Einrichten des Projekts für die Verwendung von Xbox Live XDP.

-   Verknüpfen Sie Ihr Partner Center-Produkt zu Ihrem Produkt XDP.

-   Erstellen Sie entwicklerkonten in XDP (erforderlich, wenn es sich bei Ihrer Xbox Live-Titel in Ihrer Sandbox ausgeführt wird).

Wenn Ihre Titel Multiplayer-Funktion unterstützen, müssen möglicherweise einige zusätzlichen Einstellungen in Ihren Vorlagen Multiplayer-Sitzung. Alle Windows 10-Titel, die Xbox Live Multiplayer- und Schreiben in eine MPSD (Dokument Multiplayer-Sitzung) erforderlich, dieses neue Feld in der Liste "Funktionen" in Ihre Sitzung Vorlagen: ```userAuthorizationStyle: true```.

### <a name="enabling-cross-play"></a>Aktivieren von Cross-play

Wenn Sie "Cross-Wiedergabe" (freigegebene Xbox Live-Konfiguration zwischen Xbox One und PC-Spiele, Geräteübergreifende multiplayer-Spiele zulassen) unterstützen, müssen Sie auch diese Funktion auf Ihre Sitzung Vorlagen hinzuzufügen: **CrossPlay: "true"**.

Weitere Informationen zur Unterstützung von Cross-Wiedergabe und Anforderungen für seine Konfiguration in XDP finden Sie unter "Erfassung xdk-Version und UWP Cross-Play-Titel in XDP" im Xbox Live-Programmierhandbuch.

Darüber hinaus einige programmgesteuerte Überlegungen finden Sie weiter unten [unterstützen Multiplayer-Cross-Funktion zwischen Xbox One und PC](#_Supporting_multiplayer_cross-play).

## <a name="setting-up-your-windows-development-environment"></a>Einrichten der Windows-Entwicklungsumgebung

1.  [Herunterladen der neuesten **Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) und extrahieren Sie lokal.

2.  [Installieren Sie die **Xbox Live-Plattform Erweiterungen SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) bei Bedarf die Secure Sockets-API und/oder das Spiel speichern-API (auch bekannt als verbundene Speicher) für UWP.

3.  Fügen Sie Xbox Live-Unterstützung hinzu, um Ihre universelle Windows-app-Projekt in Visual Studio. Sie können entweder den vollständigen Quellcode hinzufügen oder auf die Binärdateien durch Installieren des NuGet-Pakets in Visual Studio-Projekt verweisen. Pakete sind für C++ und WinRT verfügbar. Weitere Informationen finden Sie unter [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  Konfigurieren von Ihrem Entwicklungscomputer, um Ihre Sandbox verwenden. Ist es ein Befehlszeilenskript in das Toolverzeichnis des Xbox Live SDK, die Sie über eine Eingabeaufforderung als Administrator verwenden können (z. B.: SwitchSandbox.cmd XDKS.1).

  **Beachten Sie** um zurück an den Retail-Sandkasten zu wechseln, löschen Sie entweder den Registrierungsschlüssel, der das Skript ändert, oder Sie mit der Sandbox wird aufgerufen, Einzelhandel wechseln können.

1.  Fügen Sie ein Developer-Konto auf Ihrem Entwicklungscomputer. Ein Entwicklerkonto erstellt haben, im XDP ist erforderlich, für die Interaktion mit Xbox Live-Dienste zur Laufzeit, wenn Sie Ihre zugewiesenen Sandbox entwickeln oder Ausführen der Beispiele. So fügen Sie ein oder mehrere Konten zu Windows hinzu:

    1.  Open **Einstellungen** (Tastenkombination: Windows-Taste + I).

    2.  Open **Konten**.

    3.  Auf der **Ihres Kontos** auf **Microsoft-Konto hinzufügen**.

    4.  Geben Sie den e-Mail-Developer-Konto und das Kennwort ein.

### <a name="appxmanifest-changes"></a>AppxManifest-Änderungen

Die häufigsten Änderungen zwischen der Xbox und UWP-Version der Datei "appxmanifest.xml" sind:

1. Paketidentität spielt eine Rolle auf UWP, sogar während der Entwicklung aus. Dem Identitätsnamen und dem Verleger *übereinstimmen* wie im Partner Center für die UWP-app definiert wurde.

1. Ein Abhängigkeit von Paket-Abschnitt ist erforderlich. Zum Beispiel:

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  Ein Beispiel für UWP-Anwendungsmanifest (z. B. eine UWP-Beispielen, die mit dem Xbox Live-SDK enthalten oder ein Standard universelle Windows-app-Projekt in Visual Studio erstellte) für andere Abschnitte des Anwendungsmanifests, die speziellen Anforderungen für finden Sie unter UWP, z. B. ```<VisualElements>```.

1.  Titel und SCID werden in der Datei xboxservices.config definiert (finden Sie unter der [nächsten Abschnitt](#_Define_your_title)) statt in der Kategorie "xbox.live"-Erweiterung.

1.  Die Kategorie "xbox.system.resources"-Erweiterung ist nicht erforderlich.

1.  Secure Sockets sind in der Datei networkmanifest.xml definiert (finden Sie unter [Secure Sockets](#_Secure_sockets)) statt in der Kategorie "windows.xbox.networking".

1.  Kategorie-Erweiterung "windows.protocol" muss definiert werden, um Einladungen Xbox Live in Ihre UWP-Titel zu empfangen (finden Sie unter [senden und Empfangen von Einladungen](#_Sending_and_receiving)).

1.  Wenn Sie die GameChat-API verwenden, sollten Sie die Microphone-Gerätefunktion innerhalb Hinzufügen der ```<Capabilities>``` Element. Zum Beispiel:

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>Definieren Sie Titel und eine SCID für das Xbox Live-SDK in einer Konfigurationsdatei.

Das Xbox Live-SDK muss wissen, Ihre Titel-ID und SCID, die nicht mehr in der "appxmanifest.xml" für UWP-Titel enthalten sind. Stattdessen erstellen Sie eine Textdatei namens **xboxservices.config** in Ihrem Projekt Stammverzeichnis, und fügen Sie die folgenden Felder ein, und Ersetzen Sie dabei die Werte mit den Informationen für den Titel hinzu:

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Alle Werte in xboxservices.config sind Groß-/Kleinschreibung beachtet.

Schließen Sie diese Konfigurationsdatei als Inhalt in Ihrem Projekt, damit sie in der Buildausgabe verfügbar ist.

**Beachten Sie** diese Werte werden mithilfe der folgenden-API programmgesteuert in der Titelleiste Ihres verfügbar:

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>API-Namespace-Zuordnung

Tabelle 1: Namespace-Zuordnung von xdk-Version für die UWP.

<table>
  <tr>
    <td></td>
    <td><b>Xbox One XDK</b></td><td><b>UWP</b></td>
    <td><b>-API ist verfügbar, mit...</b></td>
  </tr>
  <tr>
    <td>Xbox-Services-API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>keine Änderung</i>)</td>
    <td>Xbox Live-SDK (verwenden Sie NuGet binäre oder Quelle)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*keine Änderung*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK (verwenden Sie NuGet-binary) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Erweiterungen Xbox Live SDK</td>
  </tr>
  <tr>
    <td>Onlinespeicher</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Erweiterungen Xbox Live SDK</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>Multiplayer-Abonnements und Behandlung von Ereignissen

Eine der die Änderungen von XSAPI 1.0 auf 2.0 XSAPI, die am häufigsten Multiplayer-Titeln begegnen werden wird das Verschieben der mehrere Methoden und Ereignisse aus der **RealTimeActivityService** auf die **MultiplayerService**.

Zum Beispiel:

-   **EnableMultiplayerSubscriptions()\***  Methode

-   **DisableMultiplayerSubscriptions()** Methode

-   **MultiplayerSessionChanged** Ereignis

-   **MultiplayerSubscriptionLost** event

-   **MultiplayerSubscriptionsEnabled** Eigenschaft

**Beachten Sie wichtige Implementierung** , obwohl Sie nicht explizit etwas anderes in verwenden können die **RealTimeActivityService** nach der Umstellung dieser Ereignisse und Methoden auf die **MultiplayerService**, Sie müssen trotzdem aufrufen **XblContext -&gt;RealTimeActivityService -&gt;Activate()** vor dem Aufruf **EnableMultiplayerSubscriptions()** Da die Multiplayer-Abonnements den RTA-Dienst erforderlich.

## <a name="whats-handled-differently-in-uwp"></a>Was ist anders in UWP behandelt.

Es folgt eine allgemeine Architektur Liste der Abschnitte des Codes, die Unterschiede zwischen dem xdk-Version und UWP, wahrscheinlich hat wie in der neuen gefunden [NetRumble Beispiel](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (einschließlich sowohl xdk-Version und UWP-Versionen):

-   Zugriff auf Informationen für Titel-ID und SCID

-   Der Vorabstart Aktivierung (neu in UWP)

-   Behandlung von Suspend/Resume-PLM

-   Erweiterte Ausführung (neu in UWP)

-   Xbox **Benutzer** -Objekt und die Unterschiede bei der Benutzer-Ausnahmebehandlung

    -   Behandlung von an- und Abmelden

    -   Controller-Kopplung (nur auf der Xbox behandelt)

    -   Behandlung von Gamepad

-   Multiplayer-Berechtigungen werden überprüft

-   Unterstützung von Multiplayer-Cross-Funktion zwischen Xbox One und PC

-   Einladungen senden Spiel

    -   Möglichkeit zum Öffnen von Partei-app aus in-Game - n/v auf UWP

    -   Auflisten von Partei Mitglieder von in-Game - n/v auf UWP

-   Zeigt das Spiele-Profil

-   Secure Socket-API-Oberfläche-Änderungen

-   QoS-Messung Initiierung und den Ergebnis-Verarbeitung

-   Schreiben von Spiele-events

-   GameChat: Ereignisse, Einstellungen und ChatUser-Objekt

-   Storage-API-Oberfläche Änderungen verbunden

-   PIX-Ereignisse (nur auf der Xbox; nicht behandelt in diesem Whitepaper)

-   Einige renderingunterschiede

In den folgenden Abschnitten besprochen ausführlich weiter viele dieser Unterschiede.

### <a name="accessing-title-id-and-scid-info"></a>Zugriff auf Informationen für Titel-ID und SCID

Auf der UWP, Ihre Titel-ID und den Konfigurations-ID des Diensts erfolgt über die AppConfig-Eigenschaft in einer Instanz von einem **XboxLiveContext**.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**Beachten Sie** In der xdk-Version, Sie erhalten diese IDs mit diese neuen Eigenschaften oder die alte statische Eigenschaften in **Windows::Xbox::Services::XboxLiveConfiguration**.

### <a name="prelaunch-activation"></a>Der Vorabstart Aktivierung

Häufig verwendete Titel in Windows 10 möglicherweise vorab gestartet werden, wenn der Benutzer anmeldet. Für diesen Fall müsste der Titel Code, der die Startargumente für überprüft **PreLaunchActivated**. Z. B. empfiehlt nicht, alle Ihre Ressourcen während dieser Art der Aktivierung zu laden. Weitere Informationen finden Sie im MSDN-Artikel [Handle app Vorabstart der](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx).

### <a name="suspendresume-plm-handling"></a>Behandlung von Suspend/Resume-PLM

Anhalten und fortsetzen und PLM im Allgemeinen funktionieren auf ähnliche Weise in einer universellen Windows-app mit der Art und Weise, die sie auf der Xbox One funktionieren; Es gibt jedoch einige wichtige Unterschiede zu beachten:

-   Es gibt keine **Constrained** Status auf dem PC – Dies ist ein Konzept für Xbox One exklusive.

-   Das Anhalten beginnt sofort, wenn der Titel minimiert wird; eine einfache Lösung dafür, finden Sie im Abschnitt [erweiterte Ausführung](#_Extended_execution).

-   Die zeitliche Steuerung unterscheidet: 5 Sekunden auf einem PC anstelle der 1 Sekunde auf der Konsole angehalten haben.

Ein weiterer wichtiger Aspekt bei der Verwendung verbundener Speicher ist der neue **ContainersChangedSinceLastSync** -Eigenschaft in die UWP-Version dieser API. Bei der Verarbeitung eines Ereignisses fortsetzen, können Sie überprüfen, diese Eigenschaft, um festzustellen, ob alle Container in der Cloud geändert, während es sich bei Ihrem Titel angehalten wurde. Dies kann vorkommen, wenn der Spieler das Spiel auf einem PC angehalten, an anderer Stelle wiedergegeben und dann an die erste PC zurückgegeben. Wenn Sie Daten aus dieser Container in den Arbeitsspeicher gelesen haben, bevor Sie unterbrochen hatte, möchten Sie wahrscheinlich auch lesen Sie sie erneut, um festzustellen, was geändert und die Änderungen entsprechend verarbeiten.

Weitere Informationen zur Behandlung von PLM in einer UWP-app unter Windows 10 finden Sie im MSDN-Artikel [Launching, resuming, sowie Hintergrundaufgaben der](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx).

Finden Sie auch die [PLM für Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) Whitepaper zur GDN nützlich, da er mit Spielen Bedenken geschrieben wurde, und die meisten Konzepte, die für die Verarbeitung des app-Lebenszyklus weiterhin auf einem PC gelten.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>Erweiterte Ausführung

Minimieren von einer UWP führt app auf einem Computer in der Regel darin die ab sofort angehalten. Mithilfe erweiterten Ausführung zu verwenden, müssen Sie die Möglichkeit, diesen Prozess zu verzögern. Beispiel für eine Implementierung:

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

Nach der **ExtensionRevokedHandler** war aufgerufen wird, eine neue Erweiterung für zukünftige potenzielle Unterbrechungen angefordert werden muss. Die **ExtensionRevokedHandler** wird aufgerufen, wenn genügend Arbeitsspeicher im System vorhanden ist, 10 Minuten vergangen oder der Benutzer wechselt zurück an das Spiel, während das Spiel minimiert wird. Also **RequestExtension()** wahrscheinlich zu diesen Zeitpunkten aufgerufen werden soll:

-   Während des Starts.

-   In der **ExtensionRevokedHandler** beim Args -&gt;Grund == fortgesetzt (der Benutzer im Registerkartenformat zurück, während das Spiel minimiert wurde, bevor der 10-Minuten-Timer abgelaufen ist).

-   In der **OnResuming** Handler (wenn der Titel aufgrund von ungenügendem Arbeitsspeicher oder den Zeitgeber 10 Minuten angehalten wurde).

### <a name="handling-users-and-controllers"></a>Behandeln von Benutzer und -Controller

Auf Windows arbeiten Sie mit einem angemeldeten Benutzer zu einem Zeitpunkt. In den Xbox Live SDK, erstellen Sie zuerst eine **XboxLiveUser** Objekt, für Xbox Live anmelden und erstellen Sie dann **XboxLiveContext** Objekte aus diesen Benutzer.

Zuvor auf der Xbox One XDK-:

1.  Abrufen eines Benutzers (von Gamepad-Aktivität, z. B.).
2.  Erstellen Sie eine **XboxLiveContext** Benutzer:
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  Verarbeiten einer **SignOut** Ereignis:
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  Behandeln Sie Gamepad/Controller Kopplung mit:
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

Jetzt Live für die UWP/Xbox SDK:

1.  Erstellen Sie eine **XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  Versuchen Sie, melden Sie sich diese mit dem letzten Microsoft-Konto, die, das Sie verwendet, ohne dass diese Benutzeroberfläche:

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  Wenn man **SignInResult::Success** erstellen Sie in das Ergebnis dieses asynchronen Vorgangs, der **XboxLiveContext**, und klicken Sie dann Sie fertig sind:

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  Wenn stattdessen Sie erhalten **SignInResult::UserInteractionRequired**, müssen Sie zum Aufrufen der Methode für die interaktive Anmeldung, die Benutzeroberfläche öffnet:

  ```
  xblUser->SignInAsync();
  ```

1.  Hier erhalten Sie möglicherweise **SignInResult::UserCancel**, in diesem Fall Sie haben keines angemeldeten Benutzers und Sie sollten Sie ggf. eine Menüoption, damit sie sich erneut anzumelden.

  **Beachten Sie** beim Bereitstellen von Menüoptionen, es ist eine gute Idee, die ihnen die Möglichkeit, wechseln Sie zu einem anderen Microsoft-Konto gewähren:

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  Nachdem Sie einen Benutzer angemeldet haben, sollten Sie zum Einbinden der **XboxLiveUser::SignOutCompleted** Ereignis, damit Sie für den Benutzer beim Abmelden reagieren können:

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  Es gibt kein Controller koppeln, um unter Windows 10 zu behandeln.

Dies ist ein vereinfachtes Beispiel für C++ / WinRT. Ein ausführlicheres Beispiel finden Sie unter "Xbox Live-Authentifizierung in Windows 10" im Xbox Live-Programmierhandbuch. Finden Sie auch das breitere Beispiel auf "Hinzufügen von Xbox Live zu einem neuen UWP-Projekt" hilfreich.

### <a name="checking-multiplayer-privileges"></a>Multiplayer-Berechtigungen werden überprüft

Entspricht der **CheckPrivilegeAsync()** ist noch nicht in das Xbox Live SDK verfügbar. Jetzt müssen Sie für das Privileg suchen, Sie in der Zeichenfolgenliste zurückgegebenes müssen, der **Berechtigungen** -Eigenschaft für eine **XboxLiveUser**. Beispielsweise um für mehrere Spieler Berechtigungen zu überprüfen, suchen Sie nach der Berechtigung "254." Verwenden der Dokumentation zum xdk-Version an, Sie finden eine Liste mit allen Xbox Live-Berechtigungen in der **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** Enumeration.

Eine Erläuterung zu diesem Thema, finden Sie im Forumsbeitrag [Xsapi Ben & utzer Berechtigungen](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html).

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Unterstützung von zwischen Xbox One und PC-UWP Multiplayer-Cross-Funktion

Zusätzlich zu der neuen Sitzung vorlagenanforderungen in XDP (finden Sie unter [einrichten und konfigurieren das Projekt im Partner Center und XDP](#_Setting_up_and)), Cross-Play im Lieferumfang von neuer Einschränkungen bei der Sitzung beitreten können. Sie können nicht mehr auf "None", als Einschränkung Join Sitzung verwenden. Sie müssen "Verfolgte" oder "Local" (die standardbeschränkung ist "Local") verwenden.

Darüber hinaus die Join- und read Einschränkungen standardmäßig auf "Local" aufgrund der erforderlichen **UserAuthorizationStyle** Funktion für Windows 10 Multiplayer-Spiele.

In diesem Artikel Forum [ist es möglich, eine öffentliche Multiplayer-Sitzung erstellen](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html), enthält zusätzliche Erkenntnisse zu gewinnen.

Weitere Informationen und Beispiele in der aktualisierten Multiplayer-Entwickler Flussdiagrammen, die Cross-Play-fähige Multiplayer-Beispiel NetRumble, oder aus Ihr Developer-Konto-Manager (DAM) finden.

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>Senden und Empfangen von Einladungen

Die API, um die Benutzeroberfläche für das Senden von Einladungen zu öffnen ist jetzt **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**. Übergeben Sie eine Sitzung -&gt; **SessionReference** Objekt aus der Sitzung Aktivität (in der Regel Ihre Lobby). Sie können optional in einem zweiten Parameter übergeben, dass Verweise ein benutzerdefinierten Zeichenfolgen-ID einladen, die in der Dienstkonfiguration in XDP definiert wurde. Die Zeichenfolge, die Sie, es definieren wird in der Toast-Benachrichtigung gesendet, um die eingeladenen Spieler angezeigt. Beachten Sie, was Sie übergeben ein Parameter an diese Methode ist die ID-Nummer, und es muss für den Dienst ordnungsgemäß formatiert sein. Z. B. ID-Zeichenfolge "1" muss als übergeben werden "/ / / 1".

Wenn Einladungen direkt unter Verwendung des Multiplayer-Diensts gesendet werden soll (d. h., ohne eine Benutzeroberfläche anzuzeigen), können Sie trotzdem verwenden, die andere Methode einladen, **Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** aus des Benutzers **XboxLiveContext**.

Um Einladungen, die in Windows ermöglichen, Ihre Titel-Protokoll aktivieren, müssen Sie diese Erweiterung zum Hinzufügen der **&lt;Anwendung&gt;** Element in die appxmanifest-Datei:

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Dann können Sie die INVITE-Nachricht behandeln, wie zuvor auf der Xbox One bei Ihrer **"coreapplication"** Ruft eine **Activated** Ereignis und die Aktivierung, die die Art ist ein **ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>Anzeigen der Spieler Profilkarte

Verwenden Sie zum Anzeigen der Spieler Profilkarte in UWP, **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**, und übergeben Sie die XUID für den Zielbenutzer.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>Secure sockets

Die Secure Socket-API befindet sich in der separaten [Xbox Live-Plattform Erweiterungen SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Finden Sie unter diesem Forumsbeitrag für API-Nutzung: [Einrichten von SecureDeviceAssociation für alle Plattformen](https://forums.xboxlive.com/answers/45722/view.html).

**Beachten Sie** für UWP, die **SocketDescriptions** Abschnitt wurde verschoben, aus die appxmanifest-Datei und in eine eigene [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt). Das Format in die &lt;SocketDescriptions&gt; Element ist nahezu identisch, allerdings ohne die **Mx:** Präfix.

Für Cross-Play zwischen Xbox und Windows 10 werden *Sie sicher, dass* , die alle Elemente wird definiert, *identisch* zwischen den beiden verschiedenen Arten von Manifesten ("Package.appxmanifest" für Xbox One und networkmanifest.xml für Windows 10). Socket-Name, Protokoll, usw. übereinstimmen *genau*.

Auch müssen Sie für Cross-Wiedergabe, definieren Sie die folgenden vier SDA Verwendung innerhalb der ```<AllowedUsages>``` Element im *sowohl* der Xbox eine Datei "Package.appxmanifest", und die Windows 10-networkmanifest.xml:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>Multiplayer-QoS-Messungen

Zusätzlich zu der Änderung des Namespace in der API für die Secure Sockets wurden einige der zu verwendenden Objektnamen und Werte, zu geändert. Die Zuordnung für den Status in der Regel verwendete Maßeinheit befindet sich in der folgenden Tabelle.

Tabelle 2: In der Regel verwendet Messung Status Zuordnung.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| Möglich                            | Erfolgreich                                  |

Die Schritte *messen* QoS (Quality des Diensts) und *Verarbeitung der Ergebnisse* sind im Prinzip identisch beim Vergleichen der xdk-Version und UWP-Versionen der API. Der resultierende Code sieht jedoch aufgrund der Namensänderungen und einige Änderungen des Entwurfs, an einigen Stellen anders.

Zum Messen der QoS für die **xdk-Version**, Sie erstellt haben, eine Auflistung von sicheren Geräteadressen und eine Sammlung von Metriken und übergeben Sie diese in die **MeasureQualityOfServiceAsync()** Methode.

Zum Messen der QoS für **UWP**, erstellen Sie ein neues **XboxLiveQualityOfServiceMeasurement()** Objekt, rufen Sie **append() ähnlich** auf seine **Metriken** und **DeviceAddresses** Eigenschaften, und klicken Sie dann das Objekt des Aufrufs **MeasureAsync()** Methode.

Zum Beispiel:

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

Weitere Beispiele finden Sie in der **MatchmakingSession::MeasureQualityOfService()** und **MatchmakingSession::ProcessQosMeasurements()** Funktionen im NetRumble-Beispiel.

### <a name="writing-game-events"></a>Schreiben von Spiele-events

Senden von spielereignisse, die in der Dienstkonfiguration des Titels konfiguriert sind, verwendet eine andere API auf UWP. Das Xbox Live SDK verwendet die **EventsService** und einem Eigenschaft-Bag-Modell.

Zum Beispiel:

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

Weitere Informationen finden Sie auf der Xbox Live SDK-Dokumentation.

**Tipp** können Sie die **xcetool.exe** angegeben mit dem Xbox Live SDK (befindet sich in das Toolverzeichnis) zum Konvertieren der events.man-Datei, die Sie in eine h-Headerdatei aus XDP heruntergeladen. Verwenden der "-X' Option aus, um diese C++-Header zu generieren, indem Sie das neue Eigenschaftsschema Behälter v2 verwenden. Dieser Header enthält C++-Funktionen, die Sie für alle konfigurierten Ereignisse aufrufen können. z. B. **EventWriteMultiplayerRoundStart()**. Wenn Sie eine WinRT-Schnittstelle verwenden lieber, weiterhin finden Sie auf diese Headerdatei, um festzustellen, wie die Eigenschaften und Messungen für jedes der Ereignisse erstellt werden.

### <a name="game-chat"></a>Spiele-chat

GameChat auf der UWP ist als binäre NuGet-Paket mit dem Xbox Live-SDK enthalten. Finden Sie Anweisungen im Xbox Live-Programmierhandbuch für dieses NuGet-Paket Ihrem Projekt hinzufügen.

Grundlegende Syntax ist praktisch identisch ist, zwischen der xdk-Version und die UWP-Versionen. Einige Unterschiede in der API gehören:

1.  Die **User::AudioDeviceAdded** Ereignis muss nicht durch einen Titel für die UWP eingebunden werden. Das zugrunde liegende Chat-Bibliothek Handles Gerät hinzugefügt und entfernt.

2.  **ChatUser** heißt jetzt **GameChatUser**.

3.  **Microsoft::Xbox::GameChat** Namespace bleibt gleich, aber die **Windows::Xbox::Chat** Namespace geworden **Microsoft::Xbox::ChatAudio**.

4.  **AddLocalUserToChatChannelAsync()** akzeptiert entweder eine XUID oder **ChatAudio::IChatUser ^** statt einer **XboxUser**.

5.  **RemoveLocalUserFromChatChannelAsync()** erfordert eine **ChatAudio::IChatUser ^** statt einer **XboxUser**. Sie erhalten eine **IChatUser** aus einem **GameChatUser**-&gt;**Benutzer**.

### <a name="connected-storage"></a>Verbundenen Speicher

Die verbundene Speicherverwaltungs-API finden Sie in der separaten [Xbox Live-Plattform Erweiterungen SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx). Dokumentation ist in der Xbox Live SDK-Dokumentation enthalten.

Der allgemeine Ablauf entspricht dem wie auf der Xbox One, durch das Hinzufügen der **ContainersChangedSinceLastSync** -Eigenschaft in die UWP-Version. Diese Eigenschaft sollte aktiviert sein, wenn Ihre Titel ein Resume-Ereignis, nach dem Aufruf verarbeitet **GetForUserAsync()** erneut, um herauszufinden, was in der Cloud als auch der Titel des Container geändert wurde angehalten. Wenn Daten geladen, im Arbeitsspeicher aus einem der Container, die sich geändert haben, möchten Sie wahrscheinlich Einlesen der Daten, um festzustellen, was geändert und die Änderungen entsprechend verarbeiten.

Weitere wichtige Unterschiede in der UWP-Version gehören:

1.  Namespace-Änderung gegenüber **Windows::Xbox::Storage** zu **Windows::Gaming::XboxLive::Storage**.

2.  **ConnectedStorageSpace** umbenannt **GameSaveProvider**.

3.  **Windows::System::User** werden in **GetForUserAsync()** statt einer **XboxUser**, und die SCID ist jetzt erforderlich.

4.  Kein "Computer" Lokaler Speicher (d. h. **GetForMachineAsync()** entfernt wurde). Erwägen Sie die Verwendung **Windows::Storage::ApplicationData** stattdessen für die kein roaming, lokale Daten speichern.

5.  Async-Ergebnisse werden zurückgegeben, in eine ausnahmefreie \*Result-Type-Objekt (z. B. **GameSaveProviderGetResult**); aus diesem sehen Sie sich die **Status** -Eigenschaft, und wenn kein Fehler vorliegt, Lesen das zurückgegebene Objekt aus der **Wert** Eigenschaft.

6.  **ConnectedStorageErrorStatus Enum** umbenannt **GameSaveErrorStatus** und wird zurückgegeben, der **Status** Eigenschaft eines Ergebnisses. Alle alten Werte vorhanden sind, und einige neue Dateien hinzugefügt wurden:

-   Abbrechen

-   ObjectExpired

-   OK

-   UserHasNoXboxLiveInfo

Finden Sie in der GameSave Beispiel bzw. die NetRumble z. B. Nutzung.

**Beachten Sie** Gamesaveutil.exe entspricht dem xbstorage.exe (das Dienstprogramm Befehlszeilen-Entwickler, die in der xdk-Version enthalten). Nach der Installation des Xbox Live-Plattform Erweiterungen SDK dieses Hilfsprogramms finden Sie hier: "C:"\\Programmdateien (x86)\\Windows-Kits\\10\\Erweiterungs-SDKs\\XboxLive\\1.0\\Bin\\X64

## <a name="summary"></a>Zusammenfassung

Die API-Änderungen und neue Anforderungen, die in diesem Whitepaper beschriebenen sind solche, die Sie stoßen bei der Portierung von vorhandenen spielcode aus dem Xbox One XDK-auf die neue UWP können. Anwendung und Einrichten der Umgebung als auch Funktionsbereiche, die im Zusammenhang mit der Xbox Live-Dienste wie Speicher für mehrere Spieler und verbunden wurde besondere Schwerpunkt liegt dabei erhält. Weitere Informationen folgen Sie den Links in diesem Artikel und in die folgenden Verweise, und finden Sie im Abschnitt "Windows 10" werden die [Entwicklerforen](https://forums.xboxlive.com) für weitere Hilfe, Antworten und Nachrichten.

## <a name="references"></a>Verweise

-   [Portieren von Xbox One auf Windows 10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Xbox One-Whitepaper](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [Beispiele](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
