---
title: Erweiterte Xbox Live-Sandbox
description: Erfahren Sie, wie an Sandboxen zu verwenden, um Inhalt während der Entwicklung von verwalteten Partnern zu isolieren.
ms.assetid: bd8a2c51-2434-4cfe-8601-76b08321a658
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Xbox eine, xdk-Version, verwaltete Partner, Sandkasten, inhaltsisolation
ms.localizationpriority: medium
ms.openlocfilehash: 5e95999fc132e5dcda556120e9cf42f9c302f654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617145"
---
# <a name="advanced-xbox-live-sandboxes"></a>Erweiterte Xbox Live-Sandbox

> [!NOTE]
> In diesem Artikel wird erläutert, erweiterte Verwendung von Sandkästen und gilt hauptsächlich für große Spiele Studios die haben mehrere Teams und komplexe berechtigungsanforderungen.  Wenn Sie dem Xbox Live Creators-Programm gehören oder ID@Xbox Developer, es wird empfohlen, betrachten die [Xbox Live-Sandboxes Einführung](xbox-live-sandboxes.md)

Der Xbox Live *Sandbox* stellt eine gesamte private Umgebung für die Entwicklung bereit. In diesem Dokument wird erläutert, was Sandboxes sind, warum sie vorhanden sind, dies gilt für Verleger und ihre internen Xbox-Teams Auswirkung auf. Die Zielgruppe für dieses Dokument ist Herausgeber, die für die Xbox One Inhalt erstellen und verwenden Sie Sandboxes.

## <a name="isolate-content-on-xbox-live"></a>Isolieren Sie Inhalte auf Xbox Live

In Xbox Live ist nur eine einzelne produktionsumgebung, in dem alle Vorabversion ("in der Entwicklung" und "Beta"), Zertifizierungen und Retail Inhalt befindet.

Inhaltsisolation ist die Möglichkeit, sicherzustellen, dass keine Content Publisher-Speicherverluste in der produktionsumgebung vorhanden sind. Im Grunde wird Sie inhaltsisolation sichergestellt, dass alle prinzipalbenutzer, Gerät oder Titel, der über die Berechtigung zum Zugriff auf eine Ressource (einen Titel oder einen Dienst) fordert Zugriff auf die Ressource autorisiert wurde. Mit der Content-Isolation werden Partitionen in Sandboxes unterteilt, in denen Titel oder der Dienst Daten gespeichert werden. In anderen Worten, werden im Rahmen einer Sandbox Autorisierungsrichtlinien definiert werden.

Sandboxes sind eine Möglichkeit zum Partitionieren von Daten, die in der Produktion befindet. Mit Xbox 360 Era-Diensten sind PartnerNet und ProductionNet über zwei verschiedene Umgebungen. Mit der Xbox One Era-Services, eine einzelnen produktionsumgebung enthält *n* unterschiedliche virtuelle Umgebungen, in denen jede virtuelle Umgebung wird eine Sandbox aufgerufen. Da für den gesamten Inhalt eine einzelnen produktionsumgebung wird sind Sandboxes tatsächlich eindeutige virtuelle Umgebungen, in denen Daten, die in einer Umgebung generiert über überschreiten können nicht, aus.

Die folgende Abbildung zeigt eine einzelne produktionsumgebung, in der Herausgeber ihre privaten Entwicklung Sandkästen erstellen können. Nur autorisierte Dev-Konten oder Developer Kits dürfen Zugriff auf diese Sandboxes.

Abbildung 1. Sandboxen in einer produktionsumgebung.

![](images/sandboxes/sandboxes_image1.png)

Genau wie PublisherA ihre Entwicklung Sandboxes verfügt, haben anderer Herausgeber ihre eigenen Sandboxes Entwicklung. Die Daten, die für die Title-ID generiert gehen über Sandboxes unterscheidet sich aber die gleiche Title-ID befindet sich möglicherweise im unterschiedliche Sandboxes.

