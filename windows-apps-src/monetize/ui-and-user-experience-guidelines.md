---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "Erfahren Sie mehr über UI- und Benutzererfahrungsrichtlinien in App-Anzeigen."
title: UI- und Benutzererfahrungsrichtlinien in App-Anzeigen
translationtype: Human Translation
ms.sourcegitcommit: 148aca16104f599f3048f5965c4131a3f37799f8
ms.openlocfilehash: 97feb4f79e0592a7b54a8263b15cd2b85dd3243d


---

# <a name="ui-and-user-experience-guidelines-for-ads-in-apps"></a>UI- und Benutzererfahrungsrichtlinien in App-Anzeigen


## <a name="general-ui-resources-for-windows-apps"></a>Allgemeine UI-Ressourcen für Windows-Apps

Informationen zur Gestaltung des Erscheinungsbilds von Apps finden Sie unter [Design und UI](https://developer.microsoft.com/windows/design).

## <a name="adcontrol-best-practices"></a>Bewährte Methoden für AdControl

* [Bewährte Methoden für AdControl: EFFEKTIV](#adcontrolbestpracticesdo10)
* [Bewährte Methoden für AdControl: INEFFEKTIV](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### <a name="adcontrol-best-practices-do"></a>Bewährte Methoden für AdControl: EFFEKTIV

* Binden Sie die Werbung in die Erfahrung ein. Geben Sie den Designern eine Beispielanzeige vor, an der sie erkennen können, wie Sie sich die Werbung vorstellen. Zwei Beispiele für gut durchdachte Anzeigen in Apps sind das Layout, bei der die Anzeige als Inhalt fungiert, und das Split-Layout.

  Probieren Sie aus, wie die verschiedenen Anzeigengrößen in Ihrer App aussehen und funktionieren. Verwenden Sie dazu unsere Anzeigeneinheiten im Testmodus für Windows Phone, Windows 8.1 und Windows 10. Sobald die Testmodus-Anzeigeneinheiten fertig sind, müssen Sie die [App anhand von realen Anzeigeneinheiten-IDs aktualisieren](set-up-ad-units-in-your-app.md), bevor Sie die App zur Zertifizierung übermitteln.

* Planen Sie für Zeiten, wenn keine Anzeigen verfügbar sind. Zu gewissen Zeiten kann es passieren, dass keine Anzeigen an Ihre App gesendet werden. Gestalten Sie Ihre Seiten so, dass sie gut aussehen, unabhängig davon, ob sie eine Anzeige enthalten oder nicht. Weitere Informationen finden Sie unter [Fehlerbehandlung](error-handling-with-advertising-libraries.md).

<span id="adcontrolbestpracticesdont10"/>
### <a name="adcontrol-best-practices-dont"></a>Bewährte Methoden für AdControl: INEFFEKTIV

* Füllen Sie freie Flächen mit Anzeigen aus. Nutzen Sie nicht gleich die erste freie Fläche, die Sie finden können, als Werbefläche. Stattdessen sollten die Anzeigen in den Gesamtentwurf integriert werden.

* Übermäßig viele Anzeigen in der App schalten. Zu viele Anzeigen beeinträchtigen das Erscheinungsbild der App sowie deren Benutzerfreundlichkeit. Sie möchten durch die Werbung zwar Geld verdienen, aber nicht auf Kosten der App selbst.

* Nutzer von ihren Kernaufgaben ablenken. Der Schwerpunkt sollte immer auf der App liegen. Werbeflächen sollten so eingebettet werden, dass sie eine untergeordnete Rolle spielen.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Richtlinien und bewährte Methoden für Interstitialanzeigen

* [Bewährte Methoden für Interstitialanzeigen: EFFEKTIV](#interstitialbestpracticesdo10)
* [Bewährte Methoden für Interstitialanzeigen: INEFFEKTIV](#interstitialbestpracticesavoid10)
* [Bewährte Methoden für Interstitialanzeigen: NIEMALS (durch Richtlinie durchgesetzt)](#interstitialbestpracticesnever10)

Wenn Video-Interstitialanzeigen geschickt eingesetzt werden, können sie den Umsatz aus Ihrer App erheblich erhöhen, ohne dass sich dies negativ auf die Kundenzufriedenheit auswirkt. Werden Sie jedoch unangemessen eingesetzt, dann können solche Anzeigen das genaue Gegenteil bewirken.

Wir bemühen uns, Ihnen bei der geschickten Umsetzung behilflich zu sein. Da Sie Ihre App, abgesehen von den Richtlinien, am besten kennen, überlassen wir Ihnen die endgültige Entscheidung. Beachten Sie dabei, dass die App-Bewertungen und Ihre Einnahmen eng miteinander verknüpft sind.

<span id="interstitialbestpracticesdo10"/>
### <a name="interstitial-best-practices-do"></a>Bewährte Methoden für Interstitialanzeigen: EFFEKTIV

* Bauen Sie Interstitialanzeigen in den natürlichen Fluss der App ein, wie zum Beispiel zwischen den verschiedenen Schwierigkeitsebenen des Spiels.

* Verbinden Sie die Anzeigen mit konkreten Vorteilen, wie beispielsweise:

    * Hinweise, die zum erfolgreichen Abschließen einer Schwierigkeitsebene führen.

    * Zusätzliche Zeit, um eine Ebene auszuführen.

    * Benutzerdefinierte Avatar-Features, wie ein Tattoo oder einen Hut.

* Sollte es bei Ihrer App erforderlich sein, eine Videoanzeige bis zum Schluss anzusehen, dann erwähnen Sie diese Regel im Vorfeld, damit die Nutzer beim Schließen nicht von einer Fehlermeldung überrascht werden.

* Vorabrufen der Anzeige (durch Aufrufen von [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)) im Idealfall 30 – 60 Sekunden, bevor Sie die Anzeige schalten möchten.

* Abonnieren Sie alle vier Ereignisse, die in der [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)-Klasse offengelegt werden(**Canceled**, **Completed**, **AdReady** und **ErrorOccurred**) und setzen Sie sie ein, um die richtigen Entscheidungen für Ihre App zu treffen.

* Stellen Sie anstelle einer vom Server abgestimmten Anzeige einige integrierte Funktionen bereit. Das könnte in einigen Situationen hilfreich sein:

    * Im Offlinemodus, wenn die Anzeigenserver nicht erreicht werden können.

    * Wenn das **ErrorOccurred**-Ereignis ausgelöst wird.

    * Wenn Sie sich dafür entscheiden, basierend auf dem [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) an der Benutzerbandbreite einzusparen, stehen Ihnen in der **ConnectionProfile**-Klasse APIs zur Verfügung, die Ihnen dabei behilflich sein können.

* Verwenden Sie das Standardtimeout (30 Sekunden). Liegt jedoch ein berechtigter Grund vor, der dagegen spricht, dann gehen Sie in diesem Fall nicht unter 10 Sekunden.

    * Das Herunterladen von Videoanzeigen dauert erheblich länger als das von Banner. Dies ist besonders für Märkte wichtig, die nicht über Hochgeschwindigkeitsverbindungen verfügen.

<span/>

* Berücksichtigen Sie daher den Datentarifplan der Nutzer. Verzichten Sie beispielsweise darauf, eine Videoanzeige auf einem mobilen Gerät anzuzeigen, das kurz davor ist, das Datenlimit zu überschreiten oder es bereits überschritten hat bzw. warnen Sie den Nutzer vorher. In der [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx)-Klasse finden Sie APIs, die Ihnen hierbei helfen.

* Optimieren Sie Ihre App nach der ersten Übermittlung fortlaufend. Sehen Sie sich die Anzeigenberichte an und nehmen Sie Designänderungen vor, um die Füll- und Videoabschlussraten zu verbessern.

<span id="interstitialbestpracticesavoid10"/>
### <a name="interstitial-best-practices-avoid"></a>Bewährte Methoden für Interstitialanzeigen: INEFFEKTIV

* Übertreibung. Erzwingen Sie Anzeigen nicht häufiger als alle 5 Minuten oder so, es sei denn der Nutzer verfolgt aktiv einen konkreten Vorteil, der über den eigentlichen Spielverlauf hinausgeht.

* Video-Interstitialanzeigen beim Starten der App schalten – Nutzer könnten annehmen, dass sie die falsche Kachel angeklickt haben.

* Video-Interstitialanzeigen beim Beenden schalten. Dies wäre eine schlechte Inventarentscheidung, da die Abschlussraten wahrscheinlich nahe Null liegen würden.

    * Zwei oder mehr Videoanzeigen unmittelbar nacheinander schalten.

    * Es würde die Nutzer frustrieren, wenn sie feststellen, dass die Statusanzeige für Anzeigen wieder an den Ausgangspunkt zurückgesetzt wurde.

    * Viele Nutzer werden annehmen, dass es sich dabei schlichtweg um einen Fehler bei der Programmierung oder der Anzeigenbereitstellung handelt.

* Abrufen einer Videoanzeige mehr als 5 Minuten vor dem Aufruf einer [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  * Gutes Inventar wird die Konvertierung von vorab abgerufenen Anzeigen in berechenbare Anzeigenaufrufe maximieren.

<span/>

* Bestrafen eines Nutzers für Probleme bei der Anzeigenbereitstellung, d. h., wenn beispielsweise keine Anzeigen verfügbar sind. Wenn Sie beispielsweise eine UI-Option anzeigen, die lautet „Sehen Sie sich eine Anzeige an und erhalten Sie *xxx*“, dann sollten Sie *xxx* auch tatsächlich bereitstellen, wenn der Nutzer seinen Teil erfüllt. Berücksichtigen Sie die folgenden beiden Optionen:

    * Bieten Sie die Option nur an, wenn das [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx)-Ereignis bereits ausgelöst wurde.

    * Sorgen Sie dafür, dass die App eine integrierte Funktion enthält, die die gleichen Vorteile bietet, wie eine wirkliche Anzeige.

* Gewähren Sie einem Nutzer einen Wettbewerbsvorteil in einem Multiplayerspiel.

    * Eine bessere Waffe in einem First-Person-Shooterspiel würde eindeutig in diese Kategorie gehören.

    * Ein speziell für den Avatar des Spielers angefertigtes Hemd ist in Ordnung, solange es keine Tarnung bietet!

<span id="interstitialbestpracticesnever10"/>
### <a name="interstitial-best-practices-never-policy-enforced"></a>Bewährte Methoden für Interstitialanzeigen: NIEMALS (durch Richtlinie durchgesetzt)

* Platzieren Sie niemals UI-Elemente auf dem Anzeigencontainer.

    * Werbekunden haben für den gesamten Bildschirm bezahlt.

<span/>

* Rufen Sie niemals [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) auf, während der Benutzer mit der App beschäftigt ist.

    * Der Benutzer wird dies als lästig empfinden, da [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) den gesamten Bildschirm überlagert.

    * Es könnte auch zu übertriebenen Klickraten führen.

* Verwenden Sie niemals Anzeigen, um etwas zu erhalten, das als Währung eingesetzt oder mit anderen Nutzern getauscht werden könnte.

* Fordern Sie niemals eine neue Anzeige im Kontext des Ereignishandlers für das Ereignis [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx) an. Dies kann zu einer Endlosschleife und Problemen beim Werbedienst führen.

* Fordern Sie niemals eine interstitialanzeige an, um lediglich eine Sicherungsanzeige für eine Wasserfallfolge von Anzeigen zu erhalten. Wenn Sie eine Interstitialanzeige anfordern und anschließend das [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx)-Ereignis erhalten, muss die nächste Interstitialanzeige in Ihrer App die Anzeige sein, die für die Anzeige über die Methode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) bereit ist.

 

 



<!--HONumber=Dec16_HO1-->


