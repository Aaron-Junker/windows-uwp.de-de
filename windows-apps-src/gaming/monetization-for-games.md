---
title: Monetarisierung für Spiele
description: Implementieren Sie Banneranzeigen, Videointerstitialanzeigen und In-App-Käufe für Universal Spiele für die universelle Windows-Plattform (UWP) unter Windows 10.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Monetarisierung
ms.localizationpriority: medium
ms.openlocfilehash: 009d4740fed47c7cde392d41bf52384071715106
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104621"
---
#  <a name="monetization-for-games"></a>Monetarisierung für Spiele

Als Spieleentwickler müssen Sie Ihre Monetisierungsoptionen kennen, um die Rentabilität Ihres Unternehmens gewährleisten und weiterhin Ihrer Leidenschaft nachgehen zu können: der Entwicklung großartiger Spiele. Dieser Artikel enthält eine Übersicht über die Monetarisierungsmethoden für ein Spiel für die universelle Windows-Plattform (UWP) und ihre Implementierung.

Bisher haben Sie Ihr Spiel einfach mit einem Preis versehen und gewartet, dass es von Benutzern in einem Geschäft erworben wird. Heute haben Sie jedoch verschiedene Optionen. Sie können ein Spiel in einem normalen Geschäft anbieten, es online (als physische Version oder als Softcopy) verkaufen oder Benutzer das Spiel kostenlos spielen lassen, dabei aber gewisse Anzeigen oder In-Game-Gegenstände integrieren, die erworben werden können. Spiele sind auch nicht mehr einfach eigenständige Produkte. Sie werden häufig mit zusätzlichem Inhalt bereitgestellt, der zusätzlich zum Hauptspiel erworben werden kann.