Es gibt zwei System-Sandboxes, die nur von Microsoft aufgefüllt werden können: CERT "und" RETAIL ". Wie die Namen nahe legen, ist die CERT-Sandbox für Titel, die Zertifizierung vor der Freigabe zwar die RETAIL-Sandbox der Sandbox, die echte Dollar darstellt, die von allen Retail-Benutzern und Geräten zugänglich ist, durchlaufen.

Während eine Title-ID in Xbox Live früher eindeutig war, jetzt eine Title-ID sowie einen Sandkasten-ID ist eindeutig. Das gleiche gilt für die Produkt-IDs und andere ID-Leerzeichen, die einmal als eindeutig behandelt wurden. Sie müssen nun mit einem Sandkasten-ID kombiniert werden Alle Daten in Xbox Live werden in erster Linie durch die Sandkasten-ID im gesamten System partitioniert werden.

## <a name="initial-setup-for-a-title"></a>Die ersteinrichtung eines Titels

Ein Titel ist in der Xbox-Entwickler-Portal (XDP) oder im Partner Center geboren. Dieses Dokument behandelt die Titel XDP geboren. Titel werden ein Titel-ID, Produkt-ID und eine ID (SCID)-Konfiguration zugewiesen.

In dieser neuen Welt einen Titel oder einen eigenen Produkt nichts zu Xbox Live, bedeutet nicht. Da wir gleichzeitige Einzelhandels- und Entwicklung mithilfe eines einzelnen Titels unterstützen muss, müssen wir unterstützen *Instanziierung* von Titeln, um die Stellen, und behalten die erforderlichen Unterschiede. Eine Instanz eines Titels befindet sich in einer Sandbox und dies kommen Sandboxes ins Spiel.

Verleger um einen Titel für XDP zu erstellen, erstellt eine Produktgruppe, gibt an, das Genre für die Produktgruppe und erstellt dann die einzelnen Produkte darin. (Weitere Informationen finden Sie in der Dokumentation XDP.) Das folgende Diagramm veranschaulicht die Beziehungen zwischen eine Produktgruppe, ein Produkt, eine Product-Instanz und einer Sandbox.

Abbildung 2. Die Beziehungen zwischen eine Produktgruppe, ein Produkt, eine Product-Instanz und einer Sandbox.

![](images/sandboxes/sandboxes_image2.png)

<a name="product-instances"></a>Product-Instanzen
-----------------
Ein *Product-Instanz* ist eine Projektion der Titel, Produkt und Konfiguration von Daten in einem bestimmten Sandkasten. Diese Daten werden in den folgenden drei Bereichen beschrieben:-Konfiguration, Katalogmetadaten und Binärdateien.

### <a name="service-configuration"></a>Dienstkonfiguration

> Konfiguration Dienstdefinitionen (Ereignisse, Statistiken, Erfolge, usw.). Die Dienstkonfiguration wird auf der Ebene der Product-Instanz definiert.

### <a name="catalog-metadata"></a>Katalogmetadaten

> Die Metadaten, die im Katalog, einschließlich Text von Selling, Grafikobjekte, Verfügbarkeit/Angebotsinformationen, Lizenzierung und weitere Daten befinden.

### <a name="binary"></a>Binär

> Eine Binärdatei kann auf zwei Arten dargestellt werden:

1.  Metadaten nur für das querladen zu aktivieren. Dies schließt die Inhalts-ID, Informationen zu Version und Informationen zur Lizenzierung.

2.  Vollständige Binärdatei weitergegeben, zu einem CDN und an einen Client zum Herunterladen.

<a name="getting-the-access-right"></a>Abrufen der richtigen
------------------------
Es gibt zwei unterschiedliche Arten von Zugriff auf Ihre Inhalte in Deiner Xbox One:

*Design-Time-Zugriff*– Zugriff auf einem PC über das Tool XDP – können Personen, die an Ihren Produkten arbeiten, hochladen, organisieren und Arbeiten mit Inhalts-, Konfigurations- und Metadaten, aber nicht ermöglicht ihnen ausführen oder Instanzen von Ihren Produkten wiedergeben.

*Run-Time-Zugriff*– den Zugriff von einer Xbox-Konsole – können Sie Ihre Entwickler, Tester, Prüfer und schließlich Ihre Kunden ausführen und Produktinstanzen wiedergeben.

