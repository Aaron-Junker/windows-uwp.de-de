---
title: Erste Schritte mit Cross-spielen
description: Erfahren Sie, wie übergreifende-Play-Spiele zu entwickeln, die auf dem PC und Xbox One-Konsolen ausgeführt.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, einen Cross-Play, überall spielen
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646725"
---
# <a name="get-started-with-cross-play-games"></a>Erste Schritte mit Cross-spielen

## <a name="summary"></a>Zusammenfassung

Mit der Einführung von Windows 10 ist die Spieleentwickler Freigeben der einzelnen Produkte, die sowohl für Xbox One (als ein Spiel xdk-Version) und Windows 10 (wie ein UWP-Spiel) möglich. In einigen Fällen werden Entwickler Cross-Play für diese Spiele, aktivieren möchten, in denen die Xbox One und Windows 10-Versionen von ihr Spiel Xbox Live-Dienste wie Multiplayer-Spiele, Spiele zu speichern, Erfolge und mehr vereinheitlicht sind. Um Cross-Funktion zu aktivieren, werden dieser Spiele sowohl die xdk-Version als auch die UWP-Version des Spiels eine einzelne Konfiguration für Titel-ID und Xbox Live-Dienst freigeben.

Erfassen einer xdk-Version + UWP-heute-spielen erfordert die 4 wichtigsten Schritte:

1.  Erstellen Sie Ihr UWP-Produkt im Partner Center

2.  Erstellen Sie Ihr Spiel xdk-Version in XDP, Auswählen der Zielplattformen XBL-Konfiguration in Teilen

3.  Binden Sie Ihren UWP-Produktinformationen an Ihrem Produkt xdk-Version in XDP

4.  Konfigurieren und Veröffentlichen von Xbox Live über XDP

Der Zweck dieses Dokuments besteht darin diese 4 Schritte, um es so einfach wie möglich für Xbox-Mitarbeiter zum Erfassen von xdk-Version + UWP plattformübergreifende Spiele machen weitere Details

## <a name="terminology"></a>Terminologie

### <a name="scenario-terms"></a>Szenario-Begriffe

1.  Cross-wiedergeben: Ein Spiel, die auf mehreren Plattformen veröffentlicht, aber die gemeinsam eine einzelne Xbox ID des Titels und Dienstkonfiguration. Das Endergebnis ist, dass beide Versionen des Spiels auf der gleiche Xbox Live-Konfiguration – Erfolge, Bestenlisten, Spiele speichert, Multiplayer-Spiele und mehr verwenden.