Sie können ein UWP-Spiel folgendermaßen bewerben und monetisieren:
* Bringen Sie Ihr Spiel in den Microsoft Store, ein sicheres Onlinegeschäft, das eine [weltweite Verteilung](#worldwide-distribution-channel)bietet. Spieler auf der ganzen Welt können Ihr Spiel online zu dem von Ihnen [festgelegten Preis](#set-a-price-for-your-game) kaufen.
* Verwenden Sie APIs im Windows SDK zum Erstellen von [In-Game-Käufen](#in-game-purchases). Spieler können In-Game-Käufe tätigen oder ergänzende Inhalte wie zusätzliche Ausstattung, Designs, Karten oder Spiellevels kaufen.
* Verwenden Sie APIs im [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) zum Anzeigen von anzeigen aus Ad-Netzwerken. Sie können [Anzeigen in Ihrem Spiel anzeigen](#display-ads-in-your-game) und Spielern die Option anbieten, Videoanzeigen im Austausch für In-Game-Belohnungen anzusehen.
* [Maximieren Sie das Potenzial Ihres Spiels durch Werbekampagnen](#maximize-your-games-potential-through-ad-campaigns). Bewerben Sie Ihr Spiel mithilfe von kostenpflichtigen Anzeigenkampagnen, kostenlosen Community-Anzeigenkampagnen oder kostenloser Eigenwerbung, um die Benutzeranzahl zu steigern.

## <a name="worldwide-distribution-channel"></a>Weltweiter Vertriebskanal

Der Microsoft Store kann das Spiel in mehr als 200 Ländern und Regionen weltweit zum Download bereitstellen, mit Unterstützung für die Abrechnung über verschiedene Zahlungsarten, einschließlich Visa, MasterCard und PayPal. Eine vollständige Liste der Länder und Regionen finden Sie unter [Definieren der Markt Auswahl](../publish/define-market-selection.md).

## <a name="set-a-price-for-your-game"></a>Festlegen eines Preises für Ihr Spiel

Im Store veröffentlichte UWP-Spiele können _kostenpflichtig_ oder _kostenlos_ sein. Bei einem kostenpflichtigen Spiel können Sie fordern, dass Spieler vorab einen von Ihnen festgelegten Preis für Ihr Spiel zahlen. Kostenlose Spiele hingegen können von Benutzern heruntergeladen und gespielt werden, ohne dass sie dafür bezahlen müssen.

Hier sind einige wichtige Konzepte bezüglich der Preise für Ihr Spiel im Store aufgeführt.

### <a name="base-price"></a>Grundpreis

Der Grundpreis für das Spiel bestimmt, ob Ihr Spiel als _bezahlt_ oder _kostenlos_ eingestuft wird. Sie können [Partner Center](https://partner.microsoft.com/dashboard) verwenden, um den Basispreis auf der Grundlage von Land und Region zu konfigurieren.
Beim Festlegen des Preises müssen unter Umständen [Steuerpflichten beim Verkauf in anderen Ländern](/partner-center/tax-details-marketplace) und [Kostenüberlegungen für bestimmte Märkte](../publish/define-market-selection.md) in Betracht gezogen werden. Sie können auch [angepasste Preise für spezifische Märkte](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) festlegen.

### <a name="sale-price"></a>Angebotspreis

Eine Werbemöglichkeit für Ihr Spiel besteht beispielsweise in der Senkung des Preises für einen bestimmten Zeitraum. Sie können als Angebotspreis auch __Kostenlos__ festlegen, damit Ihr Spiel kostenlos heruntergeladen werden kann.
Sie können Angebotskampagnen im Voraus planen, indem Sie Start- und Enddatum des Angebots festlegen. Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](../publish/put-apps-and-add-ons-on-sale.md).

## <a name="in-game-purchases"></a>In-Game-Käufe

Bei In-Game-Käufen handelt es sich um Produkte, die in einem Spiel gekauft werden. Sie werden allgemein auch als _In-App-Käufe_ bezeichnet. In der Microsoft Store werden diese Produkte als _Add-ons_ bezeichnet. [Add-ons werden](../publish/add-on-submissions.md) über Partner Center veröffentlicht. Sie müssen die Add-Ons außerdem im Code Ihres Spiels aktivieren.

### <a name="types-of-add-ons"></a>Arten von Add-Ons

Sie können zwei Arten von Add-Ons im Store erstellen: _Gebrauchsgüter_ oder _Verbrauchsartikel_. Gebrauchsgüter sind Elemente, die bis zu ihrem Ablauf für einen angegebenen Zeitraum erhalten bleiben und nur einmal erworben werden können. Verbrauchsartikel sind Elemente, die gekauft und immer wieder verwendet werden können.

Legen Sie beim Erstellen von Verbrauchsmaterialien fest, wie Sie diese nachverfolgen möchten, und zwar unabhängig davon, &mdash; ob Sie vom _Entwickler verwaltet_ oder _verwaltet_ werden (dieses Feature ist ab Windows 10, Version 1607) verfügbar. Mit einem vom Entwickler verwalteten nutzbaren sind Sie dafür verantwortlich, den Saldo des Elements für den Spieler zu verfolgen. mit einem nutzbaren Speicher, der vom Speicher verwaltet wird, verfolgt das Microsoft Store den Saldo des Elements. Weitere Informationen finden Sie unter [Übersicht über Endverbraucher-Add-Ons](../monetize/enable-consumable-add-on-purchases.md).

### <a name="create-in-game-purchases"></a>Erstellen von In-Game-Käufen

Die aktuellen APIs für In-App-Käufe und Lizenzinformationen sind Teil des [Windows.Services.Store](/uwp/api/windows.services.store)-Namespace im Windows SDK (ab Windows 10, Version 1607). Bei der Entwicklung eines neuen Spiels für 1607 oder eine höhere Version wird empfohlen, den __Windows.Services.Store__-Namespace zu verwenden, da er die aktuellen Add-On-Typen unterstützt und eine bessere Leistung bietet.
Es ist auch für die Kompatibilität mit zukünftigen Typen von Produkten und Features konzipiert, die vom Partner Center und dem Store unterstützt werden. Verwenden Sie bei der Entwicklung für vorherige Windows 10-Versionen stattdessen den [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store)-Namespace.

Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](../monetize/in-app-purchases-and-trials.md).

#### <a name="simplified-purchase-example"></a>Vereinfachtes Kaufbeispiel

In diesem Abschnitt wird anhand eines vereinfachten Kaufbeispiels die Verwendung verschiedener Methodenaufrufe für die Implementierung des Kaufablaufs veranschaulicht.

|In-Game-Aktionen/-Aktivität                                                | Hintergrundaufgaben                  |
|--------------------------------------------------------------------------|----------------------------------------|
|Ein Spieler betritt ein Geschäft. Das Einkaufsmenü mit den verfügbaren Add-Ons und dem jeweiligen Kaufpreis wird angezeigt. |  Das Spiel [ruft die Produktinfos](../monetize/get-product-info-for-apps-and-add-ons.md) des Add-Ons ab, [bestimmt, ob die Add-Ons über die entsprechende Lizenz verfügen](../monetize/get-license-info-for-apps-and-add-ons.md) und zeigt im Einkaufsmenü die Add-Ons an, die für den Spieler zum Kauf zur Verfügung stehen.                           |
|Der Spieler klickt auf __Kaufen__ um ein Element zu kaufen.             |Die Aktion __Kaufen__ sendet eine Anforderung zum Kauf des Elements und beginnt den Zahlungsprozess, um es zu erwerben. Die Implementierung variiert je nach Elementtyp. Handelt es sich um ein [Gebrauchsgut oder ein einmalig erworbenes Element](../monetize/enable-in-app-purchases-of-apps-and-add-ons.md), kann der Kunde nur ein einzelnes Element besitzen, bis es abläuft. Ist das Element ein [Verbrauchsartikel](../monetize/enable-consumable-add-on-purchases.md), kann der Kunde mehrere davon besitzen. |

### <a name="test-in-game-purchases-during-game-development"></a>Testen von In-Game-Käufen während der Spieleentwicklung

Da ein Add-On im Zusammenhang mit einem Spiel erstellt werden muss, muss das Spiel im Store veröffentlicht worden und verfügbar sein. Die Schritte in diesem Abschnitt zeigen, wie Add-Ons erstellt werden, während sich das Spiel noch in der Entwicklung befindet.
(Wenn Ihr fertiges Spiel bereits im Store veröffentlicht wurde, können Sie die ersten drei Schritte überspringen und direkt mit [Erstellen eines Add-Ons im Store](#create-an-add-on-in-the-store) fortfahren.)

So erstellen Sie Add-Ons, während sich das Spiel noch in der Entwicklung befindet:
1. [Erstellen eines Pakets](#create-a-package)
2. [Veröffentlichen des Spiels als ausgeblendet](#publish-the-game-as-hidden)
3. [Verknüpfen Ihrer Spielelösung in Visual Studio mit dem Store](#associate-your-game-solution-with-the-store)
4. [Erstellen eines Add-Ons im Store](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>Erstellen eines Pakets

Damit ein Spiel veröffentlicht werden kann, muss es die Mindestanforderungen der Zertifizierung für Windows-Apps erfüllen. Sie können das [Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) (Teil des Windows 10 SDK) verwenden, um Tests für das Spiel durchzuführen und somit sicherzustellen, dass es für die Veröffentlichung im Store vorbereitet ist. Falls Sie das Windows 10 SDK, das das Zertifizierungskit für Windows-Apps enthält, noch nicht heruntergeladen haben, rufen Sie [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) auf.

So erstellen Sie ein Paket, das in den Store hochgeladen werden kann:

1. Öffnen Sie Ihre Spielelösung in Visual Studio.
2. Wechseln Sie in Visual Studio zum __Projekt__  >  __Speicher__  >  __App-Pakete erstellen...__
3. Wählen Sie für die Option möchten __Sie Pakete zum Hochladen in die Microsoft Store? erstellen?__ die Option __Ja__ aus.
4. Melden Sie sich bei Ihrem [Partner Center](https://partner.microsoft.com/dashboard) -Entwicklerkonto an. Oder [registrieren](https://developer.microsoft.com/store/register) Sie sich für ein Entwicklerkonto, falls Sie keins besitzen.
5. Wählen Sie eine App aus, für die das Uploadpaket erstellt werden soll. Falls Sie noch keine App-Übermittlung erstellt haben, geben Sie einen neuen App-Namen ein, um eine neue Übermittlung zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer App durch Reservieren eines Namens](../publish/create-your-app-by-reserving-a-name.md).
6. Nachdem das Paket erfolgreich erstellt wurde, klicken Sie auf __Zertifizierungskit für Windows-Apps starten__, um den Testprozess zu starten.
7. Beheben Sie mögliche Fehler, um ein Spielpaket zu erstellen.

#### <a name="publish-the-game-as-hidden"></a>Veröffentlichen des Spiels als ausgeblendet

1. Wechseln Sie zu [Partner Center](https://partner.microsoft.com/dashboard) , und melden Sie sich an.
2. Klicken Sie in der __Dashboardübersicht__ oder auf der Seite __Alle Apps__ auf die App, die Sie verwenden möchten. Falls Sie noch keine App-Übermittlung erstellt haben, klicken Sie auf __Neue App erstellen__, und reservieren Sie einen Namen.
3. Klicken Sie auf der Seite __App-Übersicht__ auf __Übermittlung starten__.
4. Konfigurieren Sie diese neue Übermittlung. Auf der Übermittlungsseite:
    * Klicken Sie auf __Preise und Verfügbarkeit__. Wählen Sie im Abschnitt __Sichtbarkeit__ die Option "__diese APP ausblenden und die Übernahme verhindern...__" aus, um sicherzustellen, dass nur Ihr Entwicklungsteam auf das Spiel zugreifen kann. Weitere Informationen finden Sie unter [Verteilung und Sichtbarkeit](../publish/set-app-pricing-and-availability.md).
    * Klicken Sie auf __Eigenschaften__. Wählen Sie im Abschnitt __Kategorie und Unterkategorie__ die Option __Spiele__ und anschließend eine geeignete Unterkategorie für Ihr Spiel aus.
    * Klicken Sie auf __Altersfreigaben__. Füllen Sie den Fragebogen ordnungsgemäß aus.
    * Klicken Sie auf __Pakete__. Laden Sie das zuvor erstellte Spielpaket hoch.
5. Befolgen Sie alle anderen Übermittlungsaufforderungen auf dem Dashboard, damit dieses Spiel veröffentlicht werden kann, dabei aber für die Öffentlichkeit ausgeblendet bleibt.
6. Klicken Sie auf __An Store übermitteln__.

Weitere Informationen finden Sie unter [App-Übermittlungen](../publish/app-submissions.md).

Nachdem das Spiel an den Store übermittelt wurde, beginnt der [App-Zertifizierungsprozess](../publish/the-app-certification-process.md). Dieser Prozess kann bis zu 16 Stunden dauern, und das Spiel wird erst nach Abschluss des Prozesses aufgeführt.

#### <a name="associate-your-game-solution-with-the-store"></a>Verknüpfen Ihrer Spielelösung mit dem Store

Bei in Visual Studio geöffneter Spielelösung:

1. Wechseln Sie zum __Projekt__  >  __Speicher__  >  __App-App mit dem Store verknüpfen...__
2. Melden Sie sich bei Ihrem Partner Center-Entwicklerkonto an, und wählen Sie den APP-Namen aus, dem diese Lösung zugeordnet werden soll
3. Doppelklicken Sie auf die Datei __Package.appxmanifest.xml__, und wechseln Sie zur Registerkarte __Verpacken__, um zu überprüfen, ob das Spiel richtig zugeordnet wurde.

Wenn Sie die Lösung einem veröffentlichten Spiel zugeordnet haben, das im Store aufgeführt ist, verfügt Ihre Lösung über eine aktive Lizenz und Sie sind dem Erstellen von Add-Ons für Ihr Spiel einen Schritt näher. Weitere Informationen finden Sie unter [Verpacken von Apps](../packaging/index.md).

#### <a name="create-an-add-on-in-the-store"></a>Erstellen eines Add-Ons im Store

Stellen Sie beim Erstellen von Add-Ons sicher, dass Sie sie der richtigen Spieleübermittlung zuordnen. Ausführliche Informationen zum Konfigurieren aller Informationen für ein Add-On finden Sie unter [Add-On-Übermittlungen](../publish/add-on-submissions.md).

1. Wechseln Sie zu [Partner Center](https://partner.microsoft.com/dashboard) , und melden Sie sich an.
2. Klicken Sie in der __Dashboardübersicht__ oder auf der Seite __Alle Apps__ auf die App, für die Sie das Add-On erstellen möchten.
3. Wählen Sie auf der Seite __App-Übersicht__ im Abschnitt __Add-Ons__ die Option __Neues Add-On erstellen__.
4. Wählen Sie den Produkttyp für das Add-On aus: __von Entwicklern verwaltete Verbrauchsartikel__, __vom Store verwalteter Verbrauchsartikel__ oder __Gebrauchsgut__.
5. Geben Sie eine eindeutige Produkt-ID ein, die beim Integrieren dieses Add-Ons in den Spielcode als Zeichenfolgenvariable verwendet wird. Diese ID ist für Kunden nicht sichtbar. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für Apps](../publish/set-your-add-on-product-id.md).

Weitere Konfigurationen für Add-Ons:
* [Eigenschaften](../publish/enter-add-on-properties.md)
* [Preise und Verfügbarkeit](../publish/set-add-on-pricing-and-availability.md)
* [Store-Auflistung](../publish/create-add-on-store-listings.md)

Wenn Ihr Spiel viele Add-ons enthält, können Sie Sie mithilfe der Microsoft Store Übermittlungs- __API__ Programm gesteuert erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store-Diensten](../monetize/create-and-manage-submissions-using-windows-store-services.md).

## <a name="display-ads-in-your-game"></a>Anzeigen von Werbung in Ihrem Spiel

Mithilfe der Bibliotheken und Tools im Microsoft Advertising SDK können Sie einen Dienst in Ihrem Spiel einrichten, um Werbeeinblendungen von einem Ad-Netzwerk zu erhalten. Spielern werden Liveanzeigen angezeigt, und Sie erhalten Geld von den Inserenten, wenn Ihre Spieler die dargestellten Anzeigen ansehen oder mit ihnen interagieren.
Weitere Informationen finden Sie unter Anzeigen von [anzeigen in Ihrer APP](../monetize/display-ads-in-your-app.md).

### <a name="ad-formats"></a>Anzeigenformate

Mit dem Microsoft Advertising SDK können verschiedene Arten von Werbeeinblendungen angezeigt werden:

* Banner Werbe &mdash; Einblendungen, die einen Teil Ihres Gaming-Bildschirms einnehmen und in der Regel in einem Spiel abgelegt werden.
* Interstitial-Videowerbung für &mdash; voll Bildschirme, die sehr effektiv sein können, wenn Sie zwischen Ebenen verwendet werden. Werden Sie richtig implementiert, können Sie weniger störend als Banneranzeigen wirken.
* Native ADS &mdash; -komponentenbasierte Werbeanzeigen, bei denen die einzelnen Teile der Werbeeinblendungen (z. b. Titel, Bild, Beschreibung und Aktions Text) an Ihre APP als einzelnes Element übermittelt werden, das Sie in Ihre APP integrieren können.

### <a name="which-ads-are-displayed"></a>Welche Anzeigen werden angezeigt?

Standardmäßig zeigt Ihre APP Anzeigen von Werbeeinblendungen aus dem Microsoft-Netzwerk an. Um Ihren AD-Umsatz zu maximieren, können Sie AD-Vermittlung für Ihre Ad-Einheit aktivieren, um Werbung von zusätzlichen kostenpflichtigen Ad-Netzwerken anzuzeigen. Weitere Informationen zu aktuellen Angeboten finden Sie in unserem Leitfaden zur [AD-Vermittlung](../publish/in-app-ads.md#mediation) .

### <a name="which-markets-allow-ads-to-be-displayed"></a>Auf welchen Märkten ist die Schaltung von Anzeigen erlaubt?

Eine vollständige Liste der Länder und Regionen, die ADS unterstützen, finden Sie [unter Unterstützte Märkte für Ad-Netzwerke](../publish/in-app-ads.md#network-markets).

### <a name="apis-for-displaying-ads"></a>APIs zum Einblenden von Anzeigen

Die Klassen [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)und [nativead](/uwp/api/microsoft.advertising.winrt.ui.nativead) im Microsoft Advertising SDK werden zum Anzeigen von Werbeeinblendungen verwendet.

Laden Sie zunächst das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer höheren Version herunter, und installieren Sie es. Weitere Informationen finden Sie unter [Installieren des Microsoft Advertising SDK](../monetize/install-the-microsoft-advertising-libraries.md).

#### <a name="implementation-guides"></a>Implementierungshandbücher

Diese exemplarischen Vorgehensweisen zeigen, wie Sie ADS mithilfe von __adcontrol__, __interstitialad__ und __nativead__ implementieren:

* [Erstellen von Banner Anzeigen in XAML und .net](../monetize/adcontrol-in-xaml-and--net.md)
* [Erstellen von Banner Anzeigen in HTML5 und JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md)
* [Erstellen von Interstitial-anzeigen](../monetize/interstitial-ads.md)
* [Erstellen von nativen anzeigen](../monetize/native-ads.md)

Während der Entwicklung können Sie die [Test-Ad-Einheiten Werte](../monetize/set-up-ad-units-in-your-app.md) verwenden, um zu sehen, wie die ADS gerendert werden. Diese Test-Ad-Einheiten Werte werden auch in den oben beschriebenen exemplarischen Vorgehensweisen verwendet.

Hier sind einige bewährte Methoden aufgeführt, die Sie beim Entwurfs- und Implementierungsprozess unterstützen.

* [Bewährte Methoden für Bannerwerbung](../monetize/ui-and-user-experience-guidelines.md)
* [Bewährte Methoden für Interstitialanzeigen](../monetize/ui-and-user-experience-guidelines.md)

Wenn bei der Entwicklung Probleme auftreten, beispielsweise wenn Anzeigen nicht eingeblendet werden, die Blackbox blinkt und wieder verschwindet oder Anzeigen nicht aktualisiert werden, finden Sie Lösungen in den [Handbüchern zur Problembehandlung](../monetize/known-issues-for-the-advertising-libraries.md).

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>Vorbereiten auf die Veröffentlichung durch Ersetzen der Testwerte für Anzeigeneinheiten

Wenn Sie bereit sind, mit Livetests fortzufahren oder Anzeigen in veröffentlichten Spielen zu empfangen, müssen Sie die Testwerte der Anzeigeneinheiten auf die tatsächlichen, für Ihr Spiel angegebenen Werte aktualisieren. Informationen zum Erstellen von Anzeigeneinheiten für Ihr Spiel finden Sie unter [Einrichten von Anzeigeneinheiten in der App](../monetize/set-up-ad-units-in-your-app.md).

### <a name="other-ad-networks"></a>Weitere Anzeigennetzwerke

Dabei handelt es sich um andere Ad-Netzwerke, die sdert zum Bereitstellen von Werbeeinblendungen und spielen bereitstellen.

#### <a name="vungle"></a>Vungle

Das Vungle SDK für Windows bietet Videoanzeigen in Apps und Spielen. Das SDK können Sie unter [Vungle SDK](https://publisher.vungle.com/sdk/) herunterladen.

#### <a name="smaato"></a>Smaato

Smaato ermöglicht die Integration von Banneranzeigen in UWP-Apps und -Spiele. Laden Sie das [SDK](https://www.smaato.com/resources/sdks/) herunter. Weitere Informationen finden Sie in der [Dokumentation](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### <a name="adduplex"></a>AdDuplex

Mit AdDuplex können Sie Banner- oder Interstitialanzeigen in Ihrem Spiel implementieren.

Weitere Informationen zum Integrieren von AdDuplex direkt in ein Windows 10-XAML-Projekt finden Sie auf der AdDuplex-Website:
* Banneranzeigen: [Windows 10 SDK für XAML](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage)
* Interstitielle Anzeigen: [Windows 10 XAML AdDuplex Interstitial Ad Installation and Usage](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage) (Implementierung und Verwendung von AdDuplex-Interstitialanzeigen in Windows 10-XAML-Projekten)

Informationen zum Integrieren des AdDuplex SDK in Windows 10-UWP-Spiele, die mit Unity erstellt wurden, finden Sie unter [Windows 10 SDK for Unity apps installation and usage](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage) (Windows 10 SDK für Unity-Apps: Installation und Nutzung).

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>Maximieren des Potenzials Ihres Spiels über Anzeigenkampagnen

Gehen Sie noch einen Schritt weiter, und bewerben Sie Ihr Spiel mithilfe von Anzeigen. Wenn Sie für Ihr Spiel eine [Anzeigenkampagne erstellen](../monetize/index.md), zeigen anderen Apps und Spiele Werbung für Ihr Spiel an.

Wählen Sie zwischen verschiedenen Kampagnenarten, mit denen Sie die Zahl von Spielern erhöhen können.

|Kampagnentyp             | Anzeigen für Ihr Spiel werden in folgenden Apps angezeigt:                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Kostenpflichtig                      |Apps, die dem Gerät oder der Kategorie Ihres Spiels entsprechen                                                                                                                                                   |
|Kostenlose Community-Anzeigen            |Apps, die von anderen Entwicklern veröffentlicht werden, die ebenfalls Community-Anzeigenkampagnen nutzen. Weitere Informationen finden Sie unter [Informationen zu Community-Anzeigen](../monetize/index.md).|
|Kostenlose Eigenwerbung                |Nur in Apps, die Sie veröffentlicht haben. Weitere Informationen finden Sie unter [Über Eigenwerbung](../monetize/index.md).                                                            |

## <a name="related-links"></a>Verwandte Links

* [Bezahlung](/partner-center/marketplace-get-paid)
* [Kontotypen, Standorte und Gebühren](../publish/account-types-locations-and-fees.md)
* [Analyse](../publish/analytics.md)
* [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md)
* [Implementieren einer Testversion der App](../monetize/implement-a-trial-version-of-your-app.md)
* [Ausführen von Experimenten mit A/B-Tests](../monetize/run-app-experiments-with-a-b-testing.md)