> [!NOTE]
> Um für die Run-Time-Zugriff verfügbar zu sein, muss eine Product-Instanz in einer Sandbox platziert werden. Wenn ein Build in einer Sandbox abgelegt ist, können XDP Benutzer oder Devkit-Geräte, die Zugriff auf diese Sandbox gewährt wurden der Instanz ausführen. Zu diesem Zweck Anmeldung bei Xbox One über eine Xbox-Konsole, über ein Konto Dev – spezielle Konten diese Funktion als virtuelle Benutzer für die Run-Time-Zugriff.

Wenn wir Sandboxes sprechen, in der Regel sprechen wir über Run-Time-Zugriff auf Inhalte, die auf Xbox Live ausgeführt wird. Um einen Dienst in Xbox Live zugreifen zu können, ist eine Title-ID erforderlich. Nach einer **Appxmanifest** enthält eine Title-ID, die Konsole sendet die ID der Titel zu Xbox Live. Xbox Live-Sicherheitsdienste stellt keine wieder ein gültiges Token bereit, es sei denn, die der Prinzipal (Geräte oder Benutzer) Zugriff auf den Titel gewährt wurde.

Dieser Überprüfungsvorgang ist der Kern der Content-Isolation. Wenn auf einer sehr hohen Ebene angezeigt wird:

-   Eine Dienstprinzipale Gruppe kann es sich um Xbox-Benutzer-IDs (XUIDs), Geräte-IDs, Titel, IDs oder Dienst-IDs enthalten.

-   Eine Sandbox kann Titel, IDs, die Produkt-IDs oder Service-Konfigurations-IDs (SCIDs) enthalten.

-   Ein leitender Group wird Zugriff auf einen Sandkasten gewährt.

Daher muss für einen Benutzer oder ein Gerät auf einer Vorabversion Titel in einer Sandbox, den Zugriff über XDP zunächst erteilt werden.

Abbildung 3. Ein Modell für den Zugriff über XDP einrichten.

![](images/sandboxes/sandboxes_image3.png)

Die Effektivität der inhaltsisolation ist basiert auf der Tatsache, dass Ihre Organisation die folgenden Prozesse besitzt:

-   Erstellen Ihre XDP-Benutzerkonten, die Dev-Konten, die jeden Benutzer verwenden, melden Sie sich für die Run-Time-Zugriff und die Benutzergruppen aus, in denen jeder Benutzer die Mitgliedschaft gewährt wird.

-   Erstellen Gerätegruppen vertrauenswürdigen Konsolen.

-   Angeben aller Ihrer Entwicklung Sandboxes genau die Benutzer- und Gerätegruppen in ihr Zugriff auf die Produktinstanzen haben.

Ein Beispiel für dieses Setup wird in der folgenden Abbildung veranschaulicht.

Abbildung 4 Ein nicht autorisierter Benutzer Anmeldeinformationen fehl für den Zugriff auf die Sandbox, wie die normalen Anmeldeinformationen von einem autorisierten XDP-Benutzerkonto. Nur die Anmeldeinformationen des Kontos das autorisierte XDP Benutzerkonto, das im Besitz Dev erfolgreich in die Run-Time-Zugriff mit der Sandbox und für alle derzeit in der Product-Instanzen.

![](images/sandboxes/sandboxes_image4.png)

### <a name="dev-accounts-setup"></a>Installation des Dev-Konten

In Xbox One Dev-Konten sind nur standard-Microsoft-Konten (MSA), spezielle Regeln angewendet werden. Sie werden in Xbox Live für die Entwicklung verwendet werden. Ein Entwicklerkonto:

-   Muss von XDP oder über Partner Center erstellt werden.

-   Die Rolle der externen Entwickler beim Erstellen von Herausgebern wird zugewiesen werden.

-   Ist gebunden an die XDP-Konto oder das Partner Center-Konto, das das Entwicklerkonto erstellt haben.

-   Können nur zu Developer Kits anmelden. Anmeldung wird ein Entwicklerkonto für im Einzelhandel Geräte verweigert.

