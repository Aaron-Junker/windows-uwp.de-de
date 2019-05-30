---
Description: In diesem Thema werden Leistungsrichtlinien für Apps beschrieben, für die der Zugriff auf den Standort eines Benutzers erforderlich ist.
title: Richtlinien für Apps mit Standortbestimmung
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Standort, Ort, Karte, Geolocation
ms.localizationpriority: medium
ms.openlocfilehash: b92d74332bb13a11adc25cb33c0d026e14a5b9e9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371666"
---
# <a name="guidelines-for-location-aware-apps"></a>Richtlinien für Apps mit Standortbestimmung




**Wichtige APIs**

-   [**GeoLocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation)
-   [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)

In diesem Thema werden Leistungsrichtlinien für Apps beschrieben, für die der Zugriff auf den Standort eines Benutzers erforderlich ist.

## <a name="recommendations"></a>Empfehlungen


-   Starten Sie mithilfe des Standortobjekts nur dann, wenn die App Standortdaten erfordert.

    Rufen Sie [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) auf, bevor Sie auf die Position des Benutzers zugreifen. Zu diesem Zeitpunkt muss sich Ihre App im Vordergrund befinden, und **RequestAccessAsync** muss vom UI-Thread aufgerufen werden. Solange der Benutzer Ihrer App keinen Zugriff auf seine Position gewährt hat, kann Ihre App nicht auf Positionsdaten zugreifen.

-   Wenn die Verwendung des Standorts für Ihre App nicht zwingend erforderlich ist, greifen Sie erst dann auf das Gerät zu, wenn dies vom Benutzer angefordert wird. Besitzt eine App für ein soziales Netzwerk beispielsweise eine Schaltfläche, um sich mit dem eigenen Standort anzumelden, sollte die App nicht auf den Standort zugreifen, bis der Benutzer auf die Schaltfläche klickt. Es geht in Ordnung, sofort auf den Standort zuzugreifen, wenn es für die Hauptfunktion Ihrer App erforderlich ist.

