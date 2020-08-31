---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Weitere Informationen finden Sie unter Richtlinien für die Bereitstellung von Benutzeroberflächen-und Benutzerumgebungen mit Banner Anzeigen, Austausch-und systemeigenen anzeigen in ihren apps.
title: Richtlinien und Richtlinien zur Benutzeroberfläche für Werbung
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, Richtlinien, bewährte Methoden
ms.localizationpriority: medium
ms.openlocfilehash: 014cc5a7be370ed5fd579af9bc4ab7600cb96689
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158354"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Richtlinien und Richtlinien zur Benutzeroberfläche für Werbung

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieser Artikel enthält Richtlinien für die Bereitstellung von Werbeeinblendungen, Interstitial-Werbeeinblendungen und systemeigenen Werbung in ihren apps. Allgemeine Informationen zur Gestaltung des Erscheinungsbilds für Apps finden Sie unter [Design und UI](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Jede Verwendung von Werbung in Ihrer APP muss den Microsoft Store Richtlinien entsprechen – einschließlich, ohne Einschränkung, [Richtlinie 10,10](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (Ankündigungs Verhalten und Inhalt). Insbesondere die Implementierung von Banner-oder Interstitial-Werbeeinblendungen in der APP muss die Anforderungen in Microsoft Store Richtlinien [Richtlinie 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content)erfüllen. Dieser Artikel enthält Beispiele für Implementierungen, die gegen diese Richtlinie verstoßen würden. Diese Beispiele dienen lediglich zu Informationszwecken und zum besseren Verständnis der Richtlinie. Diese Beispiele sind nicht vollständig, und es gibt möglicherweise viele andere Möglichkeiten, um gegen die Microsoft Store Richtlinien zu verstoßen, die in diesem Artikel nicht aufgeführt sind.

## <a name="general-best-practices"></a>Allgemeine bewährte Methoden

Bevor Sie unsere Richtlinien für verschiedene Arten von Werbeeinblendungen in diesem Artikel überprüfen, lesen Sie zunächst diese allgemeinen bewährten Methoden, um ihre Werbeeinnahmen zu steigern

* [Planen Sie Ihre Ad-Platzierung sorgfältig](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Weitere Informationen zur [Optimierung der Sichtbarkeit ihrer Ad-Einheiten finden Sie](optimize-ad-unit-viewability.md)in unserem entsprechenden Leitfaden.
* [Verwenden Sie Interstitial Banner Ads als Fall Back für Interstitial-Video anzeigen](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/).
* Stellen [Sie sicher, dass Ihre Benutzer bessere gezielte Werbung erfüllen](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Verwenden Sie die neuesten Werbe Bibliotheken](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Legen Sie die richtigen Coppa-Einstellungen für Ihre APP fest](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/).


## <a name="guidelines-for-banner-ads"></a>Richtlinien für Banneranzeigen

In den folgenden Abschnitten finden Sie Empfehlungen für die Implementierung von [Banner Anzeigen](banner-ads.md) in ihrer App mithilfe von [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) und Beispiele für Implementierungen, die gegen [Richtlinien 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) der Microsoft Store Richtlinien verstoßen.

### <a name="best-practices"></a>Bewährte Methoden

Wir empfehlen diese Methoden beim Implementieren von Banneranzeigen in Ihrer App:

* [Verwenden Sie interaktive Ankündigungs bürogrößen](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/) , die gut mit dem Layout für das Gerät geeignet sind.

* Widmen Sie den Großteil der Benutzeroberfläche Ihrer App Steuerelementen und Inhalten.

* Binden Sie die Werbung in die Erfahrung ein. Geben Sie den Designern eine Beispielanzeige vor, an der sie erkennen können, wie Sie sich die Werbung vorstellen. Zwei Beispiele für gut durchdachte Anzeigen in Apps sind das Layout, bei der die Anzeige als Inhalt fungiert, und das Split-Layout.

  Um zu sehen, wie unterschiedliche Werbe Größen während der Entwicklungs-und Testphase in der APP Aussehen und funktionsfähig sind, können Sie unsere [Test-Ad-Einheiten](set-up-ad-units-in-your-app.md#test-ad-units)nutzen. Wenn Sie mit dem Testen fertig sind, denken Sie daran, [Ihre APP mit Live-Ad-Einheiten zu aktualisieren](set-up-ad-units-in-your-app.md#live-ad-units) , bevor Sie die APP zur Zertifizierung einreichen.

* Planen Sie für Zeiten, wenn keine Anzeigen verfügbar sind. Zu gewissen Zeiten kann es passieren, dass keine Anzeigen an Ihre App gesendet werden. Gestalten Sie Ihre Seiten so, dass sie gut aussehen, unabhängig davon, ob sie eine Anzeige enthalten oder nicht. Weitere Informationen finden Sie unter [Behandeln von AD-Fehlern](error-handling-with-advertising-libraries.md).

* Bei einem Szenario mit Benachrichtigung des Benutzers, das am besten mit einem Overlay abgewickelt wird, rufen Sie [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) auf, zeigen Sie das Overlay an, und rufen Sie dann [AdControl.Resume](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) auf, wenn das Benachrichtigungsszenario abgeschlossen ist.

### <a name="practices-to-avoid"></a>Nicht empfehlenswerte Methoden

Wir empfehlen folgende Methoden beim Implementieren von Banneranzeigen in Ihrer App nicht:

* Füllen Sie freie Flächen nicht mit Anzeigen aus. Nutzen Sie nicht gleich die erste freie Fläche, die Sie finden können, als Werbefläche. Stattdessen sollten die Anzeigen in den Gesamtentwurf integriert werden.

* Schalten Sie nicht übermäßig viele Anzeigen in der App. Zu viele Anzeigen beeinträchtigen das Erscheinungsbild der App sowie deren Benutzerfreundlichkeit. Sie möchten durch die Werbung zwar Geld verdienen, aber nicht auf Kosten der App selbst.

* Lenken Sie die Benutzer nicht von ihren Kernaufgaben ab. Der Schwerpunkt sollte immer auf der App liegen. Werbeflächen sollten so eingebettet werden, dass sie eine untergeordnete Rolle spielen.

### <a name="examples-of-policy-violations"></a>Beispiele für Verstöße gegen Richtlinien

Dieser Abschnitt enthält Beispiele für Banner-Ad-Szenarios, die gegen [Richtlinien 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) der Microsoft Store Richtlinien verstoßen. Diese Beispiele dienen lediglich zu Vorführungszwecken und zum besseren Verständnis der Richtlinie. Diese Beispiele sind nicht umfassend, und möglicherweise gibt es viele weitere Arten, gegen die Richtlinie 10.10.1 zu verstoßen, die hier nicht aufgeführt sind.

* Jegliches Beeinträchtigen der Möglichkeit des Benutzers, die Banneranzeige zu sehen, wie etwa das Verändern der Transparenz von [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) oder das Platzieren eines anderen Steuerelements über **AdControl** (ohne vorheriges Aufrufen von [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Auffordern der Benutzer, auf eine Banneranzeige zu klicken, um eine Aufgabe in Ihrer App auszuführen, oder Zwingen der Benutzer, als Folge des Designs Ihrer App auf Banneranzeigen zu klicken.

* Beliebig geartetes Umgehen des integrierten minimalen Zeitgebers für die Aktualisierung der Banneranzeigen, einschließlich (aber nicht beschränkt auf) Austauschen von [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol)-Objekten oder Erzwingen einer Seitenaktualisierung ohne Eingreifen des Benutzers.

* Verwenden von Live-Ad-Einheiten (d. h. Ad-Einheiten, die Sie aus Partner Center abrufen) während der Entwicklung und Tests oder in einem Emulator.

* Schreiben oder Verteilen von Code, der Anzeigendienste auf andere Weise aufruft als die Microsoft Advertising-Bibliotheken, die im Zusammenhang mit Ihrer App ausgeführt werden.

* Interagieren mit nicht dokumentierten Schnittstellen oder untergeordneten Objekten, die von den Microsoft Advertising-Bibliotheken erstellt wurden, z. B. **WebView** oder **MediaElement**.

* Platzieren von Werbeeinblendungen in eine Viewbox, um die Größe der Werbeeinblendungen zu verringern.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Richtlinien für Interstitialanzeigen

Wenn die [Werbe](interstitial-ads.md) Einblendungen elegant verwendet werden, können Sie Ihre APP-Umsätze erheblich erhöhen, ohne dass sich dies negativ auf die Benutzer Zufriedenheit auswirkt Werden Sie jedoch unangemessen eingesetzt, dann können solche Anzeigen das genaue Gegenteil bewirken.

In den folgenden Abschnitten wird erläutert, wie Sie mit [interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)Interstitial-Video anzeigen und Interstitial-Banner Anzeigen in Ihrer APP implementieren. Außerdem finden Sie hier einige Beispiele für Implementierungen, die gegen [Richtlinien 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) der Microsoft Store Richtlinien verstoßen. Da Sie Ihre App, abgesehen von den Richtlinien, am besten kennen, überlassen wir Ihnen die endgültige Entscheidung. Beachten Sie dabei, dass die App-Bewertungen und Ihre Einnahmen eng miteinander verknüpft sind.

### <a name="best-practices"></a>Bewährte Methoden

Wir empfehlen diese Methoden beim Implementieren von Interstitialanzeigen in Ihrer App:

* Bauen Sie Interstitialanzeigen in den natürlichen Fluss der App ein, wie zum Beispiel zwischen den verschiedenen Schwierigkeitsebenen des Spiels.

* Verbinden Sie die Anzeigen mit konkreten Vorteilen, wie beispielsweise:

    * Hinweise, die zum erfolgreichen Abschließen einer Schwierigkeitsebene führen.

    * Zusätzliche Zeit, um eine Ebene auszuführen.

    * Benutzerdefinierte Avatar-Features, wie ein Tattoo oder einen Hut.

* Wenn Ihre APP erfordert, dass eine Interstitial-Video-Werbe Einfügung auf den Abschluss überprüft wird, erwähnen Sie diese Regel im Vorfeld, damit Sie nicht mit einer Fehlermeldung beim Drücken der Schaltfläche Schließen überrascht werden.

* Vorabrufen der Anzeige (durch Aufrufen von [InterstitialAd.RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)) im Idealfall 30 - 60 Sekunden, bevor Sie die Anzeige schalten möchten.

* Abonnieren Sie alle vier Ereignisse, die in der [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Klasse offengelegt werden(**Canceled**, **Completed**, **AdReady** und **ErrorOccurred**) und setzen Sie sie ein, um die richtigen Entscheidungen für Ihre App zu treffen.

* Stellen Sie anstelle einer vom Server abgestimmten Anzeige einige integrierte Funktionen bereit. Das könnte in einigen Situationen hilfreich sein:

    * Im Offlinemodus, wenn die Anzeigenserver nicht erreicht werden können.

    * Wenn das **ErrorOccurred**-Ereignis ausgelöst wird.

    * Wenn Sie sich dafür entscheiden, basierend auf dem [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) an der Benutzerbandbreite einzusparen, stehen Ihnen in der **ConnectionProfile**-Klasse APIs zur Verfügung, die Ihnen dabei behilflich sein können.

* Verwenden Sie das Standardtimeout (30 Sekunden). Liegt jedoch ein berechtigter Grund vor, der dagegen spricht, dann gehen Sie in diesem Fall nicht unter 10 Sekunden. Das Herunterladen von Interstitial-Werbeeinblendungen ist erheblich länger als bei Standard Banner-Werbeeinblendungen, insbesondere bei Märkten ohne hoch Geschwindigkeits

* Berücksichtigen Sie daher den Datentarifplan der Nutzer. Beispielsweise können Sie den Benutzer vor dem Ausführen einer Interstitial-Videoanzeige auf einem mobilen Gerät nicht anzeigen oder warnen, das das datenlimit fast/überschreitet. In der [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile)-Klasse finden Sie APIs, die Ihnen hierbei helfen.

* Optimieren Sie Ihre App nach der ersten Übermittlung fortlaufend. Sehen Sie sich die [AD-Berichte](../publish/advertising-performance-report.md) an, und nehmen Sie Entwurfs Änderungen vor, um die Video Vervollständigungs Raten für Fill und Interstitial

### <a name="practices-to-avoid"></a>Nicht empfehlenswerte Methoden

Wir empfehlen folgende Methoden beim Implementieren von Interstitialanzeigen in Ihrer App nicht:

* Übertreiben Sie es nicht. Erzwingen Sie Anzeigen nicht häufiger als etwa alle 5 Minuten, es sei denn der Benutzer verfolgt aktiv einen konkreten Vorteil, der über den eigentlichen Spielverlauf hinausgeht.

* Zeigen Sie beim Starten der APP keine Austausch Vorgänge an, da Benutzer der Ansicht sind, dass Sie auf die falsche Kachel geklickt haben.

* Beim Beenden werden keine Austausch Vorgänge angezeigt. Dies wäre eine schlechte Inventarentscheidung, da die Abschlussraten wahrscheinlich nahe Null liegen würden.

* Schalten Sie nicht zwei oder mehr Interstitialanzeigen nacheinander. Es würde die Benutzer frustrieren, wenn sie feststellen, dass die Statusanzeige für Anzeigen wieder an den Ausgangspunkt zurückgesetzt wurde. Viele Benutzer werden annehmen, dass es sich dabei schlichtweg um einen Fehler bei der Programmierung oder der Anzeigenbereitstellung handelt.

* Rufen Sie eine Interstitial-Videoanzeige nicht mehr als 5 Minuten vor dem Aufruf von [interstitialad. Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)auf. Gutes Inventar wird die Konvertierung von vorab abgerufenen Anzeigen in berechenbare Anzeigenaufrufe maximieren.

* Bestrafen Sie Benutzer nicht für Probleme bei der Anzeigenbereitstellung, d. h., wenn beispielsweise keine Anzeigen verfügbar sind. Wenn Sie beispielsweise eine UI-Option anzeigen, die lautet „Sehen Sie sich eine Anzeige an und erhalten Sie *xxx*“, dann sollten Sie *xxx* auch tatsächlich bereitstellen, wenn der Benutzer seinen Teil erfüllt. Berücksichtigen Sie die folgenden beiden Optionen:

    * Bieten Sie die Option nur an, wenn das [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready)-Ereignis bereits ausgelöst wurde.

    * Sorgen Sie dafür, dass die App eine integrierte Funktion enthält, die die gleichen Vorteile bietet, wie eine wirkliche Anzeige.

* Verwenden Sie keine Interstitialanzeigen, damit ein Benutzer einen Wettbewerbsvorteil in einem Multiplayer-Spiel erhält. Locken Sie Benutzer beispielsweise nicht mit einer besseren Waffe in einem Ego-Shooter, wenn sie eine Interstitialanzeige ansehen. Ein individuelles T-Shirt für den Avatar des Spielers ist in Ordnung, solange es keine Tarnung bietet!

### <a name="examples-of-policy-violations"></a>Beispiele für Verstöße gegen Richtlinien

Dieser Abschnitt enthält Beispiele für Szenarios mit Interstitial-AD, die gegen [Richtlinien 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) der Microsoft Store Richtlinien verstoßen. Diese Beispiele dienen lediglich zu Vorführungszwecken und zum besseren Verständnis der Richtlinie. Diese Beispiele sind nicht umfassend, und möglicherweise gibt es viele weitere Arten, gegen die Richtlinie 10.10.1 zu verstoßen, die hier nicht aufgeführt sind.

* Das Platzieren eines Benutzeroberflächenelement des Containers für Interstitialanzeigen.

* Das Aufrufen von [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show), während der Benutzer mit der App beschäftigt ist.

* Das Verwenden von Interstitialanzeigen, um etwas zu erhalten, das als Währung eingesetzt oder mit anderen Benutzern getauscht werden könnte.

* Das Anfordern neuer Interstitialanzeigen im Kontext des Ereignishandlers für das Ereignis [InterstitialAd.ErrorOccurred](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Dies kann zu einer Endlosschleife und Problemen beim Werbedienst führen.

* Das Anfordern von Interstitialanzeigen, nur um eine Sicherungsanzeige für eine Wasserfallfolge von Anzeigen zu erhalten. Wenn Sie eine Interstitialanzeige anfordern und anschließend das [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready)-Ereignis erhalten, muss die nächste Interstitialanzeige in Ihrer App die Anzeige sein, die für die Anzeige über die Methode [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) bereit ist.

* Verwenden von Live-Ad-Einheiten (d. h. Ad-Einheiten, die Sie aus Partner Center abrufen) während der Entwicklung und Tests oder in einem Emulator.

* Schreiben oder Verteilen von Code, der Anzeigendienste auf andere Weise aufruft als die Microsoft Advertising-Bibliotheken, die im Zusammenhang mit Ihrer App ausgeführt werden.

* Interagieren mit nicht dokumentierten Schnittstellen oder untergeordneten Objekten, die von den Microsoft Advertising-Bibliotheken erstellt wurden, z. B. **WebView** oder **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Richtlinien für Native Werbung

[Native Werbung](native-ads.md) bieten Ihnen viel Kontrolle darüber, wie Sie Ihren Benutzern Werbeinhalte präsentieren. Befolgen Sie diese Anforderungen und Richtlinien, um sicherzustellen, dass die Nachricht des inserers Ihre Benutzer erreicht und gleichzeitig die Bereitstellung einer verwirrenden nativen Werbeanzeige für Ihre Benutzer verhindert.

### <a name="register-the-container-for-your-native-ad"></a>Registrieren des Containers für das Native AD

In Ihrem Code müssen Sie die [registeradcontainer](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) -Methode des [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) -Objekts aufrufen, um das UI-Element zu registrieren, das als Container für das systemeigene AD und optional alle spezifischen Steuerelemente fungiert, die Sie als durch Klicken aktivierbare Ziele für die Werbe einblungen registrieren möchten. Dies ist zum ordnungsgemäßen Nachverfolgen von werbeeindrücken und Klicks erforderlich.

Es gibt zwei über Ladungen für die **registeradcontainer** -Methode, die Sie verwenden können:

* Wenn Sie möchten, dass der gesamte Container für alle einzelnen nativen AD-Elemente klickbar ist, müssen Sie die **registeradcontainer (FrameworkElement)** -Methode aufrufen und das Container-Steuerelement an die-Methode übergeben. Wenn Sie z. b. alle systemeigenen AD-Elemente in separaten Steuerelementen anzeigen, die alle in einem **StackPanel** gehostet werden, und Sie möchten, dass das gesamte **StackPanel** klickbar ist, übergeben Sie das **StackPanel** an diese Methode.

* Wenn Sie möchten, dass nur bestimmte systemeigene AD-Elemente klickbar sind, können Sie die **registeradcontainer (FrameworkElement, IVector (FrameworkElement))** -Methode aufrufen. Nur die Steuerelemente, die an den zweiten Parameter übergeben werden, sind klickbar.

### <a name="required-native-ad-elements"></a>Erforderliche systemeigene AD-Elemente

Sie müssen mindestens die folgenden nativen AD-Elemente, die von den Eigenschaften des [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) -Objekts bereitgestellt werden, für den Benutzer in ihrem nativen Ad-Design anzeigen. Wenn Sie diese Elemente nicht einschließen, sehen Sie möglicherweise schlechte Leistung und niedrige Ergebnisse für Ihre Ad-Einheit.

1. Zeigen Sie immer den Titel des nativen AD an (verfügbar in der **Title** -Eigenschaft). Stellen Sie ausreichend Platz zum Anzeigen von mindestens 25 Zeichen bereit. Wenn der Titel länger ist, ersetzen Sie den zusätzlichen Text durch ein Ellipsen.
2. Zeigen Sie immer mindestens eines der folgenden Elemente an, um die systemeigene AD-Darstellung von der restlichen APP zu unterscheiden, und stellen Sie eindeutig fest, dass der Inhalt von einem Werbe Benutzer bereitgestellt wird:
    * Das unterscheidbare *AD* -Symbol (verfügbar in der **Adicon** -Eigenschaft). Dieses Symbol wird von Microsoft bereitgestellt.
    * Der *von Text gesponserte* (verfügbar in der Eigenschaft " **sponsoredby** "). Dieser Text wird vom-Werbedienst bereitgestellt.
    * Als Alternative zu dem *von Ihnen gesponserten* Text können Sie auch einen anderen Text anzeigen, mit dem Sie die systemeigene AD-Darstellung von der restlichen App unterscheiden können, z. b. "gesponserter Inhalt", "Aktions Inhalte", "Empfohlene Inhalte" usw.

### <a name="user-experience"></a>Benutzererfahrung

Ihr System eigenes AD sollte eindeutig von der restlichen App getrennt sein und über genügend Platz verfügen, um versehentliche Klicks zu vermeiden. Verwenden Sie Rahmen, andere Hintergründe oder eine andere Benutzeroberfläche, um den AD-Inhalt von der restlichen APP zu trennen. Beachten Sie, dass versehentliche Werbe Klicks für Ihren AD-basierten Umsatz oder Ihre Endbenutzer Zeit langfristig nicht vorteilhaft sind.

### <a name="description"></a>BESCHREIBUNG

Wenn Sie die Beschreibung für den AD (in der **Description** -Eigenschaft des **NativeAdV2** -Objekts verfügbar) anzeigen möchten, geben Sie ausreichend Speicherplatz an, um mindestens 75 Zeichen anzuzeigen. Es wird empfohlen, dass Sie eine Animation verwenden, um den vollständigen Inhalt der AD-Beschreibung anzuzeigen.

### <a name="call-to-action"></a>Call-to-Action (Handlungsaufforderung)

Der *Aufruf von Action* Text (in der **calldeaction** -Eigenschaft des **NativeAdV2** -Objekts verfügbar) ist eine wichtige Komponente von AD. Wenn Sie diesen Text anzeigen möchten, beachten Sie die folgenden Richtlinien:

* Zeigen Sie den *Aufruf des Aktions* Texts immer an den Benutzer für ein Klick bares Steuerelement an, z. b. eine Schaltfläche oder einen Hyperlink.
* Zeigen Sie den *Aufrufen des Aktions* Texts immer vollständig an.
* Stellen Sie sicher, dass der *Aktions Text aufgerufen* von dem verbleibenden Aktions Text des Werbe eintexts getrennt ist.