-   Können Xbox Live Developer Gold-Abonnements oder anderen Abonnements kostenlos erwerben, um zu testen.

### <a name="user-group-setup"></a>Benutzereinrichtung für die Gruppe

Eine Benutzergruppe, die erste Art von Gruppe "principal" ist eine Sammlung von XDP Benutzern. Wenn Sie Benutzergruppen XDP Benutzer hinzugefügt werden, übertragen ihre Dev-Konten für diese Benutzer XDP.

Also wenn eine Benutzergruppe aus einer Sandbox zugeordnet wird, die Dev-Konten den XDP Benutzern zugeordnet, entsprechende prinzipalgruppen Benutzergruppe hinzugefügt, und die prinzipalgruppen erhalten eine Einrichtung mit primären Ressourcensatz die Sandbox sichern.

> [!NOTE]
> Die Benutzergruppen, die erstellt werden, für den Zugriff auf Sandboxes sind die gleichen Benutzergruppen, die verwendet werden, um den Zugriff auf Konfigurationsdaten in XDP für Produktgruppen und -Produkte zu verhindern.

### <a name="device-setup"></a>Geräte-Setup

Ein Gerät wird auch ein leitender Group hinzugefügt. Ein Gerät kann nur als ein Dev-Kit verwendet werden, wenn eine Berechtigung ist über das Spiel Developer Store erworben wurden, und das Gerät ist ein Entwicklungskit werden bereitgestellt. Nachdem ein Gerät als ein Dev-Kit bereitgestellt wurde, wird das Gerät in der Liste der Geräte, die die Device-Gruppen hinzugefügt werden können.

### <a name="device-group-setup"></a>Gruppe der Geräteinstallation

Eine Gerätegruppe aus, die zweite Art der Gruppe "Hauptbenutzer" kann auch den Zugriff an Sandboxen angegeben werden. Das Setup ähnelt dem oben beschriebenen Einrichtung der Benutzer zur Verfügung.

## <a name="sandboxes"></a>Sandboxes

<a name="what-is-a-sandbox"></a>Was ist eine Sandbox?
------------------
Einfach erwähnt *eine Sandbox ist eine Möglichkeit zum Partitionieren von Daten in der Produktion*.

<a name="why-do-we-need-sandboxes"></a>Warum brauchen wir Sandboxes?
-------------------------
Genau wie Benutzern und Geräten Titel zuzugreifen, greifen auf Titel. Eine Einführung in ein Konzept der "Titel Group", in dem Gruppen von Titeln Zugriff auf Service-Ressourcen gewährt werden.

Da ist eine einzelnen produktionsumgebung für Xbox One für den gesamten Inhalt ("Vorabversion" und "Retail"), müssen mehrere Instanzen (pre-release/Einzelhandel) einen Titel für die auf denselben Instanzen der Ressourcen verhindert werden.

<a name="what-is-in-a-sandbox"></a>Was ist in einer Sandbox?
---------------------
Eine Sandbox enthält eine Product-Instanz, bei jedem Titel, die mit der Sandbox hinzugefügt wird.

<a name="what-is-a-sandbox-id"></a>Was ist ein Sandkasten-ID?
---------------------
Sandkasten-ID ist eine Partitionierung Dateneinheit für Titel, Produkt oder eine Dienstkonfiguration. Mehrere Titel können in der gleichen Sandbox, vorhanden sein, die Voraussetzung für die durch die alle Konfigurationsdaten Dienst zugänglich ist.

Eine Sandbox-ID (Groß-/ Kleinschreibung) ist eine Zeichenfolge im folgenden Format an: &lt;PublisherMoniker&gt;.*n*. Eine Beispiel für Sandkasten-ID, XLDP.5, wird unten erläutert:

-   Die *Verleger Moniker* ist für alle Verleger eindeutig. "XLPD" ist der Moniker Herausgeber für diesen bestimmten Verleger. Ein Moniker ist Herausgeber wird erstellt, wenn ein Verleger "in XDP von der Developer-Konto-Manager aktiviert ist".

-   Die Ziffer *"n"* gibt die Anzahl von der Sandbox. "5" in diesem Fall ist die sechste Sandbox, die für diesen Verleger erstellt.

