---
author: Xansky
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Erfahren Sie, wie Sie In-App-Käufe und Testversionen in UWP-Apps aktivieren.
title: In-App-Käufe und Testversionen
ms.author: mhopkins
ms.date: 05/09/2018
ms.topic: article
keywords: windows10, uwp, In-app-käufe, IAPs-add-ons, Testversionen, verbrauchbar, dauerhaft, abonnement
ms.localizationpriority: medium
ms.openlocfilehash: 17a93e10f7b440ac7c1926bc64bba4d9be248710
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861019"
---
# <a name="in-app-purchases-and-trials"></a>In-App-Käufe und Testversionen

Das Windows SDK enthält APIs, mit denen Sie die folgenden Features implementieren können, um mit Ihrer UWP-App (Universelle Windows-Plattform) mehr Geld zu verdienen:

* **In-App-Käufe**&nbsp;&nbsp;Unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, können Sie Inhalte oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen.

* **Testfunktionen**&nbsp;&nbsp;Wenn Sie Ihre App als [kostenlose Testversion im Windows Dev Center-Dashboard](../publish/set-app-pricing-and-availability.md#free-trial) konfigurieren, können Sie Ihre Kunden zum Kauf der Vollversion Ihrer App motivieren, indem Sie einige Features für den Testzeitraum einschränken oder ausschließen. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

Dieser Artikel enthält eine Übersicht darüber, wie In-App-Käufe und Testversionen in UWP-Apps funktionieren.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Auswahl des zu verwendenden Namespace

Je nachdem, für welche Version von Windows10 Ihre App ausgelegt ist, können Sie mithilfe zweier verschiedener Namespaces Ihren UWP-Apps In-App-Käufe und Testversionen hinzufügen. Obwohl die APIs in diesen Namespaces den gleichen Ziele dienen, sind sie unterschiedlich gestaltet, und der Code ist zwischen den beiden APIs nicht kompatibel.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;Ab Windows10 Version1607 können mithilfe der API in diesem Namespace In-App-Käufe und Testversionen in den Apps implementiert werden. Es wird empfohlen, die Member in diesem Namespace zu verwenden, wenn Ihr App-Projekt auf **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher in Visual Studio ausgerichtet ist. Dieser Namespace unterstützt die neuesten Add-On-Typen, wie vom Store verwaltete konsumierbare Add-Ons, und ist so gestaltet, dass er mit zukünftigen Arten von Produkten und Features kompatibel ist, die von Windows Dev Center und dem Store unterstützt werden. Weitere Informationen zu diesem Namespace finden Sie in diesem Artikel im Abschnitt [In-App-Käufe und Testversionen mit dem Windows.Services.Store-Namespace](#api_intro).

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;Alle Versionen von Windows10 unterstützen in diesem Namespace zudem eine ältere API für In-App-Käufe und Testversionen. Informationen zum **Windows.ApplicationModel.Store**-Namespace finden Sie unter [In-App-Käufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> Der The **Windows.ApplicationModel.Store**-Namespace wird nicht mehr mit neuen Funktionen aktualisiert, daher wird empfohlen, dass Sie stattdessen den **Windows.Services.Store**-Namespace falls möglich für Ihre App verwenden. Der **Windows.ApplicationModel.Store**-Namespace wird nicht in den Windows-Desktopanwendungen unterstützt, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwenden oder in Apps oder Spiele, die eine Sandbox-Entwicklung im Dev Center verwenden (z.B. ist dies der Fall für alle Spiele, die mit Xbox Live integrieren).

<span id="concepts" />

## <a name="basic-concepts"></a>Grundlegende Konzepte

Jedes im Store angebotene Element wird im Allgemeinen als *Produkt* bezeichnet. Die meisten Entwickler arbeiten nur mit den folgenden Arten von Produkten: *Apps* und *Add-Ons*.

Ein Add-On ist ein Produkt oder Feature, das Sie Ihren Kunden im Kontext Ihrer App zur Verfügung stellen: z.B. in einer App oder einem Spiel zu verwendende Währung, neue Karten oder Waffen für ein Spiel, die Möglichkeit zur Verwendung der App ohne Werbung oder digitale Inhalte wie Musik oder Videos für Apps, die diese Art von Inhalten anbieten können. Jede App und jedes Add-On verfügt über eine zugehörige Lizenz, die angibt, ob der Benutzer zur Verwendung dieser App oder des Add-Ons berechtigt ist. Wenn der Benutzer berechtigt ist, die App bzw. das Add-On als Testversion zu verwenden, enthält die Lizenz auch zusätzliche Informationen zur Testversion.

Um den Kunden in der App ein Add-On anzubieten, müssen Sie [das Add-On für Ihre App im Dev Center-Dashboard definieren](../publish/add-on-submissions.md), damit es im Store bekannt ist. Anschließend kann das Add-On mithilfe der APIs im **Windows.Services.Store**-Namespace oder im **Windows.ApplicationModel.Store**-Namespace den Kunden zum Erwerb als In-App-Kauf angeboten werden.

Für UWP-Apps können die folgenden Arten von Add-Ons angeboten werden.

| Add-On-Typ |  Beschreibung  |
|---------|-------------------|
| Gebrauchsgut  |  Ein Add-On, das für die Lebensdauer beibehalten wird, die Sie im [Windows Dev Center-Dashboard](../publish/enter-iap-properties.md) angeben. <p/><p/>Standardmäßig laufen Gebrauchsgut-Add-Ons nie ab, in diesem Fall können sie nur einmal gekauft werden. Wenn Sie eine bestimmte Dauer für das Add-On angeben, kann der Benutzer das Add-On erneut kaufen, nachdem es abgelaufen ist. |
| Vom Entwickler verwaltetes Endverbraucher-Add-On  |  Ein Add-On, das erworben, verwendet (konsumiert) und anschließend erneut gekauft werden kann. Sie sind dafür verantwortlich, das Guthaben des Benutzers an Elementen, die das Add-On darstellen, nachzuverfolgen.<p/><p/>Wenn der Benutzer Elemente verwendet, die mit dem Add-On verknüpft sind, sind Sie dafür verantwortlich, das Guthaben des Benutzers zu verwalten und den Kauf des Add-Ons als erfüllt an den Store zu melden, nachdem der Benutzer alle Elemente verwendet hat. Der Benutzer kann das Add-On erst dann erneut kaufen, nachdem Ihre App den vorherigen Kauf des Add-Ons als erfüllt gemeldet hat. <p/><p/>Wenn beispielsweise das Add-On 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, muss die App oder der Dienst den neuen Restbetrag von 90 Münzen für den Benutzer verwalten. Nachdem der Benutzer alle 100 Münzen genutzt hat, muss die App das Add-On als erfüllt melden, und danach kann der Benutzer das Add-On für 100 Münzen erneut kaufen.    |
| Vom Store verwaltetes Endverbraucher-Add-On  |  Ein Add-On, das gekauft, verwendet und jederzeit erneut gekauft werden kann. Der Store verfolgt das Guthaben des Benutzers an Elementen, die das Add-On darstellen.<p/><p/>Wenn der Benutzer Elemente verwendet, die mit dem Add-On verknüpft sind, sind Sie dafür verantwortlich, diese Elemente als erfüllt an den Store zu melden, und der Store aktualisiert das Konto des Benutzers. Der Benutzer kann das Add-On beliebig oft kaufen (er muss die Elemente nicht zuerst verwenden). Ihre App kann das aktuelle Guthaben für den Benutzer jederzeit abfragen. <p/><p/> Wenn das Add-On beispielsweise eine anfängliche Menge von 100 Münzen in einem Spiel darstellt und der Benutzer 50 Münzen nutzt, meldet die App dem Store, dass 50 Einheiten des Add-Ons verwendet wurden, und der Store aktualisiert den Restbetrag. Wenn der Benutzer dann Ihr Add-on erneut kauft, um 100 weitere Münzen zu erhalten, hat er jetzt 150 Münzen insgesamt. <p/><p/>**Hinweis**&nbsp;&nbsp;Um vom Store verwaltete Endverbraucher-Add-Ons verwenden zu können, muss Ihre App für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher in Visual Studio ausgerichtet sein. Zudem muss der **Windows.Services.Store**-Namespace anstatt von **Windows.ApplicationModel.Store**-Namespace verwendet werden.  |
| Abonnement | Ein dauerhaftes Add-On, das dem Kunden in regelmäßigen Abständen in Rechnung gestellt wird, damit das Add-On weiterhin verwendet werden kann. Der Kunde kann das Abonnement jederzeit kündigen, um weitere Gebühren zu vermeiden. <p/><p/>**Hinweis**&nbsp;&nbsp;Um Abonnement-Add-Ons verwenden zu können, muss Ihre App in Visual Studio für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher entwickelt worden sein. Zudem muss der Namespace **Windows.Services.Store** statt dem Namespace **Windows.ApplicationModel.Store** verwendet werden.  |

<span />

> [!NOTE]
> Andere Arten von Add-Ons wie dauerhafte Add-Ons mit Paketen (auch als herunterladbare Inhalte oder DLC bekannt) stehen nur einer eingeschränkten Gruppe von Entwicklern zur Verfügung und werden in dieser Dokumentation nicht behandelt.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>In-App-Käufe und Testversionen mit dem Windows.Services.Store-Namespace

Dieser Abschnittenthält eine Übersicht über wichtige Aufgaben und Konzepte für den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace. Dieser Namespace ist nur für Apps für **Windows10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verfügbar (dies entspricht Windows10, Version 1607). Es wird empfohlen, den **Windows.Services.Store**-Namespace anstelle des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace zu verwenden. Informationen zum **Windows.ApplicationModel.Store**-Namespace, finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**Inhalt dieses Abschnitts**

* [Video](#video)
* [Erste Schrittemit der StoreContext-Klasse](#get-started-storecontext)
* [Implementieren von In-App-Käufen](#implement-iap)
* [Implementieren der Testfunktionen](#implement-trial)
* [Testen der Implementierung für den In-App-Kauf oder die Testversion](#testing)
* [Belege für In-App-Käufe](#receipts)
* [Verwenden der StoreContext-Klasse mit der Desktop-Brücke](#desktop)
* [Produkte, SKUs und Verfügbarkeiten](#products-skus)
* [Store-IDs](#store-ids)

<span id="video" />

### <a name="video"></a>Video

Im folgenden Video wird eine Übersicht über das Implementieren von In-App-Käufe in Ihrer App mithilfe des [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace bereitgestellt.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Erste Schrittemit der StoreContext-Klasse

Der Haupteinstiegspunkt in den **Windows.Services.Store**-Namespace ist die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse. Diese Klasse stellt Methoden bereit, mit denen Sie Informationen für die aktuelle App und die verfügbaren Add-Ons abrufen, eine App oder ein Add-On für den aktuellen Benutzer kaufen, Lizenzinformationen für die aktuelle App oder die Add-Ons abrufen und weitere Aufgaben durchführen können. Gehen Sie zum Abrufen eines [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekts wie folgt vor:

* Rufen Sie in einer Einzelbenutzer-App (einer App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat) mit der statischen [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault)-Methode ein **StoreContext**-Objekt ab, mit dem Sie auf Microsoft Store-bezogene Daten für den Benutzer zugreifen können. Die meisten Apps für die universelle Windows-Plattform (UWP) sind Einzelbenutzer-Apps.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Verwenden Sie in einer [Mehrbenutzer-App](../xbox-apps/multi-user-applications.md) die statische [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser)-Methode, um ein **StoreContext**-Objekt abzurufen, mit dem Sie für einen bestimmten Benutzer, der beim Verwenden der App mit seinem Microsoft-Konto angemeldet ist, auf Microsoft Store-bezogene Daten zugreifen können. Im folgenden Beispiel wird ein **StoreContext**-Objekt für den ersten verfügbaren Benutzer abgerufen.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Bei Windows-Desktopanwendungen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwenden, müssen zum Konfigurieren des [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekts zusätzliche Schritte ausgeführt werden, bevor dieses verwendet werden kann. Weitere Informationen finden Sie in [diesem Abschnitt](#desktop).

Nachdem Sie über ein [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekt verfügen, können Sie beginnen, Methoden dieses Objekts aufzurufen, um Store-Produktinformationen und Lizenzinformationen für die aktuelle App und deren Add-Ons abzurufen, eine App oder ein Add-On für den aktuellen Benutzer zu erwerben und weitere Aufgaben auszuführen. Weitere Informationen zu den allgemeinen Aufgaben, die Sie mit diesem Objekt ausführen können, finden Sie in den folgenden Artikeln:

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Käufen konsumierbarer Add-Ons](enable-consumable-add-on-purchases.md)
* [Aktivieren von Abonnements für Add-Ons für Ihre App](enable-subscription-add-ons-for-your-app.md)
* [Implementieren einer Testversion Ihrer App](implement-a-trial-version-of-your-app.md)

Eine Beispiel-App, die die Verwendung von **StoreContext** und anderen Typen im **Windows.Services.Store**-Namespace aufzeigt, finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implementieren von In-App-Käufen

So bieten Sie den Kunden mit dem **Windows.Services.Store**-Namespace in Ihrer App In-App-Käufe an

1. Wenn Ihre App über Add-Ons verfügt, die die Kunden kaufen können, [erstellen Sie Add-On-Übermittlungen im Dev Center-Dashboard für Ihre App](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Schreiben Sie Code für Ihre App, um [Produktinformationen über Ihre App oder ein von der App angebotenes Add-On abzurufen](get-product-info-for-apps-and-add-ons.md). [Ermitteln Sie anschließend, ob die Lizenz aktiv ist](get-license-info-for-apps-and-add-ons.md) (d.h., ob der Benutzer über eine Lizenz für die App bzw. das Add-On verfügt). Wenn die Lizenz nicht aktiv ist, zeigen Sie eine Benutzeroberfläche an, auf der die App bzw. das Add-On dem Benutzer als In-App-Kauf angeboten wird.

3. Wenn sich der Benutzer für den Kauf Ihrer App oder Ihres Add-Ons entscheidet, verwenden Sie für den Erwerb des Produkts die geeignete Methode:

    * Wenn der Benutzer Ihre App oder ein dauerhaftes Add-On erwirbt, befolgen Sie die Anweisungen in [Unterstützen von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Wenn der Benutzer ein konsumierbares Add-On erwirbt, befolgen Sie die Anweisungen in [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md).
    * Wenn der Benutzer ein Abonnement-Add-On erwirbt, befolgen Sie die Anweisungen in [Unterstützen von Abonnement-Add-Ons für Ihre App](enable-subscription-add-ons-for-your-app.md).

4. Testen der Implementierung anhand der [Hinweise für Tests](#testing) in diesem Artikel.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implementieren der Testfunktionen

So schränken Sie mit dem **Windows.Services.Store**-Namespace Features in einer Testversion Ihrer App ein oder schließen diese aus

1. [Konfigurieren Sie Ihre App im Windows Dev Center-Dashboard als kostenlose Testversion](../publish/set-app-pricing-and-availability.md#free-trial).

2. Schreiben Sie Code für Ihre App, um [Produktinformationen über Ihre App oder ein von der App angebotenes Add-On abzurufen](get-product-info-for-apps-and-add-ons.md), und [ermitteln Sie anschließend, ob es sich bei der der App zugeordneten Lizenz um eine Testlizenz handelt](get-license-info-for-apps-and-add-ons.md).

3. Handelt es sich um eine Testversion der App, schließen Sie bestimmte Features aus oder schränken Sie sie ein, und aktivieren Sie diese, wenn der Benutzer eine Lizenz für die Vollversion erwirbt. Weitere Informationen und ein Codebeispiel finden Sie unter [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md).

4. Testen der Implementierung anhand der [Hinweise für Tests](#testing) in diesem Artikel.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Testen der Implementierung für den In-App-Kauf oder die Testversion

Wenn Ihre App APIs im **Windows.Services.Store**-Namespace zum Implementieren von In-App-Käufe und Testfunktionen verwendet, müssen Sie Ihre App im Store veröffentlichen und die App auf Ihrem Entwicklungsgerät herunterladen, um seine Lizenz für Tests zu verwenden. Gehen Sie folgendermaßen vor, um Ihren Code zu testen:

1. Wenn Ihre App noch nicht veröffentlicht und im Store verfügbar ist, stellen Sie sicher, dass diese die minimalen Anforderungen des [Zertifizierungskits für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) erfüllt, [übermitteln Sie die App](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) an das Windows Dev Center-Dashboard, und stellen Sie sicher, dass Ihre App den Zertifizierungsprozess bestanden hat. Optional können Sie [die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) ist. Bitte beachten Sie die ordnungsgemäße Konfiguration des [Flight-Pakete](../publish/package-flights.md). Falsch konfigurierte Paket, die Pakete sind möglicherweise nicht heruntergeladen werden.

2. Stellen Sie anschließend sicher, dass die folgenden Schritte durchgeführt wurden:

    * Schreiben Sie Code für Ihre App, die die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse sowie weitere verwandte Typen im **Windows.Services.Store**-Namespace verwendet, um [In-App-Käufe](#implement-iap) oder [Testfunktionen](#implement-trial) zu implementieren.
    * Wenn Ihre App über ein Add-On verfügt, das Kunden kaufen können, [erstellen Sie eine Add-On-Übermittlung für Ihre App im Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Wenn Sie einige Features in der Testversion Ihrer App einschränken oder ausschließen möchten, [konfigurieren Sie die App im Windows Dev Center-Dashboard als kostenlose Testversion](../publish/set-app-pricing-and-availability.md#free-trial).

3. Öffnen Sie Ihr Projekt in Visual Studio, klicken Sie auf das **Menü „Projekt“**, zeigen Sie auf **Store**, und klicken Sie dann auf **App mit Store verknüpfen**. Befolgen Sie die Anweisungen des Assistenten, um das App-Projekt mit der App im Windows Dev Center-Konto zu verknüpfen, die Sie zum Testen verwenden möchten.
    > [!NOTE]
    > Wenn Sie das Projekt nicht mit einer App im Store verknüpfen, legen die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Methoden die **ExtendedError**-Eigenschaften ihrer Rückgabewerte auf den Fehlercodewert 0x803F6107 fest. Dieser Wert gibt an, dass die App im Store nicht bekannt ist.
4. Falls noch nicht geschehen, installieren Sie die App aus dem im vorherigen Schritt angegebenen Store, führen Sie die App einmal aus, und schließen Sie dann diese App. Damit wird sichergestellt, dass auf dem Gerät für die Entwicklung eine gültige Lizenz für die App installiert wird.

5. Starten Sie in Visual Studio die Ausführung oder das Debuggen des Projekts. Der Code sollte App- und Add-On-Daten aus der Store-App abrufen, die Sie mit dem lokalen Projekt verknüpft haben. Wenn Sie zur Neuinstallation der App aufgefordert werden, folgen Sie den Anweisungen, und führen Sie dann das Projekt aus oder debuggen Sie es.
    > [!NOTE]
    > Nachdem Sie diese Schritte ausgeführt haben, können Sie mit dem Aktualisieren des App-Codes fortfahren und dann das aktualisierte Projekt auf dem Entwicklungscomputer debuggen, ohne neue App-Pakete an den Store zu übermitteln. Sie müssen die Store-Version Ihrer App nur einmal auf den Entwicklungscomputer herunterladen, um die lokale Lizenz zu erhalten, die zum Testen verwendet wird. Sie übermitteln neue App-Pakete erst an den Store, nachdem Sie die Tests abgeschlossen haben und Ihren Kunden App-Features zur Verfügung stellen möchten, die sich auf In-App-Käufe oder Testversionen beziehen.

Wenn Ihre App den **Windows.ApplicationModel.Store**-Namespace verwendet, können Sie die [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse in Ihrer App nutzen, um Lizenzinformationen während der Tests vor dem Übermitteln an den Store simulieren zu können. Weitere Informationen finden Sie unter [Erste Schrittemit den Klassen CurrentApp und CurrentAppSimulator] (in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> Der **Windows.Services.Store**-Namespace stellt keine Klasse bereit, mit der Sie während der Tests Lizenzinformationen simulieren können. Wenn den **Windows.Services.Store**-Namespace zum Implementieren von In-App-Käufe oder Testversionen verwendet, müssen Sie Ihre App im Store wie oben beschrieben veröffentlichen und die App auf Ihrem Entwicklungsgerät herunterladen, um seine Lizenz für Tests zu verwenden.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Belege für In-App-Käufe

Der **Windows.Services.Store**-Namespace verfügt über keine API, mit der Sie im Code der App einen Transaktionsbeleg für erfolgreiche Käufe erhalten können. Dies unterscheidet sich von Apps, die den **Windows.ApplicationModel.Store**-Namespace verwenden, der [zum Abrufen von Transaktionsbelegen eine clientseitige API verwenden kann](use-receipts-to-verify-product-purchases.md).

Wenn Sie In-App-Käufe mit dem **Windows.Services.Store**-Namespace implementieren und überprüfen möchten, ob ein bestimmter Kunde eine App oder ein Add-On erworben hat, können Sie die [Produktmethodenabfrage](query-for-products.md) in der [REST-API der Microsoft Store-Sammlung](view-and-grant-products-from-a-service.md) verwenden. Die für diese Methode zurückgegebenen Daten bestätigen, ob der jeweilige Kunde über eine Berechtigung für ein bestimmtes Produkt verfügt. Zudem werden Daten für die Transaktion bereitgestellt, im Rahmen derer der Benutzer das Produkt erworben hat. Die Microsoft Store-Sammlungs-API verwendet zum Abrufen dieser Informationen die Azure AD-Authentifizierung.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Verwenden der StoreContext-Klasse mit der Desktop-Brücke

Für Desktopanwendungen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwenden, kann zum Implementieren von In-App-Käufen und Testversionen die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse verwendet werden. Wenn Sie jedoch über eine Win32-Desktopanwendung oder eine Desktopanwendung mit Fenster-Handle (HWND) verfügen, die dem Renderingframework zugeordnet ist (z.B. eine WPF-Anwendung), muss für Ihre Anwendung das **StoreContext**-Objekt konfiguriert werden, um anzugeben, welches Anwendungsfenster das Besitzerfenster für die vom Objekt angezeigten modalen Dialogfelder ist.

Viele **StoreContext**-Member (und andere Member verwandter Typen, auf die über das **StoreContext**-Objekt zugegriffen wird) zeigen den Benutzern für Store-bezogene Vorgänge wie z.B. den Kauf eines Produkts ein modales Dialogfeld an. Wenn für eine Desktopanwendung das **StoreContext**-Objekt nicht konfiguriert wurde, um das Besitzerfenster für modale Dialogfelder anzugeben, gibt das Objekt ungenaue Daten oder Fehler zurück.

Führen Sie die folgenden Schritte durch, um ein **StoreContext**-Objekt in einer Desktopanwendung zu konfigurieren, die die Desktop-Brücke verwendet.

1. Führen Sie einen der folgenden Schritte aus, um Ihrer App den Zugriff auf die [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx)-Schnittstelle zu ermöglichen:

    * Wenn die Anwendung in einer verwalteten Sprache wie z.B. C# oder Visual Basic geschrieben wurde, deklarieren Sie die **IInitializeWithWindow**-Schnittstelle im App-Code wie im folgenden C#-Beispiel mit dem [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx)-Attribut. In diesem Beispiel wird davon ausgegangen, dass Ihre Codedatei über eine **using**-Anweisung für den **System.Runtime.InteropServices**-Namespace verfügt.

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

2. Rufen Sie wie weiter oben in diesem Artikel beschrieben mit der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault)-Methode (oder [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser), falls Ihre App eine [Mehrbenutzer-App](../xbox-apps/multi-user-applications.md) ist) ein [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekt ab, und wandeln Sie es in ein [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx)-Objekt um. Rufen Sie dann die [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx)-Methode auf, und übergeben Sie das Handle des Fensters, das der Besitzer aller modalen Dialoge sein soll, die durch **StoreContext**-Methoden angezeigt werden. Im folgenden C#-Beispiel wird gezeigt, wie das Handle des Hauptfensters der App an die Methode übergeben wird.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Produkte, SKUs und Verfügbarkeiten

Jedes Produkt im Store verfügt über mindestens eine *SKU*, und jede SKU verfügt über mindestens eine *Verfügbarkeit*. Diese Konzepte werden im Windows Dev Center-Dashboard für die meisten Entwickler herausgenommen, und die meisten Entwickler definieren nie SKUs oder Verfügbarkeiten für ihre Apps oder Add-Ons. Da jedoch das Objektmodell für Store-Produkte im **Windows.Services.Store**-Namespace SKUs und Verfügbarkeiten umfasst, können Grundkenntnisse zu diesen Konzepten für einige Szenarien hilfreich sein.

| Objekt |  Beschreibung  |
|---------|-------------------|
| Produkt  |  Ein *Produkt* stellt alle im Store verfügbaren Produkttypen dar, einschließlich der Apps und Add-Ons. <p/><p/> Jedes Produkt im Store verfügt über ein entsprechendes [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) Objekt. Diese Klasse stellt Eigenschaften für den Zugriff auf Daten bereit, z.B. die Store-ID des Produkts, die Bilder und Videos für den Store-Eintrag sowie Preisinformationen. Sie stellt außerdem Methoden bereit, mit denen Sie das Produkt kaufen können. |
| SKU |  Eine *SKU* ist eine bestimmte Version eines Produkts mit eigener Beschreibung, Preis und anderen eindeutigen Produktdetails. Jede App und jedes Add-On verfügt über eine Standard-SKU. Die meisten Entwickler verfügen nur dann über mehrere SKUs für eine App, wenn sie eine Vollversion der App und eine Testversion veröffentlichen (im Store-Katalog ist jede dieser Versionen eine andere SKU derselben App). <p/><p/> Einige Herausgeber können ihre eigenen SKUs definieren. Ein großer Spieleherausgeber kann z.B. ein Spiel mit einer SKU, die grünes Blut zeigt, in Märkten veröffentlichen, in denen kein rotes Blut zulässig ist, und eine andere SKU für rotes Blut in allen anderen Märkten. Alternativ kann ein Herausgeber, der digitale Videoinhalte verkauft, zwei SKUs für ein Video veröffentlichen, eine SKU für eine HD-Version und eine andere SKU für die SD-Version. <p/><p/> Jede SKU im Store verfügt über ein entsprechendes [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) Objekt. Jedes [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) hat eine [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus)-Eigenschaft, mit der Sie auf die SKUs für das Produkt zugreifen können. |
| Verfügbarkeit  |  Eine *Verfügbarkeit* ist eine bestimmte Version einer SKU mit eigenen eindeutigen Preisinformationen. Jede SKU verfügt über eine Standardverfügbarkeit. Einige Herausgeber können eigene Verfügbarkeiten zum Vorstellen unterschiedlicher Preisoptionen für eine bestimmte SKU definieren. <p/><p/> Jede Verfügbarkeit im Store verfügt über ein entsprechendes [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) Objekt. Jede [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) verfügt über eine [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities)-Eigenschaft, mit der Sie auf die Verfügbarkeiten für die SKU zugreifen können. Für die meisten Entwickler verfügt jede SKU über eine einzelne Standardverfügbarkeit.  |

<span id="store_ids" />

### <a name="store-ids"></a>Store-IDs

Jede App, jedes Add-On oder jedes andere Produkt im Store hat eine zugeordnete **Store-ID** (diese wird manchmal auch als *Produkt-Store-ID* bezeichnet). Viele APIs erfordern eine Store-ID, um einen Vorgang für eine App oder ein Add-On auszuführen.

Die Store-ID eines Produkts im Store ist eine zwölfstellige alphanumerische Zeichenfolge, z.B. ```9NBLGGH4R315```. Es gibt mehrere Möglichkeiten, die Store-ID für ein Produkt im Store zu erhalten:

* Die Store-ID einer App ist auf der [Seite mit der App-Identität](../publish/view-app-identity-details.md) des DevCenter-Dashboards verfügbar.
* Für ein Add-On finden Sie die Store-ID auf der Übersichtsseite des Add-Ons im Dashboard.
* Für jedes Produkt können Sie außerdem die Store-ID programmgesteuert mit der [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid)-Eigenschaft des [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekts des Produkts erhalten.

Für Produkte mit SKUs und Verfügbarkeiten haben die SKUs und Verfügbarkeiten außerdem eigene Store-IDs in verschiedenen Formaten.

| Objekt |  Store-ID-Format  |
|---------|-------------------|
| SKU |  Die SKU-ID für eine SKU hat das Format ```<product Store ID>/xxxx```, wobei ```xxxx``` eine vierstellige alphanumerische Zeichenfolge ist, die eine SKU für das Produkt identifiziert. Beispiel: ```9NBLGGH4R315/000N```. Diese ID wird von der [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid)-Eigenschaft eines [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx)-Objekts zurückgegeben und mitunter als die *SKU-Store-ID* bezeichnet. |
| Verfügbarkeit  |  Die Store-ID für eine Verfügbarkeit hat das Format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, wobei ```xxxx``` eine vierstellige alphanumerische Zeichenfolge ist, die eine SKU für das Produkt identifiziert, und ```yyyyyyyyyyyy``` eine zwölfstellige alphanumerische Zeichenfolge, die eine Verfügbarkeit für die SKU identifiziert. Beispiel: ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Diese ID wird von der [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid)-Eigenschaft eines [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)-Objekts zurückgegeben und mitunter als die *Verfügbarkeits-Store-ID* bezeichnet.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>So verwenden Sie die Produkt-IDs für Add-Ons im Code

Wenn Sie ein Add-On für Ihre Kunden im Kontext Ihrer App zur Verfügung stellen möchten, müssen Sie [eine eindeutige Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) für Ihr Add-On eingeben, wenn Sie [Ihrer Add-On-Übermittlung](../publish/add-on-submissions.md) im Dev Center-Dashboard erstellen. Sie können diese Produkt-ID verwenden, um in Ihrem Code auf das Add-On zu verweisen. Die Szenarien, in denen Sie die Produkt-ID verwenden können, hängen jedoch davon ab, welchen Namespace Sie für In-App-Käufe in Ihrer App verwenden.

> [!NOTE]
> Die im Dev Center-Dashboard eingegebene Produkt-ID für ein Add-On unterscheidet sich von der [Store-ID ](#store-ids) des Add-Ons. Die Store-ID wird von Dev Center generiert.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Apps, die den Windows.Services.Store-Namespace verwenden

Wenn Ihre App den **Windows.Services.Store**-Namespace verwendet, können Sie die Produkt-ID verwenden, um das [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) zu identifizieren, das das Add-On oder die [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) darstellt, die Ihre Lizenz darstellt. Die Produkt-ID wird über die Eigenschaften [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) und [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken) verfügbar gemacht.

> [!NOTE]
> Zwar ist die Produkt-ID eine praktische Möglichkeit zum Identifizieren eines Add-Ons im Code, der Großteil der Vorgänge im **Windows.Services.Store**-Namespace verwendet jedoch die [Store-ID](#store-ids) eines Add-Ons anstelle der Produkt-ID. Um z.B. um eines oder mehrere bekannte Add-Ons für eine App programmgesteuert abrufen, übergeben Sie die Store-IDs (statt die Produkt-IDs) der Add-Ons an die [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync)-Methode. Um ein verbrauchbares Add-On als erfüllt zu melden, übergeben Sie ebenfalls die Store-ID des Add-Ons (statt die Produkt-ID) an die [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync)-Methode.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Apps, die den Windows.ApplicationModel.Store-Namespace verwenden

Wenn Ihre App den **Windows.ApplicationModel.Store**-Namespace verwendet, müssen Sie die Produkt-ID verwenden, die Sie für ein Add-On im Dev Center-Dashboard zuweisen. Zum Beispiel:

* Verwenden Sie die Produkt-ID zum Identifizieren der [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting), die das Add-On darstellt oder der [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) die die Lizenz Ihres Add-Ons darstellt. Die Produkt-ID wird über die Eigenschaften [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) und [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId) verfügbar gemacht.

* Geben Sie die Produkt-ID an, wenn Sie das Add-Ons für den Benutzer mithilfe der [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Methode kaufen. Weitere Informationen finden Sie unter [Ermöglichen von In-App-Produktkäufen](enable-in-app-product-purchases.md).

* Geben Sie die Produkt-ID an, wenn Sie das verbrauchbare Add-Ons für den Benutzer mithilfe der [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync)-Methode als erfüllt melden. Weitere Informationen finden Sie unter [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Verwandte Themen

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Käufen konsumierbarer Add-Ons](enable-consumable-add-on-purchases.md)
* [Aktivieren von Abonnement-Add-Ons für die App](enable-subscription-add-ons-for-your-app.md)
* [Implementieren einer Testversion Ihrer App](implement-a-trial-version-of-your-app.md)
* [Fehlercodes für Store-Vorgänge](error-codes-for-store-operations.md)
* [In-App-Käufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
