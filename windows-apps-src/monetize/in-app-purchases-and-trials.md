---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Lernen Sie die grundlegenden Aufgaben und Konzepte kennen, die zum Implementieren und Testen von in-App-Käufen und-Testfunktionen in UWP-apps erforderlich sind
title: In-App-Käufe und Testversionen
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, in-App-Käufe, IAPS, Add-ons, Test, nutzbar, langlebig, Abonnement
ms.localizationpriority: medium
ms.openlocfilehash: fea4b222d604c2da1b97d692658abd8561ab1501
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942802"
---
# <a name="in-app-purchases-and-trials"></a>In-App-Käufe und Testversionen

Das Windows SDK enthält APIs, mit denen Sie die folgenden Features implementieren können, um mit Ihrer UWP-App (Universelle Windows-Plattform) mehr Geld zu verdienen:

* **In-App-Käufe** &nbsp; &nbsp; Unabhängig davon, ob Ihre App kostenlos ist oder nicht, können Sie Inhalte oder neue App-Funktionen (z. b. das Entsperren der nächsten Ebene eines Spiels) direkt in der APP verkaufen.

* **Test Funktionalität** &nbsp; &nbsp; Wenn Sie [Ihre APP als kostenlose Testversion in Partner Center konfigurieren](../publish/set-app-pricing-and-availability.md#free-trial), können Sie Ihre Kunden dazu verleiten, die Vollversion Ihrer APP zu erwerben, indem Sie einige Features während des Testzeitraums ausschließen oder einschränken. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

Dieser Artikel enthält eine Übersicht darüber, wie In-App-Käufe und Testversionen in UWP-Apps funktionieren.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Auswahl des zu verwendenden Namespace

Je nachdem, für welche Version von Windows 10 Ihre App ausgelegt ist, können Sie mithilfe zweier verschiedener Namespaces Ihren UWP-Apps In-App-Käufe und Testversionen hinzufügen. Obwohl die APIs in diesen Namespaces den gleichen Ziele dienen, sind sie unterschiedlich gestaltet, und der Code ist zwischen den beiden APIs nicht kompatibel.

* **[Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store)** &nbsp; &nbsp; Ab Windows 10, Version 1607, können Apps die API in diesem Namespace verwenden, um in-App-Käufe und-Tests zu implementieren. Es wird empfohlen, dass Sie die Elemente in diesem Namespace verwenden, wenn Ihr App-Projekt auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version in Visual Studio. Dieser Namespace unterstützt die neuesten Add-on-Typen, z. b. von Filialen verwaltete, nutzbare Add-ons, und ist für die Kompatibilität mit zukünftigen Typen von Produkten und Features konzipiert, die von Partner Center und dem Store unterstützt werden. Weitere Informationen zu diesem Namespace finden Sie in diesem Artikel im Abschnitt [In-App-Käufe und Testversionen mit dem Windows.Services.Store-Namespace](#api_intro).

* **[Windows. applicationmodel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store)** &nbsp; &nbsp; Alle Versionen von Windows 10 unterstützen auch eine ältere API für in-App-Käufe und-Tests in diesem Namespace. Weitere Informationen zum **Windows. applicationmodel. Store** -Namespace finden [Sie unter in-App-Käufe und-Tests mit dem Windows. applicationmodel. Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> Der **Windows. applicationmodel. Store** -Namespace wird nicht mehr mit neuen Features aktualisiert, und es wird empfohlen, stattdessen den **Windows. Services. Store** -Namespace zu verwenden, sofern dies für Ihre APP möglich ist. Der **Windows. applicationmodel. Store** -Namespace wird nicht in Windows-Desktop Anwendungen unterstützt, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) verwenden, oder in Apps oder spielen, die eine Entwicklungs Sandbox im Partner Center verwenden (z. b. bei jedem Spiel, das in Xbox Live integriert ist).

<span id="concepts" />

## <a name="basic-concepts"></a>Grundlegende Konzepte

Jedes im Store angebotene Element wird im Allgemeinen als *Produkt* bezeichnet. Die meisten Entwickler arbeiten nur mit den folgenden Produkttypen: *apps* und *Add-ons*.

Ein Add-on ist ein Produkt oder Feature, das Sie Ihren Kunden im Kontext ihrer App zur Verfügung stellen: z. b. in einer APP oder einem Spiel zu verwendende Währung, neue Zuordnungen oder Waffen für ein Spiel, die Möglichkeit, Ihre APP ohne Werbung zu verwenden, oder digitale Inhalte wie Musik oder Videos für apps, die diese Art von Inhalten anbieten können. Jede App und jedes Add-On verfügt über eine zugehörige Lizenz, die angibt, ob der Benutzer zur Verwendung dieser App oder des Add-Ons berechtigt ist. Wenn der Benutzer berechtigt ist, die App bzw. das Add-On als Testversion zu verwenden, enthält die Lizenz auch zusätzliche Informationen zur Testversion.

Um Kunden in Ihrer APP ein Add-on zur Verfügung zu stellen, müssen Sie [das Add-on für Ihre APP im Partner Center definieren](../publish/add-on-submissions.md) , damit der Store dies kennt. Anschließend kann das Add-On mithilfe der APIs im **Windows.Services.Store**-Namespace oder im **Windows.ApplicationModel.Store**-Namespace den Kunden zum Erwerb als In-App-Kauf angeboten werden.

Für UWP-Apps können die folgenden Arten von Add-Ons angeboten werden.

| Add-On-Typ |  BESCHREIBUNG  |
|---------|-------------------|
| Durable  |  Ein Add-on, das für die Lebensdauer beibehalten wird, die Sie [im Partner Center angeben](../publish/enter-iap-properties.md). <p/><p/>Standardmäßig laufen Gebrauchsgut-Add-Ons nie ab, in diesem Fall können sie nur einmal gekauft werden. Wenn Sie eine bestimmte Dauer für das Add-On angeben, kann der Benutzer das Add-On erneut kaufen, nachdem es abgelaufen ist. |
| Von Entwicklern verwaltetes Endverbraucher-Add-On  |  Ein Add-on, das gekauft, verwendet und dann erneut erworben werden kann, nachdem es verbraucht wurde. Sie sind dafür verantwortlich, das Gleichgewicht der Elemente des Benutzers zu verfolgen, die das Add-on darstellt.<p/><p/>Wenn der Benutzer Elemente verwendet, die dem Add-on zugeordnet sind, sind Sie dafür verantwortlich, den Saldo des Benutzers beizubehalten und den Kauf des Add-ons als erfüllt für den Speicher zu melden, nachdem der Benutzer alle Elemente verbraucht hat. Der Benutzer kann das Add-On erst dann erneut kaufen, nachdem Ihre App den vorherigen Kauf des Add-Ons als erfüllt gemeldet hat. <p/><p/>Wenn beispielsweise das Add-On 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, muss die App oder der Dienst den neuen Restbetrag von 90 Münzen für den Benutzer verwalten. Nachdem der Benutzer alle 100 Münzen genutzt hat, muss die App das Add-On als erfüllt melden, und danach kann der Benutzer das Add-On für 100 Münzen erneut kaufen.    |
| Vom Store verwaltetes Endverbraucher-Add-On  |  Ein Add-on, das jederzeit gekauft, verwendet und gekauft werden kann. Der Speicher verfolgt das Gleichgewicht der Elemente des Benutzers, die das Add-on darstellt.<p/><p/>Wenn der Benutzer Elemente verwendet, die dem Add-on zugeordnet sind, sind Sie dafür verantwortlich, diese Elemente als erfüllt an den Store zu melden, und der Speicher aktualisiert den Saldo des Benutzers. Der Benutzer kann das Add-on so oft wie gewünscht erwerben (es ist nicht erforderlich, die Elemente zuerst zu verwenden). Ihre App kann das aktuelle Guthaben für den Benutzer jederzeit abfragen. <p/><p/> Wenn das Add-on beispielsweise eine anfängliche Menge von 100-Münzen in einem Spiel darstellt und der Benutzer 50-Münzen beansprucht, meldet Ihre APP an den Store, dass 50 Einheiten des Add-ins erfüllt wurden, und der Speicher aktualisiert das verbleibende Guthaben. Wenn der Benutzer dann das Add-on erneut kauft, um 100 weitere Münzen zu erhalten, verfügen Sie nun über 150 Münzen gesamt. <p/><p/>**Note** &nbsp; Hinweis &nbsp; Zum Verwenden von durch Filialen verwalteten Verbrauchsmaterialien muss Ihre APP auf **Windows 10 Anniversary Edition (10,0; Build 14393)** oder ein späteres Release in Visual Studio, und es muss den **Windows. Services. Store** -Namespace anstelle des **Windows. applicationmodel. Store** -Namespace verwenden.  |
| Subscription | Ein dauerhaftes Add-on, bei dem der Kunde weiterhin zu wiederkehrenden Intervallen in Rechnung gestellt wird, um das Add-on weiterhin zu verwenden. Der Kunde kann das Abonnement jederzeit kündigen, um weitere Kosten zu vermeiden. <p/><p/>**Note** &nbsp; Hinweis &nbsp; Um Abonnement-Add-ons verwenden zu können, muss Ihre APP auf **Windows 10 Anniversary Edition (10,0; Build 14393)** oder ein späteres Release in Visual Studio, und es muss den **Windows. Services. Store** -Namespace anstelle des **Windows. applicationmodel. Store** -Namespace verwenden.  |

<span />

> [!NOTE]
> Andere Typen von Add-ons, wie z. b. permanente Add-ons mit Paketen (auch als herunterladbarer Inhalt oder DLC bezeichnet), sind nur für eine eingeschränkte Gruppe von Entwicklern verfügbar und werden in dieser Dokumentation nicht behandelt.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>In-App-Käufe und Testversionen mit dem Windows.Services.Store-Namespace

Dieser Abschnitt bietet eine Übersicht über wichtige Aufgaben und Konzepte für den [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) -Namespace. Dieser Namespace ist nur für apps verfügbar, die auf **Windows 10 Anniversary Edition abzielen (10,0; Build 14393)** oder eine spätere Version in Visual Studio (entspricht Windows 10, Version 1607). Es wird empfohlen, dass apps den **Windows. Services. Store** -Namespace anstelle des [Windows. applicationmodel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) -Namespace verwenden, wenn dies möglich ist. Weitere Informationen zum **Windows. applicationmodel. Store** -Namespace finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**In diesem Abschnitt**

* [Video](#video)
* [Erste Schritte mit der StoreContext-Klasse](#get-started-storecontext)
* [Implementieren von In-App-Käufen](#implement-iap)
* [Implementieren der Testfunktionen](#implement-trial)
* [Testen der Implementierung für den In-App-Kauf oder die Testversion](#testing)
* [Belege für In-App-Käufe](#receipts)
* [Verwenden der StoreContext-Klasse mit der Desktop-Brücke](#desktop)
* [Produkte, SKUs und Verfügbarkeiten](#products-skus)
* [Store-IDs](#store-ids)

<span id="video" />

### <a name="video"></a>Video

Im folgenden Video sehen Sie eine Übersicht über die Implementierung von in-App-Käufen in ihrer App mithilfe des [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) -Namespace.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Erste Schritte mit der StoreContext-Klasse

Der Haupteinstiegspunkt in den **Windows.Services.Store**-Namespace ist die [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse. Diese Klasse stellt Methoden bereit, mit denen Sie Informationen für die aktuelle App und die verfügbaren Add-Ons abrufen, eine App oder ein Add-On für den aktuellen Benutzer kaufen, Lizenzinformationen für die aktuelle App oder die Add-Ons abrufen und weitere Aufgaben durchführen können. Gehen Sie zum Abrufen eines [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Objekts wie folgt vor:

* Verwenden Sie in einer Einzelbenutzer-app (d. h. eine APP, die nur im Kontext des Benutzers ausgeführt wird, der die APP gestartet hat) die statische [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) -Methode, um ein **storecontext** -Objekt zu erhalten, das Sie verwenden können, um auf Microsoft Store bezogenen Daten des Benutzers zuzugreifen. Die meisten Apps für die universelle Windows-Plattform (UWP) sind Einzelbenutzer-Apps.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Verwenden Sie in einer [mehr Benutzer-App](../xbox-apps/multi-user-applications.md)die statische [getforuser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) -Methode, um ein **storecontext** -Objekt zu erhalten, das Sie verwenden können, um auf Microsoft Store bezogenen Daten für einen bestimmten Benutzer zuzugreifen, der bei Verwendung der APP mit Ihrem Microsoft-Konto angemeldet ist. Im folgenden Beispiel wird ein **StoreContext**-Objekt für den ersten verfügbaren Benutzer abgerufen.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Windows-Desktop Anwendungen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) verwenden, müssen zusätzliche Schritte ausführen, um das [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) -Objekt zu konfigurieren, bevor dieses Objekt verwendet werden kann. Weitere Informationen finden Sie in [diesem Abschnitt](#desktop).

Nachdem Sie über ein [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Objekt verfügen, können Sie beginnen, Methoden dieses Objekts aufzurufen, um Store-Produktinformationen und Lizenzinformationen für die aktuelle App und deren Add-Ons abzurufen, eine App oder ein Add-On für den aktuellen Benutzer zu erwerben und weitere Aufgaben auszuführen. Weitere Informationen zu den allgemeinen Aufgaben, die Sie mit diesem Objekt ausführen können, finden Sie in den folgenden Artikeln:

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Aktivieren von Abonnement-Add-Ons für die App](enable-subscription-add-ons-for-your-app.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)

Eine Beispiel-App, die die Verwendung von **StoreContext** und anderen Typen im **Windows.Services.Store**-Namespace aufzeigt, finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implementieren von In-App-Käufen

So bieten Sie den Kunden mit dem **Windows.Services.Store**-Namespace in Ihrer App In-App-Käufe an

1. Wenn Ihre APP Add-ons bietet, die Kunden erwerben können, [Erstellen Sie Add-on-Übermittlungen für Ihre APP im Partner Center ](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Schreiben Sie Code für Ihre App, um [Produktinformationen über Ihre App oder ein von der App angebotenes Add-On abzurufen](get-product-info-for-apps-and-add-ons.md). [Ermitteln Sie anschließend, ob die Lizenz aktiv ist](get-license-info-for-apps-and-add-ons.md) (d. h., ob der Benutzer über eine Lizenz für die App bzw. das Add-On verfügt). Wenn die Lizenz nicht aktiv ist, zeigen Sie eine Benutzeroberfläche an, auf der die App bzw. das Add-On dem Benutzer als In-App-Kauf angeboten wird.

3. Wenn sich der Benutzer für den Kauf Ihrer App oder Ihres Add-Ons entscheidet, verwenden Sie für den Erwerb des Produkts die geeignete Methode:

    * Wenn der Benutzer Ihre App oder ein dauerhaftes Add-On erwirbt, befolgen Sie die Anweisungen in [Unterstützen von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Wenn der Benutzer ein konsumierbares Add-On erwirbt, befolgen Sie die Anweisungen in [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md).
    * Wenn der Benutzer ein Abonnement-Add-on kauft, befolgen Sie die Anweisungen unter [Aktivieren von Abonnement-Add-ons für Ihre APP](enable-subscription-add-ons-for-your-app.md).

4. Testen der Implementierung anhand der [Hinweise für Tests](#testing) in diesem Artikel.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implementieren der Testfunktionen

So schränken Sie mit dem **Windows.Services.Store**-Namespace Features in einer Testversion Ihrer App ein oder schließen diese aus

1. [Konfigurieren Sie Ihre APP als kostenlose Testversion in Partner Center](../publish/set-app-pricing-and-availability.md#free-trial).

2. Schreiben Sie Code für Ihre App, um [Produktinformationen über Ihre App oder ein von der App angebotenes Add-On abzurufen](get-product-info-for-apps-and-add-ons.md), und [ermitteln Sie anschließend, ob es sich bei der der App zugeordneten Lizenz um eine Testlizenz handelt](get-license-info-for-apps-and-add-ons.md).

3. Handelt es sich um eine Testversion der App, schließen Sie bestimmte Features aus oder schränken Sie sie ein, und aktivieren Sie diese, wenn der Benutzer eine Lizenz für die Vollversion erwirbt. Weitere Informationen und ein Codebeispiel finden Sie unter [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md).

4. Testen der Implementierung anhand der [Hinweise für Tests](#testing) in diesem Artikel.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Testen der Implementierung für den In-App-Kauf oder die Testversion

Wenn Ihre APP APIs im **Windows. Services. Store** -Namespace verwendet, um in-App-Kauf-oder Testfunktionen zu implementieren, müssen Sie Ihre APP im Store veröffentlichen und die APP auf Ihr Entwicklungs Gerät herunterladen, um die Lizenz für Tests zu verwenden. Gehen Sie folgendermaßen vor, um den Code zu testen:

1. Wenn Ihre APP noch nicht veröffentlicht wurde und im Store verfügbar ist, stellen Sie sicher, dass Ihre APP die Mindestanforderungen an das [Windows-App-zertifizierungskit](https://developer.microsoft.com/windows/develop/app-certification-kit) erfüllt, [Ihre APP](https://docs.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center übermittelt und sicherzustellen, dass Ihre APP den Zertifizierungsprozess übergibt. Sie können [Ihre APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) , während Sie Sie testen. Beachten Sie die ordnungsgemäße Konfiguration von [Paket Flügen](../publish/package-flights.md). Falsch konfigurierte Paket Flüge können möglicherweise nicht heruntergeladen werden.

2. Stellen Sie anschließend sicher, dass die folgenden Schritte durchgeführt wurden:

    * Schreiben Sie Code für Ihre App, die die [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse sowie weitere verwandte Typen im **Windows.Services.Store**-Namespace verwendet, um [In-App-Käufe](#implement-iap) oder [Testfunktionen](#implement-trial) zu implementieren.
    * Wenn Ihre APP ein Add-on anbietet, das Kunden erwerben können, [Erstellen Sie im Partner Center eine Add-on-Übermittlung für Ihre APP](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Wenn Sie einige Features in einer Testversion Ihrer APP ausschließen oder einschränken möchten, konfigurieren Sie [Ihre APP als kostenlose Testversion in Partner Center](../publish/set-app-pricing-and-availability.md#free-trial).

3. Öffnen Sie Ihr Projekt in Visual Studio, klicken Sie auf das **Menü „Projekt“**, zeigen Sie auf **Store**, und klicken Sie dann auf **App mit Store verknüpfen**. Vervollständigen Sie die Anweisungen im Assistenten, um das App-Projekt mit der app in Ihrem Partner Center-Konto zu verknüpfen, das Sie für den Test verwenden möchten.
    > [!NOTE]
    > Wenn Sie Ihr Projekt nicht einer APP im Store zuordnen, legen die [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) -Methoden die **ExtendedError** -Eigenschaft ihrer Rückgabewerte auf den Fehler Codewert 0x803f6107 fest. Dieser Wert gibt an, dass die App im Store nicht bekannt ist.
4. Falls noch nicht geschehen, installieren Sie die App aus dem im vorherigen Schritt angegebenen Store, führen Sie die App einmal aus, und schließen Sie dann diese App. Damit wird sichergestellt, dass auf dem Gerät für die Entwicklung eine gültige Lizenz für die App installiert wird.

5. Starten Sie in Visual Studio die Ausführung oder das Debuggen des Projekts. Der Code sollte App- und Add-On-Daten aus der Store-App abrufen, die Sie mit dem lokalen Projekt verknüpft haben. Wenn Sie zur Neuinstallation der App aufgefordert werden, folgen Sie den Anweisungen, und führen Sie dann das Projekt aus oder debuggen Sie es.
    > [!NOTE]
    > Nachdem Sie diese Schritte ausgeführt haben, können Sie den Code Ihrer APP weiterhin aktualisieren und dann Ihr aktualisiertes Projekt auf dem Entwicklungs Computer Debuggen, ohne neue APP-Pakete an den Store zu senden. Sie müssen die Store-Version Ihrer App nur einmal auf den Entwicklungscomputer herunterladen, um die lokale Lizenz zu erhalten, die zum Testen verwendet wird. Sie übermitteln neue App-Pakete erst an den Store, nachdem Sie die Tests abgeschlossen haben und Ihren Kunden App-Features zur Verfügung stellen möchten, die sich auf In-App-Käufe oder Testversionen beziehen.

Wenn Ihre APP den **Windows. applicationmodel. Store** -Namespace verwendet, können Sie die [currentappsimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) -Klasse in Ihrer APP verwenden, um während des Tests Lizenzinformationen zu simulieren, bevor Sie Ihre APP an den Store übermitteln. Weitere Informationen finden Sie unter [Get Started with the currentapp and currentappsimulator Classes](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> Der **Windows.Services.Store**-Namespace stellt keine Klasse bereit, mit der Sie während der Tests Lizenzinformationen simulieren können. Wenn Sie den **Windows. Services. Store** -Namespace für die Implementierung von in-App-Käufen oder-Testversionen verwenden, müssen Sie die APP im Store veröffentlichen und die APP auf Ihr Entwicklungs Gerät herunterladen, um Ihre Lizenz für Tests zu verwenden, wie oben beschrieben.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Belege für In-App-Käufe

Der **Windows.Services.Store**-Namespace verfügt über keine API, mit der Sie im Code der App einen Transaktionsbeleg für erfolgreiche Käufe erhalten können. Dies unterscheidet sich von Apps, die den **Windows.ApplicationModel.Store**-Namespace verwenden, der [zum Abrufen von Transaktionsbelegen eine clientseitige API verwenden kann](use-receipts-to-verify-product-purchases.md).

Wenn Sie in-App-Käufe mit dem **Windows. Services. Store** -Namespace implementieren und überprüfen möchten, ob ein bestimmter Kunde eine APP oder ein Add-on gekauft hat, können Sie die [Methode "Query for Products](query-for-products.md) " in der [Microsoft Store Collection-Rest-API](view-and-grant-products-from-a-service.md)verwenden. Die für diese Methode zurückgegebenen Daten bestätigen, ob der jeweilige Kunde über eine Berechtigung für ein bestimmtes Produkt verfügt. Zudem werden Daten für die Transaktion bereitgestellt, im Rahmen derer der Benutzer das Produkt erworben hat. Die Microsoft Store Sammlungs-API verwendet Azure AD Authentifizierung, um diese Informationen abzurufen.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Verwenden der StoreContext-Klasse mit der Desktop-Brücke

Für Desktopanwendungen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwenden, kann zum Implementieren von In-App-Käufen und Testversionen die [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse verwendet werden. Wenn Sie jedoch über eine Win32-Desktopanwendung oder eine Desktopanwendung mit Fenster-Handle (HWND) verfügen, die dem Renderingframework zugeordnet ist (z. B. eine WPF-Anwendung), muss für Ihre Anwendung das **StoreContext**-Objekt konfiguriert werden, um anzugeben, welches Anwendungsfenster das Besitzerfenster für die vom Objekt angezeigten modalen Dialogfelder ist.

Viele **StoreContext**-Member (und andere Member verwandter Typen, auf die über das **StoreContext**-Objekt zugegriffen wird) zeigen den Benutzern für Store-bezogene Vorgänge wie z. B. den Kauf eines Produkts ein modales Dialogfeld an. Wenn für eine Desktopanwendung das **StoreContext**-Objekt nicht konfiguriert wurde, um das Besitzerfenster für modale Dialogfelder anzugeben, gibt das Objekt ungenaue Daten oder Fehler zurück.

Führen Sie die folgenden Schritte durch, um ein **StoreContext**-Objekt in einer Desktopanwendung zu konfigurieren, die die Desktop-Brücke verwendet.

1. Führen Sie einen der folgenden Schritte aus, um Ihrer App den Zugriff auf die [IInitializeWithWindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow)-Schnittstelle zu ermöglichen:

    * Wenn die Anwendung in einer verwalteten Sprache wie z. B. C# oder Visual Basic geschrieben wurde, deklarieren Sie die **IInitializeWithWindow**-Schnittstelle im App-Code wie im folgenden C#-Beispiel mit dem [ComImport](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)-Attribut. In diesem Beispiel wird davon ausgegangen, dass Ihre Codedatei über eine **using**-Anweisung für den **System.Runtime.InteropServices**-Namespace verfügt.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * Wenn Ihre Anwendung in C++ geschrieben ist, fügen Sie im Code einen Verweis auf die Headerdatei „shobjidl.h“ hinzu. Diese Headerdatei enthält die Deklaration der **IInitializeWithWindow**-Schnittstelle.

2. Rufen Sie wie weiter oben in diesem Artikel beschrieben mit der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault)-Methode (oder [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser), falls Ihre App eine [Mehrbenutzer-App](../xbox-apps/multi-user-applications.md) ist) ein [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Objekt ab, und wandeln Sie es in ein [IInitializeWithWindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow)-Objekt um. Rufen Sie dann die [IInitializeWithWindow.Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize)-Methode auf, und übergeben Sie das Handle des Fensters, das der Besitzer aller modalen Dialoge sein soll, die durch **StoreContext**-Methoden angezeigt werden. Im folgenden C#-Beispiel wird gezeigt, wie das Handle des Hauptfensters der App an die Methode übergeben wird.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Produkte, SKUs und Verfügbarkeiten

Jedes Produkt im Store verfügt über mindestens eine *SKU*, und jede SKU verfügt über mindestens eine *Verfügbarkeit*. Diese Konzepte werden von den meisten Entwicklern in Partner Center abstrahiert, und die meisten Entwickler definieren niemals SKUs oder Verfügbarkeit für Ihre apps oder Add-ons. Da das Objektmodell für Store-Produkte im **Windows. Services. Store** -Namespace jedoch SKUs und Verfügbarkeit enthält, kann ein grundlegendes Verständnis dieser Konzepte für einige Szenarien hilfreich sein.

| Object |  BESCHREIBUNG  |
|---------|-------------------|
| Produkt  |  Ein *Produkt* bezieht sich auf alle Produkttypen, die im Store verfügbar sind, einschließlich einer APP oder eines Add-on. <p/><p/> Jedes Produkt im Geschäft verfügt über ein entsprechendes [storeproduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) -Objekt. Diese Klasse stellt Eigenschaften für den Zugriff auf Daten bereit, z. B. die Store-ID des Produkts, die Bilder und Videos für den Store-Eintrag sowie Preisinformationen. Sie stellt außerdem Methoden bereit, mit denen Sie das Produkt kaufen können. |
| SKU |  Eine *SKU* ist eine bestimmte Version eines Produkts mit einer eigenen Beschreibung, einem Preis und anderen eindeutigen Produktdetails. Jede App und jedes Add-On verfügt über eine Standard-SKU. Die meisten Entwickler verfügen nur dann über mehrere SKUs für eine App, wenn sie eine Vollversion der App und eine Testversion veröffentlichen (im Store-Katalog ist jede dieser Versionen eine andere SKU derselben App). <p/><p/> Einige Herausgeber können ihre eigenen SKUs definieren. Ein großer Spieleherausgeber kann z. B. ein Spiel mit einer SKU, die grünes Blut zeigt, in Märkten veröffentlichen, in denen kein rotes Blut zulässig ist, und eine andere SKU für rotes Blut in allen anderen Märkten. Alternativ kann ein Herausgeber, der digitale Videoinhalte verkauft, zwei SKUs für ein Video veröffentlichen, eine SKU für eine HD-Version und eine andere SKU für die SD-Version. <p/><p/> Jede SKU im Speicher verfügt über ein entsprechendes [storesku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) -Objekt. Jedes [storeproduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) verfügt über eine [SKUs](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) -Eigenschaft, die Sie für den Zugriff auf die SKUs für das Produkt verwenden können. |
| Verfügbarkeit  |  Eine *Verfügbarkeit* ist eine bestimmte Version einer SKU mit ihren eigenen eindeutigen Preisinformationen. Jede SKU verfügt über eine Standardverfügbarkeit. Einige Herausgeber können eigene Verfügbarkeiten zum Vorstellen unterschiedlicher Preisoptionen für eine bestimmte SKU definieren. <p/><p/> Jede Verfügbarkeit im Speicher verfügt über ein entsprechendes [storeavailability](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) -Objekt. Jede [storesku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) verfügt über eine Eigenschaft [availabilitäten](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) , mit der Sie auf die Verfügbarkeit der SKU zugreifen können. Für die meisten Entwickler verfügt jede SKU über eine einzelne Standardverfügbarkeit.  |

<span id="store_ids" />

### <a name="store-ids"></a>Store-IDs

Jede APP, jedes Add-on oder ein anderes Produkt im Speicher verfügt über eine zugeordnete **Speicher-ID** (auch als *Produkt Store-ID*bezeichnet). Viele APIs erfordern die Store-ID, um einen Vorgang für eine APP oder ein Add-on auszuführen.

Die Store-ID eines Produkts im Store ist eine zwölfstellige alphanumerische Zeichenfolge, z. B. ```9NBLGGH4R315```. Es gibt verschiedene Möglichkeiten, die Store-ID für ein Produkt im Geschäft zu erhalten:

* Für eine APP können Sie die Store-ID auf der [Seite "App-Identität](../publish/view-app-identity-details.md) " im Partner Center erhalten.
* Für ein Add-on können Sie die Store-ID auf der Übersichtsseite des Add-ons im Partner Center erhalten.
* Für jedes Produkt können Sie die Store-ID auch Programm gesteuert mithilfe der [StoreID](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) -Eigenschaft des [storeproduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) -Objekts, das das Produkt darstellt, erhalten.

Für Produkte mit SKUs und Verfügbarkeit verfügen die SKUs und Verfügbarkeit auch über eigene Store-IDs mit unterschiedlichen Formaten.

| Object |  Store-ID-Format  |
|---------|-------------------|
| SKU |  Die Speicher-ID für eine SKU hat das Format ```<product Store ID>/xxxx``` , wobei ```xxxx``` eine Zeichenfolge mit vier Zeichen als alphanumerische Zeichenfolge ist, die eine SKU für das Produkt angibt. Beispiel: ```9NBLGGH4R315/000N```. Diese ID wird von der [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid)-Eigenschaft eines [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku)-Objekts zurückgegeben und mitunter als die *SKU-Store-ID* bezeichnet. |
| Verfügbarkeit  |  Die Speicher-ID für eine Verfügbarkeit weist das Format auf ```<product Store ID>/xxxx/yyyyyyyyyyyy``` , wobei ```xxxx``` eine alphanumerische Zeichenfolge mit vier Zeichen ist, die eine SKU für das Produkt identifiziert und ```yyyyyyyyyyyy``` eine alphanumerische Zeichenfolge mit 12 Zeichen ist, die eine Verfügbarkeit für die SKU identifiziert. Beispiel: ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Diese ID wird von der [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid)-Eigenschaft eines [StoreAvailability](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability)-Objekts zurückgegeben und mitunter als die *Verfügbarkeits-Store-ID* bezeichnet.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Verwenden von Produkt-IDs für Add-ons in Ihrem Code

Wenn Sie Ihren Kunden im Kontext ihrer App ein Add-on zur Verfügung stellen möchten, müssen Sie beim [Erstellen der Add-on-Übermittlung](../publish/add-on-submissions.md) im Partner Center [eine eindeutige Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) für das Add-on eingeben. Sie können diese Produkt-ID verwenden, um auf das Add-on in Ihrem Code zu verweisen, obwohl die spezifischen Szenarien, in denen Sie die Produkt-ID verwenden können, von dem Namespace abhängen, den Sie für in-App-Käufe in Ihrer APP verwenden.

> [!NOTE]
> Die Produkt-ID, die Sie im Partner Center für ein Add-on eingeben, unterscheidet sich von der [Store-ID](#store-ids)des Add-ons. Die Store-ID wird von Partner Center generiert.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Apps, die den Windows. Services. Store-Namespace verwenden

Wenn Ihre APP den **Windows. Services. Store** -Namespace verwendet, können Sie die Produkt-ID verwenden, um das [storeproduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) leicht zu identifizieren, das das Add-on darstellt, oder die [storelicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) , die die Lizenz Ihres Add-ons darstellt. Die Produkt-ID wird von den Eigenschaften [storeproduct. inappoffertoken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) und [storelicense. inappoffertoken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken) bereitgestellt.

> [!NOTE]
> Obwohl die Produkt-ID eine nützliche Möglichkeit ist, ein Add-on in Ihrem Code zu identifizieren, verwenden die meisten Vorgänge im **Windows. Services. Store** -Namespace die [Store-ID](#store-ids) eines Add-on anstelle der Produkt-ID. Wenn Sie z. b. ein oder mehrere bekannte Add-ons für eine APP Programm gesteuert abrufen möchten, übergeben Sie die Store-IDs (und nicht die Produkt-IDs) der Add-ons an die [getstoreproductionasync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) -Methode. Wenn Sie ein nutzbares Add-on als erfüllt melden möchten, übergeben Sie die Store-ID des Add-ons (und nicht die Produkt-ID) an die [reportconsumableerfüllllmentasync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) -Methode.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Apps, die den Windows. applicationmodel. Store-Namespace verwenden

Wenn Ihre APP den **Windows. applicationmodel. Store** -Namespace verwendet, müssen Sie die Produkt-ID verwenden, die Sie einem Add-on im Partner Center für die meisten Vorgänge zuweisen. Beispiel:

* Verwenden Sie die Produkt-ID, um das [productlisting](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) zu identifizieren, das das Add-on darstellt, oder [productlicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) , das die Lizenz Ihres Add-ons darstellt. Die Produkt-ID wird von den Eigenschaften [productlisting. ProductID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) und [productlicense. ProductID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId) verfügbar gemacht.

* Geben Sie die Produkt-ID an, wenn Sie das Add-on für den Benutzer mit der [requestproductpurchaseasync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) -Methode erwerben. Weitere Informationen finden Sie unter [Ermöglichen von In-App-Produktkäufen](enable-in-app-product-purchases.md).

* Geben Sie die Produkt-ID an, wenn Sie das nutzbare Add-on als erfüllt melden, indem Sie die [reportconsumableerfüllllmentasync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) -Methode verwenden. Weitere Informationen finden Sie unter Aktivieren von nutzbaren [in-App-Produkt Käufen](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Aktivieren von Abonnement-Add-Ons für die App](enable-subscription-add-ons-for-your-app.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Fehlercodes für Store-Vorgänge](error-codes-for-store-operations.md)
* [In-App-Käufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