Wechselt die Titeldaten über Dienste, verwenden Xbox-Dienste die Sandkasten-ID zur eindeutigen Identifizierung der "Environment" für die Daten, die generiert wird.

<a name="what-data-is-sandboxed"></a>Welche Daten als sandkastenlösung bereitgestellt werden?
-----------------------
Das folgende Diagramm zeigt, welche Benutzer und Titel Daten als sandkastenlösung bereitgestellt werden.

![](images/sandboxes/sandboxes_image5.png)

<a name="global-override-sandbox"></a>Globale Überschreibung statt sandbox
-----------------------
Ein Entwickler legt die Sandkasten-ID auf ihr Dev-Kit und somit die Sandbox, die das Entwicklungskit ausgeführt, im wird; Dies ist auch bekannt als der Sandbox globale Überschreibung statt. Daher alle Anforderungen an Xbox Live-Dienste (z. B. Erfolge, Vermittlung, Lizenzierung, EDS usw.) aus den Titel (Shell-apps und herkömmlichen apps) in der Entwickler Kit in diese Sandbox vorgenommen werden.

Die globale Überschreibung statt Sandbox bedeutet auch, dass nur die Inhalte, die in der Sandbox globale Überschreibung statt erfasst angezeigt wird, wenn durchsucht wird.

<a name="types-of-sandboxes"></a>Arten von Sandkästen
-------------------------------------------------------------------------------------------------------------------------------------------------------
Es gibt zwei verschiedene Kategorien von Sandkästen. Diese Kategorien sind folgendermaßen definiert:

-   *Verleger-Sandboxes*. Herausgeber müssen Zugriff auf ihre Sandkästen in der Entwicklung. Dies sieht möglicherweise wie XLDP.0, XLDP.1, XLDP.2, XLDP.3 usw. Dies ist das, wo der Herausgeber ihre Titel Produktinstanzen platziert würde. Zugriff auf diese Sandboxes ist für die Benutzer/Geräte gated, die Zugriff der Verleger erteilt

-   *Microsoft-Sandboxes*. Hierbei handelt es sich um den integrierten Sandkästen: Einzelhandel und Zertifikat nur Microsoft kann auf diese geschützten Sandboxes veröffentlichen.

<a name="cert-sandbox"></a>CERT-sandbox
------------
Wenn Sie ein Titel für die allgemeine Verfügbarkeit bereit ist, muss sie zuerst die Zertifizierung durchlaufen. Die Zertifikat-Sandbox ist eine Kontrolle von Microsoft-Sandbox, der nur Personen in Zertifizierung zugreifen zu können. Herausgeber können sehen, welche Inhalte sie eigene Zertifizierung durchlaufen wird.

Alle Produktinstanzen, die nicht in der Zertifizierungsstelle können wieder hochgefahren werden, um eine Sandbox Entwicklung gedebuggt und von den Herausgebern mit XDP oder Partner Center behoben werden.

<a name="retail-sandbox"></a>Einzelhandel-sandbox
--------------
Die RETAIL-Sandbox ist das endgültige Ziel für den gesamten Inhalt, die für Xbox One erstellt wird.

Nachdem Sie ein Titel Zertifizierung erfolgreich ist, wird sie mit der Sandbox RETAIL hinzugefügt. Nur das grüne signierten Inhalt ist in der Sandbox RETAIL ausgeführt werden darf. Dies hat eine wichtige Auswirkung, die Herausgeber-driven Betaversionen auch in der Sandbox im Einzelhandel durchgeführt werden. Daten, die in der Sandbox RETAIL generiert stellt Produktionsdaten realen Kunden dar.

Beachten Sie, dass Zugriff auf den Inhalt der RETAIL-Sandbox ist immer noch über inhaltsisolation steuerbar.

Beispielsweise werden Betaversionen Publisher-driven ausgeführt in der Sandbox RETAIL auswählt, in dem der Verleger Herausgeber definierter Beta Resource Set Titel den Zugriff auf welche prinzipalgruppen zu erhalten. Die Dienstdaten generiert, indem Sie die Beta-Titel ist echte Daten prod und weiterhin vorhanden, nachdem Sie der Titel zur allgemeinen Verfügbarkeit wechselt.