2.  Partner Center: Die [Portal](https://partner.microsoft.com/dashboard) , in dem Sie für die Verwendung in UWP-Entwicklung heute und Setup Xbox Live-Konfiguration für UWP-app-Identitäten reservieren können.

3.  XDP: Xbox-Entwicklerportal, die heute vorhanden ist, zu erfassen, zu konfigurieren, und veröffentlichen Sie Spiele für Xbox One XDK und durchgeführt und sehen die hinzugefügten Verwendung zu erfassen, konfigurieren und veröffentlichen Sie xdk-Version + UWP Cross-Play-Spiele.

### <a name="identity-terms"></a>-Identität

1.  Titel-ID: Dies ist die Xbox Title-ID, mit dem jedes Spiel Xbox Live. Ein Titel-ID ordnet ein einzelnes Produkt, in denen mehrere Plattformen erstrecken.

2.  Dienst-Konfigurations-ID (SCID): Jeder Xbox-Titel (identifiziert durch einen Titel-ID) verfügt über einen entsprechenden Dienst Konfigurations-ID (auch bekannt als SCID). Diese ID ermöglicht das Xbox Live, zur eindeutigen Identifizierung die Regeln / Konfiguration verwendet wird, wenn durch den Titel zu interagieren.

3.  Paketfamiliennamens (PFN): Dies ist eine zugewiesene Identität zu jedem Produkt im Partner Center erstellt. Nachdem Sie Ihrem UWP an die Identität dieses Partner Center-Produkts binden, dauert es auf diese PFN. PFNs sind eindeutige Produkt-IDs, bei die mehrere Plattformen erstrecken. PFNs sind 1:1 mit den IDs der Xbox-Titel.

4.  MSA-App-ID: MSA-Client-ID, ist dies auch eine andere app-Identität, die bei der Erstellung des Produkts im Partner Center von MSA zugewiesen. Diese Identität kann Microsoft-Dienste, die Ihre app identifizieren. MSA-App-IDs sind 1:1 mit PFNs (und entsprechend mit den IDs der Xbox-Titel).

## <a name="scenario-overview"></a>Szenarioübersicht

### <a name="what-is-cross-play"></a>Was ist die Cross-Play?

Ein Showcase Windows 10 auftreten; Cross-Play ist Geräteübergreifende Spiele zwischen dem Xbox One und PC bei Spielen, die Freigabe einer einzelnen Xbox Live-Konfigurations für Szenarios wie Geräteübergreifende Multiplayer-Spiele, Erfolge und Bestenlisten, light-Geräteversionen aus, und Game speichert.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>Was sind die vor- und Nachteile von Cross-Play?

Cross-Play ist wahrscheinlich der richtige Ansatz für Sie ggf. die xdk-Version und UWP-Versionen von Ihr Spiel:

-   Engagieren Sie sich in Geräteübergreifende Multiplayer-Spiele (Xbox One-Vs. PC) in mindestens ein Multiplayer-Spiele-Modus

-   Teilen Sie ein einzelnes Spiel speichern, die der Benutzer auf beiden Geräten verwenden können

-   Haben Sie einen Satz von Erfolge und Gamerscore / Herausforderungen / Bestenlisten, die sie auf beiden Geräten lichtdurchlässigen für Status können

Cross-Play ist wahrscheinlich nicht der richtige Ansatz für Sie, wenn:

-   Sie möchten, um zu verhindern, dass die Spieler PC und Xbox One Ihres Spiels ausüben Multiplayer-Spiele auf Geräten alle Multiplayer-Spiele-Modi

-   Xbox One bleiben sollen, und speichern Sie PC-Spiel separate (z. B. Sicherheit oder Gründen Vertrauensstellung)

-   Sie möchten die Xbox One und PC-Versionen Ihres Spiels separate Gamerscore haben (auch bekannt als können Benutzer, die Xbox One und PC kaufen 1000 Gamerscore für jede Plattform, anstatt einen gemeinsam genutzten 1000 empfangen)

Cross-Play fügt im Allgemeinen den größten Nutzen:

-   Free-to-Play / Xbox Spielen überall, der Kontinuität zwischen der Xbox One und PC-Version des Spiels hervorheben

-   Spiele mit Geräteübergreifende Multiplayer zwischen Xbox One und PC

**HINWEIS**: Cross-Play ist sowohl als Spiele, die die xdk-Version und UWP-Versionen von ihr Spiel gleichzeitig freigeben sowie Spiele, die bereits versendeten ein xdk-Version, aber fügen eine UWP-Version verfügbar.

### <a name="what-are-the-restrictions-of-cross-play"></a>Was sind die Einschränkungen von Cross-Play?

Bis Xbox One UWP Spiele unterstützt, müssen Cross-Play Games, ein xdk-Version (für Xbox One-Konsole) und UWP (für Windows 10-PCs)-Version des Spiels.

Alle xdk-Version und UWP-Cross-Spielen des Spiels enthält einige wichtigen Einschränkungen:

1.  **Titel der xdk-Version müssen in XDP erfasst werden**. Sowohl von einer Dienstkonfiguration und die Hauptlinie Benutzererlebnis bei der Veröffentlichung ist Partner Center nicht ausgestattet, um xdk-Version basierend Titel zu unterstützen.

2.  **Eine einzelne Konfiguration in XDP erstellt werden kann sowohl von der ein Spiel xdk-Version und UWP-Version verwendet**. Wir haben XDP können ein Spiel teilen Sie eine einzelne Konfiguration zwischen der xdk-Version und UWP-Versionen neue Features hinzugefügt. Die UWP-Version müssen dennoch im Partner Center veröffentlicht werden, für Pakete / Katalog, aber alle dienstveröffentlichung Konfiguration kann in XDP ausgeführt werden.

3.  **Dienstkonfiguration nicht aufgeteilt werden, zwischen XDP und Partner Center**. XDP und Partner Center kennen nicht voneinander – in eine übermäßige Schreibvorgänge, die von einem anderen vorhandenen veröffentlicht werden. veröffentlichen. Dies kann zum irreparabel Unterbrechen der Dienstkonfiguration und erstellen Sie terrible Benutzeroberflächen (verschwindenden Gradienten Erfolge, verloren gehen, Spiele speichert usw.), und daher 100 % der Dienstkonfiguration in XDP für Cross-Play-Spiele xdk-Version + UWP ausgeführt werden muss.

### <a name="create-your-uwp-product-in-partner-center"></a>Erstellen Sie Ihr UWP-Produkt im Partner Center

Erstellen Sie ein UWP-Produkt im Partner Center an, indem Sie im Leitfaden: [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>Richten Sie Ihres Produkts xdk-Version in XDP ein

Nun, da Ihr UWP erstellt wird, können Sie Ihr Produkt xdk-Version in XDP einrichten. Wenn Sie nicht bereits über einen Titel ein xdk-Version verfügen, müssen Sie eine erstellen.

### <a name="create-your-xdp-product"></a>Erstellen Sie Ihr Produkt XDP

Arbeiten mit Ihren Account Manager zum Erstellen eines neuen Produkts unter des Herausgebers in XDP ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

Beim Erstellen des Produkts in XDP stellen Sie sicher, dass Sie einen Bildlauf zum unteren Rand der linken Teil der Benutzeroberfläche auf den Plattformen Ihrer durchführen. Überprüfen Sie jede Plattform, die Sie **beabsichtigen, eines Tages** release das Spiel auf mit Xbox Live-Cross-Play-Integration.

Nachdem Sie Ihre Plattformen ausgewählt haben, geben Sie den Zugriff auf Ressourcen für Ihr Spiel (wahrscheinlich einen Titel xdk-Version) und des beabsichtigten Veröffentlichungsmechanismus für dieses Produkt.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>Aktualisieren Sie Ihre XDP-Produkt-Plattformen

Wenn Sie bereits ein vorhandenes Produkt mit xdk-Version in XDP haben, muss sie aktualisiert werden, um die PC-Plattform zu unterstützen. Zu diesem Zweck auf der Produkt, navigieren Sie auf der Produkt-Setup &gt; Plattformtyp.

![](../images/ingesting_crossplay_games_xdp/image10.png)

Auf dieser Seite Wählen Sie die Plattformen, die Sie unterstützen möchten (die Optionen sind Xbox One, Schwellenwert für PC und Windows Mobile). Sobald Sie mit der Auswahl zufrieden sind, wählen Sie die Schaltfläche "Übermitteln Platforms" aus.

Diese Änderung wird sofort live vorgenommen werden (keine Notwendigkeit für eine Dienstkonfiguration, Katalog oder binär veröffentlichen für diesen um wirksam zu werden). Beachten Sie, dass diese Konfiguration Sandkästen umfasst – keine andere Plattformtypen pro Sandbox für Ihr Spiel.

### <a name="enter-your-msa-app-id"></a>Geben Sie Ihre MSA-App-ID

Sobald das Produkt XDP erstellt wurde, wechseln Sie zur Produkt-Setup-Seite für das Produkt, die zuvor erstellte MSA-App-ID einzugeben. Produkt-Setup erreichen Sie durch Klicken auf die am weitesten links "Status Box" in der Leiste "Aufgaben" für das Produkt XDP, wie unten dargestellt.

![](../images/ingesting_crossplay_games_xdp/image11.png)

Nachdem Sie es auf der Produkt-Setup-Seite haben, wählen Sie im Abschnitt "Application ID Setup". In diesem Bereich können Sie geben Sie den MSA-App-ID, die Sie abgerufen haben, und platzieren Sie es in das Feld "Anwendungs-ID", wie unten dargestellt.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**Sie** **müssen nicht zur Eingabe von Name und Herausgeber Attribution**, **und insbesondere sollten nicht den Link "Anwendungs-ID abrufen" auf der Seite**, wie Sie bereits ein MSA-App-ID verfügen, Sie in diesem eingeben müssen, Feld, und möchten nicht, dass eine neue Ressourcengruppe für Ihre Anwendung generiert.

Wenn Sie Ihrer MSA-App-ID im Feld "Anwendungs-ID" eingegeben haben, klicken Sie auf die Schaltfläche "Übermitteln der Anwendung-ID-Setup". Dadurch Speichern Ihrer MSA-App-ID-Informationen mit Xbox Live-Sicherheit – Wenn Sie eine Anforderung zum Abrufen einer XToken aus Ihrer UWP, vornehmen der Title-Anspruch, der in enthalten jetzt für dieses Produkt XDP ordnet (solange die AppX-Manifest-Identität, die Sie Erstellen von UWP ordnungsgemäß verwendet wird d im Partner Center!)

### <a name="enter-the-partner-center-pfn-into-xdp"></a>Geben Sie den PFN Partner Center in XDP

Die oben genannten Schritte sind ausreichend, um Ihr UWP-Spiel authentifiziert und Xbox Live verwenden, mit der Dienstkonfiguration, die Sie erstellen und Veröffentlichen in XDP zu erhalten, müssen bestimmte Xbox Live-Features (z. B. Multiplayer-Einladungen) Ihrer UWP-Spiel PFN beachtet werden, um ordnungsgemäß zu funktionieren.

Navigieren Sie zu diesem Zweck mit der Einrichtung des Produkts &gt; Dev Center-Bindung

![](../images/ingesting_crossplay_games_xdp/image13.png)

Geben Sie auf dieser Seite den PFN für Ihre UWP-app (wie im Abschnitt 4.1.1 abgerufen wird), und wählen Sie dann die Schaltfläche "Speichern".

Diese Konfiguration wird nicht sofort live vorgenommen. Es wird über die Zukunft live geschaltet Dienstkonfiguration, die in einer Sandbox veröffentlicht. Daher ist es möglich, diese Informationen Sandkasten und jede Sandbox verfügbar ist, veröffentlicht werden muss.

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>Kennzeichnen Sie Ihre App für Xbox-Zertifikat im Partner Center

Abrufen von Ihr Spiel Xbox Live für die Xbox-Zertifikats aktiviert ernannt werden aktuell einigen manuellen Eingriffen benötigt. Arbeiten mit den releasemanager so kennzeichnen Sie Ihre app ist für die Erkennung von SmartCert Xbox Live im Partner Center aktiviert.

### <a name="update-your-uwp-game-in-the-xbox-app"></a>Aktualisieren Sie Ihr UWP-Spiel in der Xbox-App

In der Regel verwendet die Xbox-App ein automatisch generierter Titel-ID für alle UWP-Spiele mit Exponenten, die die Xbox Live in der Xbox-App auftritt. Um Ihre UWP-Spiel ordnungsgemäß XDP generierter Titel-ID in der Xbox-App verwenden, muss ein Datenupdate in Partner Center vorgenommen werden **vor dem Übermitteln Ihrer UWP-Spiel für Version.**

Zu diesem Zweck wenden Sie sich an Ihre Mutter, und teilen Sie ihnen mit, dass Sie Ihre Xbox-App-Titel-ID für den Titelnamen Ihrer aktualisieren möchten.  Achten Sie darauf, dass Sie die Title-ID, die in XDP erstellt enthalten (sichtbar im XDP unter Produkt-Setup &gt; Produktdetails) und die URL für Windows 10 für Ihre UWP (sichtbar im Partner Center-App-Verwaltung &gt; App-Identität).

## <a name="configure-xbox-live-in-xdp"></a>Konfigurieren von Xbox Live in XDP

### <a name="service-configuration"></a>Dienstkonfiguration

Mit unserer XDP und Partner Center-Produkten ordnungsgemäß konfiguriert und gebunden können Sie Ihre Xbox Live-Freigabekonfiguration in XDP einrichten, wie gewohnt für einen Titel ein xdk-Version verwenden.

**Erinnerung** – gebundenen xdk-Version + UWP Spiele sollten, unter keinen Umständen, aktivieren, konfigurieren oder Xbox Live-Dienstkonfiguration über das Partner Center zu veröffentlichen. Fehler bei dieser Anleitung befolgen kann die Xbox Live-Konfiguration für Ihr Spiel dauerhaft beschädigt werden.

### <a name="catalog-configuration"></a>Katalogkonfiguration

Für ein xdk-Version + UWP-Spiel, Katalogkonfiguration muss Setup zweimal werden: einmal in XDP für Ihre xdk-Version und auch in Partner Center für Ihre UWP.

Für die Konfiguration XDP ist dies genau dasselbe wie eine normale xdk-Version. Für die Partner Center-Konfiguration, ausführlichere Schritte finden Sie [hier](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Bei nur UWP-Spielen muss nur eine begrenzte Anzahl von Katalogkonfiguration eingerichtet werden. Insbesondere sollten Marketing-Informationen für nur UWP-Spiele, ausgefüllt werden, wie die Dienstkonfiguration nutzt die Zeichenfolgen, die hier eingegeben werden, für alle XDP konfigurieren Spiel Xbox Live. Darüber hinaus muss Verfügbarkeit Setup mit "nicht verfügbar", stellen Sie sicher, dass das Spiel nie im Xbox One-Katalog bei die Kataloginformationen angezeigt werden, für das Spiel veröffentlicht wird.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>Binary-Konfiguration

Für ein xdk-Version + UWP-Spiel, binäre Konfiguration Setup zweimal werden muss: einmal in XDP für Ihre xdk-Version und auch in Partner Center für Ihre UWP.

Für die Konfiguration XDP ist dies genau dasselbe wie eine normale xdk-Version. Für die Partner Center-Konfiguration, ausführlichere Schritte finden Sie [hier](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Für nur UWP-Spiele besteht keine Notwendigkeit für binäre Konfiguration in XDP zur Verfügung. Sie können diesen Schritt vollständig überspringen.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>Publish in XDP

Veröffentlichen ein xdk-Version + UWP-Spiel im Allgemeinen erfolgt auf dieselbe Weise als einen Titel ein xdk-Version separat in XDP und Ihre UWP im Partner Center zu veröffentlichen. Daher die folgenden konzentriert sich auf die eindeutige Prozess oder Punkte beachten für Cross-Play-Spiele.

### <a name="dev-sandbox-publishing"></a>Dev-Sandbox-Veröffentlichung

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Keine Entsprechung des Dev-Sandbox-Katalog für Microsoft Store

Während XDP Ihr Katalog & Binärdatei in der Dev-Sandbox-Version des Katalogs, Xbox One veröffentlichen kann, hat der Microsoft Store-Katalog keine Sandbox-Unterstützung. Daher Testen Ihre UWP in einer Sandbox Dev erfordert, dass Sie zum Sideloaden, UWP und direkt abzuspielen. Dies hat keinerlei Auswirkung auf Xbox Live testen, aber Sie können Ihre Standard Testverfahren ändern.

<table>
  <tr>
    <td>
      Für ein nur für UWP-Spiel, ist es immer noch erforderlich zum Veröffentlichen der xdk-Version veröffentlichen Softwaretitel-Kataloginformationen Dienstkonfiguration Blockierung aufheben, obwohl das nur für UWP-Spiel nicht Xbox One-Katalog vorhanden ist.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>CERT-Sandbox-Veröffentlichung

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Koordination zwischen XDP Veröffentlichung und der Partner Center-Veröffentlichung

Verschieben von CERT auf "RETAIL" in XDP sind zwei separate, explizite Aktionen aus. Im Partner Center verschiebt der Übermittlungsprozess jedoch automatisch Ihr Spiel durch die Zertifizierung und sich im Einzelhandel. Es ist daher eine Reihenfolge der Vorgänge zu berücksichtigen.

Wenn Sie zur Zertifizierung wechseln möchten, sollten Sie die folgenden Schritte aus, in der Reihenfolge befolgen:

1.  Veröffentlichen Sie Ihr Produkt xdk-Version in XDP auf Zertifikat (einschließlich der Katalog, Binär- und Service-Konfiguration)

2.  Starten Sie die Partner Center-Übermittlung für Ihre UWP-Produkt

    1.  **Wählen Sie "Veröffentlichen dieser app manuell" oder "frühestens \[Datum\]" im Datumsfeld veröffentlichen!** Wenn Sie dies nicht tun, kann Ihr UWP-Spiel freigegeben werden um ohne Ihr Zutun automatisch zur einzelhandelsanalyse

<table>
  <tr>
    <td>
      Ein nur für UWP-Spiel ist es erforderlich, damit Ihre Katalog- und Service-Konfiguration auf Zertifikat in XDP veröffentlichen vor Ihrer Übermittlung Partner Center, obwohl Sie nicht über eine Title-Binärdatei xdk-Version verfügen, die Sie veröffentlichen.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>Einzelhandel-Sandbox-Veröffentlichung

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Koordination zwischen XDP Veröffentlichung und der Partner Center-Veröffentlichung

Nach dem Titel des xdk-Version-Zertifizierung abgeschlossen ist, und Ihre UWP-Zertifizierung verlassen hat und jetzt veröffentlicht werden, sind Sie bereit zum Veröffentlichen Ihrer app auf "RETAIL". In diesem Fall ist es wichtig, gehen Sie wie in einer bestimmten Reihenfolge aus, um sicherzustellen, dass Ihr xdk-Version-Titel und die UWP ausgerichtet bleiben.

1.  Sobald Sie fertig sind, veröffentlichen Sie Ihr Produkt xdk-Version in XDP auf "RETAIL" (einschließlich der Katalog, Binär- und Service-Konfiguration)

2.  Auf der Seite Zertifizierung Ihres Produkts, in Partner Center auswählen "Jetzt veröffentlichen", wenn Sie zuvor ausgewählt haben, "Manuell veröffentlichen Sie diese app", oder warten Sie, bis das Publish to auftreten, wenn Sie ausgewählt "frühestens \[Datum\]".

Nachdem diese Schritte abgeschlossen haben, sollten Sie Ihr xdk-Version-Titel und die UWP-Spiel auf der ganzen Welt, mit einer shared Service-Konfiguration im Einzelhandel veröffentlicht verfügen. Gratulation!

<table>
  <tr>
    <td>
      Nur UWP-Spiel es ist weiterhin erforderlich, dass Ihr Katalog zu veröffentlichen, und Service-Konfiguration auf "RETAIL" in XDP vor der Veröffentlichung im Partner Center, andernfalls die freigegebene UWP abgeschlossen ist ist nicht in der Lage, Xbox Live-Zugriff auf.
    </td>
  </tr>
</table>
