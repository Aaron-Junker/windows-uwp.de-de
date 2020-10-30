---
description: Sie können Aktionscodes für eine APP oder ein Add-on generieren, die Sie im Microsoft Store veröffentlicht haben.
title: Generieren von Werbecodes
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Promotioncode, Aktionscodes, Token, Token
ms.localizationpriority: medium
ms.openlocfilehash: b9640ad9ddc00741677b8a1341c12d0489564368
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029643"
---
# <a name="generate-promotional-codes"></a>Generieren von Werbecodes


[Partner Center](https://partner.microsoft.com/dashboard) ermöglicht es Ihnen, Werbecodes für eine APP oder ein Add-on zu generieren, die Sie im Microsoft Store veröffentlicht haben. Werbecodes sind eine einfache Möglichkeit, einflussreichen Benutzern kostenlosen Zugriff auf Ihre App oder Ihr Add-On zu ermöglichen. Sie können Werbecodes auch in Kundendienstszenarien verwenden, indem Sie Benutzern kostenlosen Zugriff auf Ihre App oder Ihr Add-On gewähren, oder für [Betatests](beta-testing-and-targeted-distribution.md) mit Windows 10. 

Jeder Werbe Code verfügt über eine entsprechende eindeutige einlösbare URL, auf die ein Kunde klicken kann, um den Code zu einlösen und Ihre APP oder das Add-on aus dem Microsoft Store zu installieren.  Beachten Sie, dass Ihre APP die letzte Veröffentlichungs Phase des [Zertifizierungs Vorgangs der APP](the-app-certification-process.md) bestehen muss, damit Kunden einen Werbe Code einlösen können, um Sie zu installieren.

Sie können einzelne Verwendungs Codes generieren (und eine an jeden Kunden verteilen), oder Sie können einen Code generieren, der mehrmals von einer bestimmten Anzahl von Kunden verwendet werden kann.

> [!TIP]
> Sie können [gezielte Pushbenachrichtigungen](send-push-notifications-to-your-apps-customers.md) verwenden, um einen Werbe Code an ein Segment ihrer Kunden zu verteilen. Stellen Sie dabei sicher, dass Sie einen Werbe Code verwenden, der es mehreren Kunden ermöglicht, denselben Code zu verwenden.


## <a name="promotional-code-policies"></a>Richtlinien für Werbecodes

Beachten Sie die folgenden Richtlinien für Werbecodes:

-   Sie können Werbecodes für jede APP oder jedes Add-on generieren (mit Ausnahme von Add-ons für Abonnements), die Sie auf dem Microsoft Store veröffentlicht haben. Kunden können die Codes für jede Windows-Version einlösen, die von Ihrer APP oder Ihrem Add-on unterstützt wird.
-   Für Spiele:
    - Sie können bis zu 5000 Aktionscodes pro Spiel generieren.
    - Die für Spiele generierten Aktionscodes laufen nie ab.
- Für alle anderen Arten von apps oder Add-ons:
    - In einem Zeitraum von sechs Monaten können Sie bis zu 1600 Aktionscodes für die Einzelverwendung oder eine beliebige Anzahl von mehreren Nutzungs Codes generieren, sodass die zulässige Gesamtanzahl nicht länger als 1600 ist.
    - Der 6-monatige Zeitraum beginnt, wenn Sie generieren, dass der erste Aktions Code erstellt wird und sechs Monate lang dauert, unabhängig davon, ob Sie ein älteres Ablaufdatum für die Codes festgelegt haben.
    - Alle Codes, die während eines bestehenden Zeitraums von sechs Monaten erstellt werden, werden in Bezug auf die Anzahl der Codes gezählt, die innerhalb dieses Zeitraums generiert werden. Dies gilt auch, wenn Sie nach Ablauf des Zeitraums ablaufen (z. b. Wenn Sie am letzten Tag des sechsmonatigen Fensters einen Code generieren).
-   Sie müssen die in der [Vereinbarung für App-Entwickler](/legal/windows/agreements/app-developer-agreement) definierten Anforderungen erfüllen, einschließlich Abschnitt **3k. Werbecodes** .

> [!NOTE]
> Sie können Aktionscodes auch dann verwenden, wenn Ihre APP für Kunden nicht verfügbar ist (d. h. Wenn Sie die Option **dieses Produkt verfügbar machen, aber nicht im Store** mit der Beendigung-Übernahme auffindbar ausgewählt haben **: jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts anzeigen, ihn aber nur herunterladen, wenn er das Produkt bereits besitzt, oder einen Werbe Code haben und eine Windows 10-Geräte** Option im Abschnitt zur [Auffindbarkeit](choose-visibility-options.md#discoverability) ihrer Übermittlung verwenden. Mit dieser Option müssen sich Kunden auf Windows 10 (einschließlich Xbox) befinden, um Ihr Produkt mit einem Werbe Code zu erhalten.


## <a name="order-promotional-codes"></a>Bestellen von Werbecodes

So ordnen Sie Aktionscodes für eine APP oder ein Add-on an:

1.  Erweitern Sie im linken Navigationsmenü von [Partner Center](https://partner.microsoft.com/dashboard)die Option **anziehen** , und wählen Sie dann Aktions **Codes** aus.

2.   Klicken Sie auf der Seite **Werbecodes** auf **Codes bestellen** .

3.  Geben Sie auf der Seite **Neue Werbecodes bestellen** Folgendes ein:
    -   Wählen Sie die Apps oder Add-Ons, für die Sie Codes generieren möchten. (Beachten Sie, dass Sie keine Aktionscodes für Add-ons von Abonnements generieren können.)
    -   Geben Sie einen Namen für die Bestellung an. Anhand dieses Namens können Sie beim Überprüfen der Werbecode-Nutzungsdaten zwischen verschiedenen Codebestellungen unterscheiden.
    -   Wählen Sie den Auftragstyp. Sie können eine Reihe von Aktionscodes generieren, die jeweils einmal verwendet werden können, oder Sie können einen Aktions Code generieren, der mehrmals verwendet werden kann.
    -   Geben Sie die Anzahl der zu sortierenden Codes (bei der Erstellung eines Satzes von Codes) oder die Häufigkeit an, mit der der Code eingelöst werden kann (wenn ein Code mehrmals erstellt werden soll).
    -   Geben Sie an, wann die Werbecodes aktiv werden sollen. Um ein spezifisches Startdatum und eine spezifische Startuhrzeit zu wählen, entfernen Sie die Markierung für das Kontrollkästchen **Codes werden umgehend aktiv** . Andernfalls werden die Codes sofort aktiviert (obwohl das Produkt den Veröffentlichungs Vorgang abgeschlossen haben muss, damit ein Kunde den Code verwenden kann).
    -   Geben Sie an, wann die Werbecodes ablaufen sollen. Deaktivieren Sie das Kontrollkästchen **Codes laufen nach 6 Monaten ab** , um ein bestimmtes Ablaufdatum und eine bestimmte Uhrzeit als 6 Monate auszuwählen.

4.  Klicken Sie auf **Codes bestellen** . Anschließend werden Sie auf die Seite " **Aktionscodes** " zurückkehren, auf der Sie Ihre neue Bestellung in der Zusammenfassungs Tabelle der Werbe Code Bestellungen für diese APP sehen können.


## <a name="download-and-distribute-promotional-codes"></a>Herunterladen und Verteilen von Werbecodes

So laden Sie eine erfüllte Werbe Code Bestellung herunter und verteilen die Codes an Kunden:

1.  Erweitern Sie im linken Navigationsmenü von [Partner Center](https://partner.microsoft.com/dashboard)die Option **anziehen** , und wählen Sie dann Aktions **Codes aus.**
2.  Klicken Sie auf den **Download** Link für die Aktions Code Bestellung, und speichern Sie die generierte Datei auf Ihrem Computer. Diese Datei enthält Informationen zur Reihenfolge ihrer Aktionscodes im durch Tabstopps getrennten Format (. TSV).
3.  Öffnen Sie die TSV-Datei im Editor Ihrer Wahl. Um optimale Ergebnisse zu erzielen, öffnen Sie die TSV-Datei in einer Anwendung, die die Daten in einer tabellarischen Struktur, z. b. Microsoft Excel, anzeigen kann. Allerdings können Sie die Datei in einem beliebigen Text-Editor öffnen.

    Die Datei enthält Spalten mit den folgenden Daten für jeden Code:

    -   **Produktname** : Der Name der App oder des Add-Ons, dem der Code zugeordnet ist.
    -   **Order Name** : der Name der Reihenfolge, in der der Code generiert wurde.
    -   **Werbecode** : Der Code selbst. Dies ist eine 5x5-Zeichenfolge von alphanumerischen Zeichen, die durch Bindestriche getrennt sind. Beispiel: DM3GY-M2GYM-6ymw6-4qhht-23w2z
    -   **Einsetzbare URL** : die URL, die ein Kunde verwenden kann, um den Code einzulösen und Ihre APP oder Ihr Add-on zu installieren. Die URL weist das folgende Format auf: https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt ;p romotional_code>
    -   **Start Datum** : das Datum, an dem dieser Code aktiv wurde.
    -   **Ablaufdatum** : Das Datum, an dem dieser Code abläuft.
    -   **Code-ID** : Eine eindeutige ID für diesen Code.
    -   **Bestell-ID** : eine eindeutige ID für die Reihenfolge, in der dieser Code generiert wurde.
    -   **An** : ein leeres Feld, das Sie verwenden können, um nachzuverfolgen, welchem Kunden Sie den Code zugewiesen haben.
    -   **Verfügbar** : gibt an, wie oft der Code noch nicht eingelöst werden kann (zu dem Zeitpunkt, zu dem die Datei generiert wurde).
    -   **Eingelöst** : gibt an, wie oft der Code eingelöst wurde (zu dem Zeitpunkt, zu dem die Datei generiert wurde).

4.  Verteilen Sie die einsetzbaren URLs über jedes von Ihnen bevorzugte Kommunikationsformat an Ihre Kunden (z. b. gezielte Benachrichtigungen, e-Mail, SMS oder gedruckte Karten). Die Kommunikation sollte Folgendes enthalten:
    -   Eine Erläuterung der APP oder des Add-Ins für den Aktions Code und optional eine Beschreibung, warum der Kunde den Code empfängt.
    -   Die einlösbare URL für den Code.
    -   Anweisungen, die den Kunden dazu leiten, die einsetzbare URL zu besuchen, sich mit Ihrem Microsoft-Konto anzumelden und die Anweisungen zum herunterladen und Installieren Ihrer APP befolgen.


## <a name="code-redemption-user-experience"></a>Benutzererfahrung bei Einlösung des Codes

Nachdem Sie einen Werbe Code (oder seine einlösbare URL) an einen Kunden verteilt haben, können Sie auf die URL klicken, um das Produkt kostenlos zu erhalten. Wenn Sie auf die einlösebare URL klicken, wird ein authentifizierter Vorgang zum **Einlösen Ihrer Codepage** unter gestartet <https://account.microsoft.com/billing/redeem> . Diese Seite enthält eine Beschreibung der App, für die der Benutzer den Code einlöst. Wenn der Kunde nicht mit seinem Microsoft-Konto angemeldet ist, wird er möglicherweise dazu aufgefordert. Der Kunde kann <https://account.microsoft.com/billing/redeem> den Code auch direkt besuchen und eingeben.

> [!IMPORTANT]
> Es wird empfohlen, dass Sie keine Aktionscodes an Ihre Kunden verteilen, bis Ihr Produkt den Veröffentlichungsprozess abgeschlossen hat (auch wenn Sie **diese Produkte verfügbar machen, aber nicht im Store auffähierbar** sind). Kunden wird ein Fehler angezeigt, wenn Sie versuchen, einen Werbe Code für ein Produkt zu verwenden, das noch nicht veröffentlicht wurde.

Nachdem der Kunde auf " **einlösen** " geklickt hat, wird der Microsoft Store auf der Übersichtsseite für die APP geöffnet (wenn Sie sich auf einem Windows 10-oder Windows 8.1-Gerät befinden), auf dem Sie auf **Installieren** klicken können, um die App kostenlos herunterzuladen und zu installieren. Wenn sich der Kunde auf einem Computer oder Gerät befindet, auf dem die Microsoft Store nicht installiert ist, wird der Link Microsoft Store Webseite für die APP gestartet. Der Code wird auf den Microsoft-Konto des Kunden angewendet, sodass er die APP später auf einem Windows-Gerät (das mit demselben Microsoft-Konto verknüpft ist) kostenlos herunterladen kann.

> [!NOTE]
> In einigen Fällen wird einem Kunden möglicherweise die Schaltfläche " **kaufen** " anstelle von " **install** " angezeigt, auch wenn die APP erfolgreich über den Aktions Code eingelöst wurde. Der Kunde kann auf **kaufen** klicken, um die App kostenlos zu installieren.


## <a name="review-your-promotional-codes"></a>Überprüfen der Werbecodes

Um eine ausführliche Zusammenfassung der Werbeaufträge für Ihre apps und Add-ons anzuzeigen, navigieren Sie zur Seite " **Aktionscodes** " (Klicken Sie im linken Navigationsmenü von Partner Center auf " **anziehen** " und dann auf "Aktions **Codes** "). In den folgenden Details finden Sie alle aktuellen und inaktiven Aktionscodes:
-   Auftragsname
-   APP oder Add-on
-   Startdatum
-   Ablaufdatum
-   Verfügbar
-   Ster

Sie können auch eine Bestellung aus dieser Tabelle [herunterladen](#download-and-distribute-promotional-codes) .

 