<a name="cross-sandbox-data-interaction"></a>Cross-Sandbox die dateninteraktion
------------------------------
Eine Sandbox ist ein Container, der die Datenfreigabe beschränkt. Cross-Sandbox die dateninteraktion ist daher nicht möglich.

## <a name="organizing-your-sandboxes"></a>Organisieren Ihre Sandkästen

Dieser Abschnitt enthält ein Beispiel, wie ein Verleger Sandboxes organisieren kann. Ein Herausgeber muss wissen, wie an Sandboxen verwenden, um Daten zu organisieren.  

Die Beispiele unten Laufzeitzugriff Management mit inhaltsisolation nur anzeigen.

### <a name="scenario-1-two-titles-one-sandbox"></a>Szenario 1: Zwei Titel, eine sandbox

Die grundlegende Struktur für einen Herausgeber kann Folgendes sein:

-   Zwei Titel, die für alle Benutzer und Geräte im Besitz des Verlegers für die Entwurfszeit und Laufzeit zugegriffen werden kann.

-   Eine Product-Instanz pro Titel.

In diesem Fall ist der Verleger nur einen einzigen Sandkasten für den gesamten Inhalt der Vorabversion erforderlich.

Das folgende Diagramm zeigt eine Benutzergruppe. Der Verleger kann entscheiden, eine Gerätegruppe anstelle einer Benutzergruppe zu verwenden, wenn einfacher erachtet. Außerdem hat diese Benutzergruppe von folgender Laufzeit und Entwurfszeit-Zugriff auf Sandbox XLDP.1 und den Titel, in diese Sandbox.

![](images/sandboxes/sandboxes_image6.png)

### <a name="scenario-2-one-title-different-teams"></a>Szenario 2: Ein Titel zulässig, verschiedene teams

In diesem Modell sind die Anforderungen:

-   Ein Titel zulässig.

-   Dev-Team arbeitet an täglichen Builds.

-   QA-Team arbeitet an wöchentlichen LKGs.

-   Entwicklungsteam muss wöchentlich LKGs bei einem Fehler zu debuggen.

-   Dem finanzteam benötigt Zugriff auf den Preis-Karten und andere Metadaten, die im Zusammenhang mit der Catalog-Version eines Titels.

Die folgende Abbildung zeigt, dass TitleX zwei Produktinstanzen verfügt: PI-1 und PI-2. Eine Product-Instanz in einer Sandbox sein muss und zwei Produktinstanzen von denselben Titel darf nicht in der gleichen Sandbox sein. Daher TitleX-PI-1 ist, in der Sandbox XLDP.1 und TitleX-PI-2 ist in der Sandbox XLDP.2.

Die Dev-Benutzergruppe hat Zugriff auf beide Sandboxes, während die QA-Benutzergruppe Zugriff auf nur Sandbox XLDP.2 hat.

Darüber hinaus hat es sich bei der Finance-Benutzer (Gruppe C) um Design-Time-Zugriff auf TitleX. Da die Finance-Gruppe für Benutzer keine in der Regel werden alle Run-Time-Debuggen eines Titels, aber sie verteilt.

> [!NOTE]
> Unabhängig von der Organisation kann ein Benutzer XDP in mehr als eine Benutzergruppe angehören.

![](images/sandboxes/sandboxes_image7.png)

### <a name="scenario-3-two-titles-completely-separate"></a>Szenario 3: Zwei Titel, vollständig getrennt

In diesem Beispiel ändern die Anforderungen ein wenig:

-   Zwei Titel.

-   Zugriff auf jeden einzelnen Titel sollte auf einen bestimmten Satz von Personen beschränkt werden.

-   Eine Product-Instanz pro Titel.

-   Ein Admin-Benutzergruppe Zugriff auf während der Entwurfszeit XDP-Konfigurationsdaten für die Titel benötigt. Die Personen in dieser Gruppe werden alle Administratoren für den Verleger und steuern alle Daten, die im Katalog (Katalogmetadaten, Finanzen, marketing, Zertifizierung Übermittlung usw.) veröffentlicht wird