-   Die erste Verwendung des [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekts muss im Hauptthread der Benutzeroberfläche der Vordergrund-App erfolgen, um eine Zustimmungsaufforderung für den Benutzer auszulösen. Bei der ersten Verwendung des **Geolocator**-Objekts kann es sich um den ersten Aufruf von [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) handeln oder um die erste Registrierung eines Handlers für das [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignis.

-   Informieren Sie den Benutzer über die Verwendung von Standortdaten.
-   Stellen Sie eine Benutzeroberfläche bereit, um Benutzern zu ermöglichen, ihren Standort manuell zu aktualisieren.
-   Zeigen Sie eine Statusleiste bzw. einen Statusring an, während auf das Abrufen der Geolocation-Daten gewartet wird. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   Zeigen Sie passende Fehlermeldungen oder Dialogfelder an, wenn Positionsdienste deaktiviert oder nicht verfügbar sind.

    Wenn Ihre App gemäß den Positionseinstellungen nicht auf die Position des Benutzers zugreifen darf, wird empfohlen, einen praktischen Link zu den **Datenschutzeinstellungen für den Standort** in der **Einstellungs**-App bereitzustellen. Beispielsweise können Sie ein Hyperlinksteuerelement verwenden oder die [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode aufrufen, um die **Einstellungs**-App über Code mithilfe des `ms-settings:privacy-location`-URIs zu starten. Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-settings-app).

-   Löschen Sie zwischengespeicherte Standortinformationen, und geben Sie das [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt frei, wenn der Zugriff auf die Standortinformationen vom Benutzer deaktiviert wird.

    Geben Sie das [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt frei, wenn der Benutzer den Zugriff auf Standortinformationen unter „Einstellungen“ deaktiviert. Die app erhält **Zugriff\_"abgelehnt"** Ergebnisse für alle ortungs-API-Aufrufe. Wenn Ihre App Standortdaten (zwischen)speichert, löschen Sie zwischengespeicherte Daten, wenn der Benutzer den Zugriff auf seinen Standort zurücknimmt. Stellen Sie eine Alternativmöglichkeit zur Angabe des Standorts bereit, wenn keine diesbezüglichen Informationen für Positionsdienste verfügbar sind.

-   Stellen Sie eine Benutzeroberfläche für das erneute Aktivieren der Positionsdienste bereit. Geben Sie z. B. eine Schaltfläche "Aktualisieren", die erneut instanziiert die [ **Geolocator** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) -Objekt und versucht, Speicherortinformationen erneut abzurufen.

    Stellen Sie in Ihrer App eine Benutzeroberfläche zum Aktivieren der Positionsdienste bereit.

    -   Wenn der Benutzer den Standortzugriff nach dem Deaktivieren erneut aktiviert, wird die App nicht benachrichtigt. Die [**status**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.statuschangedeventargs.status)-Eigenschaft ändert sich nicht, und es findet kein [**statusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignis statt. Ihre App sollte ein neues [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt erstellen und [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) aufrufen, um zu versuchen, aktualisierte Standortdaten zu erhalten, oder [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignisse erneut abonnieren. Wenn aus dem Status hervorgeht, dass die Standortbestimmung erneut aktiviert wurde, müssen Sie alle UI-Elemente entfernen, die die App zuvor angezeigt hat, um den Benutzer auf die deaktivierte Standortbestimmung hinzuweisen. Reagieren Sie entsprechend auf den neuen Status.
    -   Ihre App sollte außerdem erneut versuchen, Standortdaten abzurufen, wenn die Aktivierung erfolgt, wenn der Benutzer explizit versucht, eine Funktion mit erforderlichem Standortzugriff zu verwenden, oder zu jedem anderen dem Szenario angemessenen Zeitpunkt.

**Leistung**

-   Verwenden Sie einmalige Standortanforderungen, wenn Ihre App keine Positionsupdates empfangen muss. Beispielsweise muss eine App, die einem Foto ein Tag zum Standort hinzufügt, keine Positionsupdateereignisse empfangen. Sie sollte den Standort stattdessen wie unter [Abrufen der aktuellen Position](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location) beschrieben mit der [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)-Methode anfordern.

    Bei einer einmaligen Standortanforderung sollten Sie die folgenden Werte festlegen.

    -   Geben Sie die von Ihrer App angeforderte Genauigkeit durch Festlegen von [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) oder [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) an. Empfehlungen zur Verwendung dieser Parameter finden Sie weiter unten.
    -   Legen Sie mit dem Parameter „max age“ von [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) fest, wie lange das Abrufen eines Standorts bei Ihrer App zurückliegen kann. Kann Ihre App eine Position nutzen, die einige Sekunden oder Minuten alt ist, kann sie eine Position fast sofort empfangen und dadurch den Stromverbrauch des Geräts reduzieren.
    -   Legen Sie den Parameter „timeout“ von [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) fest. Diese Einstellung bestimmt, wie lange Ihre App auf eine Position warten kann, bevor ein Fehler zurückgegeben wird. Versuchen Sie, einen Kompromiss zwischen der vom Benutzer wahrgenommenen Reaktionsfähigkeit und der für Ihre App erforderlichen Genauigkeit zu finden.
-   Verwenden Sie eine fortlaufende Standortsitzung, wenn häufige Aktualisierungen der Position erforderlich sind. Verwenden Sie [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)- und [**statusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignisse, um Bewegungen zu erkennen, die über eine bestimmte Grenze hinausgehen, oder für fortlaufende Positionsupdates.

    Beim Anfordern von Positionsupdates können Sie die von Ihrer App angeforderte Genauigkeit mit [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) oder [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) angeben. Sie sollten auch die erforderliche Häufigkeit von Positionsupdates angeben (mit [**MovementThreshold**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) oder [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval).

    -   Geben Sie die Bewegungsgrenze an. Bei einigen Apps sind Positionsupdates nur erforderlich, wenn der Benutzer eine große Entfernung zurückgelegt hat. Eine App, die lokale Nachrichten oder aktuelle Wetterinformationen zur Verfügung stellt, benötigt beispielsweise keine Positionsupdates, es sei denn, der derzeitige Standort des Benutzers liegt in einer anderen Stadt. In diesem Fall sollten Sie die für ein Positionsupdateereignis mindestens erforderliche Entfernung anpassen, indem Sie die [**MovementThreshold**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.movementthreshold)-Eigenschaft festlegen. Dadurch werden [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignisse herausgefiltert. Diese Ereignisse werden dann nur ausgelöst, wenn die Änderung der Position die Bewegungsgrenze überschreitet.

    -   Verwenden Sie einen [**reportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval)-Wert, der Ihrer App-Funktion entspricht und die Nutzung von Systemressourcen minimiert. Beispielsweise kann bei einer Wetter-App eine Aktualisierung der Daten alle 15 Minuten ausreichen. Für die meisten Apps ist im Gegensatz zu Apps für die Echtzeitnavigation kein präziser, konstanter Datenstrom mit Positionsupdates erforderlich. Wenn für Ihre App kein möglichst präziser Datenstrom oder nur seltene Aktualisierungen erforderlich sind, legen Sie die **ReportInterval**-Eigenschaft fest, um die Mindesthäufigkeit der für die App erforderlichen Positionsupdates anzugeben. Die Standortquelle kann dann Energie sparen, indem der Standort nur bei Bedarf berechnet wird.

        Bei Apps, für die Echtzeitdaten erforderlich sind, sollte [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval) auf „0“ festgelegt werden. Damit geben Sie an, dass kein Mindestintervall festgelegt ist. Das standardmäßige Berichtsintervall lautet 1 Sekunde oder entspricht der durch die Hardware unterstützten Häufigkeit, je nachdem, was davon kürzer ist.

        Geräte, die Standortdaten bereitstellen, können das von verschiedenen Apps angeforderte Berichtsintervall nachverfolgen und Datenberichte im kürzesten angeforderten Intervall bereitstellen. Dann empfängt die App, die den höchsten Bedarf an Präzision hat, die benötigten Daten. Es ist deshalb möglich, dass die Positionssuche häufiger als von der App angefordert Updates generiert, falls eine andere App häufigere Updates angefordert hat.

        **Beachten Sie**  ist nicht garantiert, dass der Quellpfad für die Anforderung für des angegebenen Berichtsintervalls berücksichtigt wird. Nicht alle Geräte für die Positionssuche berücksichtigen das Berichtintervall, Sie sollten es jedoch dennoch angeben.

    -   Um Energie zu sparen, sollte die [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy)-Eigenschaft festgelegt werden. Damit wird angegeben, ob für die App präzise Daten erforderlich sind. Falls keine Apps präzise Daten benötigen, wird vom System Energie gespart, indem keine GPS-Anbieter aktiviert werden.

        -   Legen Sie [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) auf **HIGH** fest, damit das GPS Daten empfangen kann.
        -   Legen Sie [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) auf **Default** fest, und verwenden Sie ein einmaliges Aufrufmuster, um den Energieverbrauch zu minimieren, wenn Ihre App die Standortinformationen ausschließlich für gezielte Werbung verwendet.

        Falls für Ihre App besondere Anforderungen an die Genauigkeit gelten, können Sie ggf. die [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters)-Eigenschaft anstelle von [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) verwenden. Dies eignet sich besonders für Windows Phone, da die Position dort normalerweise anhand von Mobiltelefonbeacons, WiFi-Beacons und Satelliten abgerufen werden kann. Wenn Sie einen spezifischeren Genauigkeitswert auswählen, kann das System die geeigneten Technologien ermitteln, mit denen die Position zu den niedrigsten Energiekosten bereitgestellt werden kann.

        Zum Beispiel:

        -   Wenn Ihre App den Standort zum Abstimmen von Werbung, für Wettermeldungen, Nachrichten usw. abruft, ist eine Genauigkeit von 5000 m in der Regel ausreichend.
        -   Wenn Ihre app in der Nähe Angebote in der Nachbarschaft angezeigt wird, eignet sich eine Genauigkeit von 300 Verbrauchseinheit in der Regel um Ergebnisse bereitzustellen.
        -   Sucht der Besucher nach Restaurantempfehlungen in der Nähe, ist wahrscheinlich eine Position innerhalb eines Straßenblocks gewünscht, sodass eine Genauigkeit von 100 m genügt.
        -   Möchte der Benutzer seine Position teilen, sollte die App eine Genauigkeit von ungefähr 10 m anfordern.
    -   Verwenden Sie die [**Geocoordinate.accuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geocoordinate.accuracy)-Eigenschaft, wenn für Ihre App bestimmte Genauigkeitsanforderungen gelten. Navigations-Apps sollten z. B. mit der **Geocoordinate.accuracy**-Eigenschaft ermitteln, ob die verfügbaren Standortdaten die Anforderungen der App erfüllen.

-   Berücksichtigen Sie die Startverzögerung. Wenn die App zum ersten Mal Standortdaten anfordert, kann während des Startens des Anbieters eine kurze Verzögerung (1 – 2 Sekunden) auftreten. Berücksichtigen Sie dies beim Design der App-Benutzeroberfläche. Beispielsweise können Sie verhindern, dass andere Aufgaben bis zum Abschluss des Aufrufs von [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) blockiert werden.

-   Berücksichtigen Sie das Hintergrundverhalten. Falls die App inaktiv und im Hintergrund angehalten ist, empfängt sie keine Positionsupdateereignisse. Beachten Sie dies, wenn Ihre App Positionsupdates durch Protokollierung nachverfolgt. Wenn die App wieder aktiv ist, empfängt sie nur neue Ereignisse. Es werden keine Aktualisierungen abgerufen, die während ihrer Inaktivität stattfanden.

-   Verwenden Sie Rohdaten- und Fusionssensoren auf effiziente Art und Weise. Es gibt zwei Sensortypen: *Rohsensoren* und *Fusion-Sensoren*.

    -   Zu den Rohdatensensoren zählen Beschleunigungsmesser, Gyrometer und Magnetfeldmesser.
    -   Zu Fusionssensoren zählen Orientierungssensoren, Neigungsmesser und Kompasse. Fusionssensoren erhalten ihre Daten aus einer Kombination aus Rohdatensensoren.

    Die Windows-Runtime-APIs können alle diese Sensoren mit Ausnahme der Magnetometer zugreifen. Fusionssensoren sind genauer und stabiler als Rohdatensensoren, sie benötigen jedoch auch mehr Energie. Verwenden Sie die richtigen Sensoren für den richtigen Zweck. Weitere Informationen finden Sie unter [Sensoren](https://docs.microsoft.com/windows/uwp/devices-sensors/sensors).

**Verbundenen standby**
- Wenn sich der PC im Verbindungsstandby befindet, können [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekte immer instanziiert werden. Das **Geolocator**-Objekt findet jedoch keine zu aggregierenden Sensoren, sodass für Aufrufe von [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) nach 7 Sekunden ein Timeout auftritt. [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignislistener werden niemals aufgerufen, und [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignislistener werden einmal mit dem **NoData**-Status aufgerufen.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


### <a name="detecting-changes-in-location-settings"></a>Erkennen von Änderungen an den Standorteinstellungen

Der Benutzer kann die Standortbestimmung mit **Datenschutzeinstellungen für den Standort** in der **Einstellungs**-App deaktivieren.

-   Gehen Sie wie folgt vor, um das Aktivieren bzw. Deaktivieren der Standortbestimmung durch den Benutzer zu erkennen:
    -   Behandeln Sie das [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignis. Die [**Status**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.statuschangedeventargs.status)-Eigenschaft des Arguments für das **StatusChanged**-Ereignis weist den Wert **Disabled** auf, wenn der Benutzer die Standortdienste deaktiviert.
    -   Überprüfen Sie die von [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) zurückgegebenen Fehlercodes. Wenn der Benutzer die Standortdienste deaktiviert hat, Aufrufe von **GetGeopositionAsync** tritt ein Fehler mit einer **Zugriff\_"abgelehnt"** Fehler und die [ **LocationStatus** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.locationstatus) Eigenschaft hat den Wert **deaktiviert**.
-   Wenn Standortdaten für Ihre App unbedingt notwendig sind, beispielsweise in einer Karten-App, sollten Sie Folgendes sicherstellen:
    -   Behandeln Sie das [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignis, um Updates abzurufen, falls sich der Standort des Benutzers ändert.
    -   Behandeln Sie das [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignis wie oben beschrieben, um Änderungen an den Standorteinstellungen zu erkennen.

Beachten Sie, dass der Standortdienst Daten zurückgibt, sobald diese verfügbar sind. Zunächst wird unter Umständen ein Standort mit einem größeren Fehlerradius zurückgegeben. Dieser wird dann nach und nach mit exakteren Informationen aktualisiert, wenn diese zur Verfügung stehen. Apps, die den Standort des Benutzers anzeigen, aktualisieren den Standort in der Regel, wenn exaktere Informationen verfügbar werden.

### <a name="graphical-representations-of-location"></a>Grafische Standortdarstellungen

Ermöglichen Sie Ihrer App die Verwendung von [**Geocoordinate.accuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geocoordinate.accuracy), um den aktuellen Standort des Benutzers klar auf der Karte anzuzeigen. Es gibt drei Hauptbereiche für die Genauigkeit: einen Fehlerradius von ungefähr 10 Metern, einen Fehlerradius von ungefähr 100 Metern und einen Fehlerradius von mehr als einem Kilometer. Durch die Verwendung der Genauigkeitsinformationen können Sie sicherstellen, dass die App Standorte im Kontext der verfügbaren Daten präzise anzeigt. Allgemeine Informationen zur Verwendung des Kartensteuerelements finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps).

-   Bei einer Genauigkeit von ungefähr 10 Metern (GPS-Auflösung) kann der Standort durch einen Punkt oder eine Stecknadel auf der Karte angegeben werden. Bei dieser Genauigkeit können auch die Breiten- und Längengradkoordinaten und die Straße angezeigt werden.

    ![Beispiel für eine Karte mit einer GPS-Genauigkeit von ungefähr 10 Metern](images/10metererrorradius.png)

-   Bei einer Genauigkeit zwischen 10 und 500 Metern (ungefähr 100 Meter) wird der Standort üblicherweise durch WiFi-Auflösung empfangen. Der von einem Mobiltelefon abgerufene Standort hat eine Genauigkeit von ca. 300 m. In diesem Fall empfiehlt es sich, einen Fehlerradius in der App anzuzeigen. Bei Apps, die Wegbeschreibungen anzeigen, für die ein Zentrierpunkt erforderlich ist, kann ein solcher Punkt mit einem ihn umgebenden Fehlerradius angezeigt werden.

    ![Beispiel für eine Karte mit einer WiFi-Genauigkeit von ungefähr 100 Metern](images/100metererrorradius.png)

-   Wenn die zurückgegebene Genauigkeit einen Kilometer übersteigt, werden Standortinformationen vermutlich durch die Auflösung auf IP-Ebene empfangen. Dieser Grad an Genauigkeit ist häufig zu niedrig, um einen bestimmten Punkt auf einer Karte präzise anzuzeigen. Die App sollte auf die Stadtebene auf der Karte zoomen oder zum entsprechenden Bereich auf Grundlage des Fehlerradius (beispielsweise Regionsebene).

    ![Beispiel für eine Karte mit einer WiFi-Genauigkeit von ungefähr einem Kilometer](images/1000metererrorradius.png)

Wenn die Standortgenauigkeit von einem Genauigkeitsbereich zu einem anderen wechselt, sollte der Übergang zwischen den verschiedenen grafischen Darstellungen ansprechend sein. Dies kann folgendermaßen erfolgen:

-   Gestalten Sie die Übergangsanimation gleichmäßig. Der Übergang sollte schnell und flüssig erfolgen.
-   Warten Sie auf mehrere aufeinander folgende Meldungen, um die Änderung der Genauigkeit zu bestätigen und auf diese Weise unerwünschtes und zu häufiges Zoomen zu vermeiden.

### <a name="textual-representations-of-location"></a>Textliche Standortdarstellungen

Bei einigen Arten von Apps, beispielsweise Wetter-Apps oder Apps, die lokale Informationen bereitstellen, ist es nötig, den Standort für die verschiedenen Genauigkeitsbereiche in Textform darzustellen. Sorgen Sie dafür, dass der Standort deutlich und nur mit dem Grad an Genauigkeit angezeigt wird, der durch die Daten gestützt wird.

-   Bei einer Genauigkeit von ungefähr 10 Metern (GPS-Auflösung) sind die empfangenen Standortdaten ziemlich genau und können daher bis zur Ebene des Stadtteilnamens angegeben werden. Die Namen von Städten, Bundesländern und Ländern/Regionen können ebenfalls verwendet werden.
-   Bei einer Genauigkeit von ungefähr 100 Metern (WiFi-Auflösung) sind die empfangenen Standortdaten halbwegs genau, und es wird empfohlen, Informationen bis hin zur Ebene des Städtenamens anzugeben. Vermeiden Sie es, Stadtteilnamen anzugeben.
-   Zeigen Sie bei einer Genauigkeit von mehr als einem Kilometer (IP-Auflösung) nur das Bundesland oder den Namen des Landes/der Region an.

### <a name="privacy-considerations"></a>Überlegungen zum Datenschutz

Der geografische Standort eines Benutzers gehört zu den personenbezogenen Informationen (Personally Identifiable Information, PII). Die folgende Website enthält Richtlinien zum Datenschutz.

-   [Microsoft Privacy]( https://go.microsoft.com/fwlink/p/?LinkId=259692)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>Verwandte Themen

* [Einrichten von Geofence-Bereichen](https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)
* [Abrufen der aktuellen Position](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location)
* [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP-Ort-Beispiel (Geolocation)](https://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