In diesem Modell hat der Verleger zu beiden Titel vollständig getrennt und somit zugewiesen diese zwei Titel in zwei unterschiedliche Sandboxes ausgewählt. Der Verleger hat auch eine separate Admin-Benutzergruppe erstellen möchten und Zugriff auf den beiden Produkten zugewiesen.

![](images/sandboxes/sandboxes_image8.png)

### <a name="scenario-4-anyway-you-like-it"></a>Szenario 4: Trotzdem sind wie Sie es

Aufgrund der Anzahl von Verbindungen und die Sprache kurz zu halten haben wir uns entschieden, um nur die Sandbox Laufzeitverzeichnisse anzuzeigen. "Nothing" verhindert, dass Sie auch andere Berechtigungen für Design-Time-Zugriff hinzufügen.

In diesem Beispiel sind die Anforderungen:

-   Nur bestimmte Personen haben Zugriff auf bestimmte Titel innerhalb der Herausgeber.

-   Der Verleger arbeitet mit Herstellern aus verschiedenen Unternehmen zuständig und diese Anbieter sind möglicherweise kurzfristige.

-   Der Verleger sollte können außer Betrieb setzen einen Titel ein, und auf diese Weise verhindern Zugriff auf alle Daten, denen der Anbieter oder VBE zugreifen konnte.

Um diese Anforderung zu modellieren, kann eine Struktur wie z. B. folgendermaßen übernommen werden.

Das Modell unter gefolgt wird:

-   TitleX und TitleY haben nur jeweils eine produktinstanz in Sandkasten XLDP.1 ein.

-   TitleZ verfügt über zwei Product-Instanzen, in der Sandbox XLDP.2 und in der Sandbox XLDP.3.

-   FTE Benutzer Gruppe B erhält Zugriff auf Produktinstanzen in allen Sandboxes.

-   Der Benutzergruppe ein Anbieter ist eine nur-Anbieter-Benutzergruppe, die Zugriff auf die Sandbox XLDP.1 gewährt wird.

-   Hersteller Gerät Gruppe C ist eine reine Hersteller, die Zugriff auf die Sandbox XLDP.3 gewährt wird.

![](images/sandboxes/sandboxes_image9.png)

## <a name="determine-the-sandbox-your-device-is-targeting"></a>Bestimmen der Sandbox, die Ihr Gerät abzielt

Die Xbox Live-APIs enthalten ein app-Config-Singleton, mit denen Sie welche Sandbox finden Sie unter dem Titel des als Ziel zur Laufzeit verwendet wird. Dies erfolgt durch den Zugriff auf die **Sandbox** Eigenschaft `xbox::services::xbox_live_app_config`.

C++ XDK
```cpp
auto appConfig = xbox::services::xbox_live_app_config::get_app_config_singleton();
string_t sandbox = appConfig->sandbox;
```

C#WinRT
```csharp
XboxLiveAppConfiguration appConfig = XboxLiveAppConfiguration.SingletonInstance;
string sandbox = appConfig.Sandbox;
```

> [!NOTE]
> Die Sandbox-Eigenschaft ist einen Wert nicht erhält, bis ein Benutzer angemeldet ist.

## <a name="summary"></a>Zusammenfassung

Xbox Live-Entwicklung bietet hervorragende Gelegenheit Verleger in der Produktion mit Produktionsqualität-Dienste und MSA-entwicklerkonten Produktion testen. Die Erhöhung der Funktionalität und Flexibilität erfordert neue Konfigurationsschritte in XDP aus, um Daten zu Softwaretiteln erstellen und Verwalten des Zugriffs auf die Titel und bei der Entwicklung und allgemein verfügbar.

Sandboxes sind eine Möglichkeit zum Partitionieren von Daten in der Produktion. Da ist für den gesamten Inhalt eine einzelnen produktionsumgebung, fungieren als "virtual Environments", in denen Daten, die in einer Umgebung generiert werden. nicht mit anderen überschneiden ist, Sandboxes.
