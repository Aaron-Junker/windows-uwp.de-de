---
Description: Sie können Werbecodes für Apps oder Add-Ons generieren, die Sie im Microsoft Store veröffentlicht haben.
title: Generieren von Werbecodes
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Angebotscode, Angebotscodes, Token, Token
ms.localizationpriority: medium
ms.openlocfilehash: db4cde6f8c195101ec31de26c00ffa7325e08d71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605395"
---
# <a name="generate-promotional-codes"></a>Generieren von Werbecodes


[Partner Center](https://partner.microsoft.com/dashboard) ermöglicht Ihnen das Generieren von Promotioncodes für eine app oder ein Add-on, das Sie in der Microsoft Store veröffentlicht haben. Werbecodes sind eine einfache Möglichkeit, einflussreichen Benutzern kostenlosen Zugriff auf Ihre App oder Ihr Add-On zu ermöglichen. Sie können auch Promotioncodes Customer Service Szenarien verwenden, indem Benutzer kostenlos Zugriff auf Ihre app oder ein Add-on oder für [beta-Tests](beta-testing-and-targeted-distribution.md) mit Windows 10. 

Jedes Angebotscode verfügt über eine entsprechende eindeutige Gegenwert URL, die ein Kunde klicken kann, um lösen Sie den Code und Ihrer app oder ein Add-on aus dem Microsoft Store installieren.  Hinweis: Ihre App muss die abschließende Veröffentlichungsphase des [App-Zertifizierungsprozesses](the-app-certification-process.md) bestehen, bevor Kunden einen Werbecode zur Installation einlösen können.

Sie können einmaligen Code generieren (und eine für jeden Kunden zu verteilen) oder Sie können auch einen Code, der verwendet werden kann, wird mehrere Male durch eine angegebene Anzahl von Kunden zu generieren.

> [!TIP]
> Sie können [benutzerorientierte Pushbenachrichtigungen](send-push-notifications-to-your-apps-customers.md) verwenden, um einen Werbecode an ein Segment Ihrer Kunden zu verteilen. Verwenden Sie dabei unbedingt einen Werbecode, der mehreren Kunden die Nutzung des gleichen Codes ermöglicht.


## <a name="promotional-code-policies"></a>Richtlinien für Werbecodes

Beachten Sie die folgenden Richtlinien für Werbecodes:

-   Werbecodes können für alle über den Microsoft Store veröffentlichten Apps oder Add-Ons (mit Ausnahme von Abonnements für Add-Ons) generiert werden. Kunden können die Codes mit allen Versionen von Windows einlösen, die von Ihren Apps oder Add-Ons unterstützt werden.
-   Werbecodes laufen sechs Monate nach dem Datum der Bestellung ab, es sei denn, Sie wählen ein früheres Ablaufdatum aus.
-   Sie können für alle Apps oder Add-Ons alle 6 Monate Codes generieren, die bis zu 1600 Einlösungen ermöglichen. Der Zeitraum von sechs Monaten beginnt mit der Übermittlung der ersten Werbecodebestellung, auch wenn Sie ein früheres Ablaufdatum auswählen. Die Summe aller 1600 Einlösungen pro Produkt bezieht sich auf Codes zur einmaligen Verwendung und Codes, die mehrmals verwendet werden können.
-   Führen Sie die Anforderungen der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), einschließlich Abschnitt **3 k. Promotioncodes**.

> [!NOTE]
> Sie können Promotioncodes verwenden, auch wenn Ihre app für Kunden nicht verfügbar ist (d. h., wenn Sie ausgewählt haben **dieses Produkt verfügbar, aber nicht sichtbaren in den Store vornehmen** mit der **Übernahme beenden: Jeder Kunde mit einem direkten Link kann die Produktliste Store finden Sie unter, aber sie können nur heruntergeladen, wenn sie im Besitz des Produkts vor, oder haben einen Angebotscode, und ein Windows 10-Geräts verwenden** Option in Ihrer Übermittlung des [Erkennbarkeit ](choose-visibility-options.md#discoverability) Abschnitt). Mit dieser Option müssen Kunden unter Windows 10 (einschließlich Xbox) sein, um die Erwerb Ihres Produkts mit einem Angebotscode.


## <a name="order-promotional-codes"></a>Bestellen von Werbecodes

Um die Reihenfolge Promotioncodes für eine app oder ein Add-on:

1.  Im linken Navigationsmenü der [Partner Center](https://partner.microsoft.com/dashboard), erweitern Sie **Attract** und wählen Sie dann **Aktionscodes**.

2.   Klicken Sie auf der Seite **Werbecodes** auf **Codes bestellen**.

3.  Geben Sie auf der Seite **Neue Werbecodes bestellen** Folgendes ein:
    -   Wählen Sie die Apps oder Add-Ons, für die Sie Codes generieren möchten. (Beachten Sie, dass Sie keine Werbecodes für die Abonnement-Add-Ons erstellen können.)
    -   Geben Sie einen Namen für die Bestellung an. Anhand dieses Namens können Sie beim Überprüfen der Werbecode-Nutzungsdaten zwischen verschiedenen Codebestellungen unterscheiden.
    -   Wählen Sie den Auftragstyp. Sie können auch auswählen, dass ein Satz von Werbecodes generiert werden soll, die jeweils nur einmal verwendet werden können, oder dass ein Werbecode generiert wird, der mehrmals verwendet werden kann.
    -   Geben Sie die Anzahl der zu bestellenden Codes (sofern eine Reihe von Codes generiert wird) oder die Häufigkeit an, mit der der Code eingelöst werden kann (sofern ein Code generiert wird, der mehrmals verwendet werden kann).
    -   Geben Sie an, wann die Werbecodes aktiv werden sollen. Um ein spezifisches Startdatum und eine spezifische Startuhrzeit zu wählen, entfernen Sie die Markierung für das Kontrollkästchen **Codes werden umgehend aktiv**. Andernfalls werden die Codes sofort aktiv (auch wenn Ihr Produkt des Veröffentlichungsprozesses in der Bestellung eines Kunden den Code wurde muss).
    -   Geben Sie an, wann die Werbecodes ablaufen sollen. Um ein spezifisches Ablaufdatum und eine frühere Ablaufzeit als sechs Monate zu wählen, deaktivieren Sie das Kontrollkästchen **Codes laufen nach 6 Monaten ab**.

4.  Klicken Sie auf **Codes bestellen**. Sie werden dann zurück auf die Seite **Werbecodes** geleitet, auf der Sie Ihre neue Bestellung in der Zusammenfassungstabelle der Werbecodebestellungen für diese App sehen.


## <a name="download-and-distribute-promotional-codes"></a>Herunterladen und Verteilen von Werbecodes

So laden Sie erfüllte Werbecodebestellungen herunter und verteilen die Codes an Kunden:

1.  Im linken Navigationsmenü der [Partner Center](https://partner.microsoft.com/dashboard), erweitern Sie **Attract** und wählen Sie dann **Aktionscodes.**
2.  Klicken Sie auf den Link zum **Herunterladen** Ihres Angebotscodes, und speichern Sie die Datei auf Ihrem Computer. Diese Datei enthält Informationen zu Ihrer Werbecodebestellung im TSV-Format (.tsv).
3.  Öffnen Sie die .tsv-Datei im Editor Ihrer Wahl. Öffnen Sie für ein optimales Ergebnis die .tsv-Datei in einer Anwendung, die Daten in tabellarischer Struktur anzeigen kann, z. B. Microsoft Excel. Allerdings können Sie die Datei auch in einem Text-Editor öffnen.

    Die Datei enthält Spalten mit den folgenden Daten für jeden Code:

    -   **Produktname**: Der Name der app oder ein Add-on, mit dem Code zugeordnet ist.
    -   **Auftragsname**: Der Name der die Reihenfolge, in der dieser Code generiert wurde.
    -   **Angebotscode**: Der Code selbst. Dies ist eine 5x5-Zeichenfolge von alphanumerischen Zeichen, die durch Bindestriche getrennt sind. Zum Beispiel: DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **Gegenwert URL**: Die URL, die ein Kunde verwenden können, lösen Sie den Code, und installieren Sie Ihre app oder ein Add-on. Die URL weist das folgende Format: https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; Promotional_code >
    -   **Startdatum**: Das Datum, an das diesem Code aktiv geworden ist.
    -   **Laufen ab Datum**: Das Datum, an das diesem Code abläuft.
    -   **Codieren von ID**: Eine eindeutige ID für diesen Code.
    -   **Order ID**: Eine eindeutige ID für die Reihenfolge, in der dieser Code generiert wurde.
    -   **Übergeben, um**: Ein leeres Feld, das Sie verwenden können, welcher Kunde von konnten Sie den Code hinzu.
    -   **Verfügbar**: Die Anzahl der Häufigkeit, mit die der Code weiterhin verfügbar ist (zu dem Zeitpunkt, der die Datei generiert wurde) einlösen.
    -   **Eingelöst**: Die Anzahl der Fälle, in denen der Code hat (zu dem Zeitpunkt, der die Datei generiert wurde) eingelöst.

4.  Sie können die einlösbaren URLs in einem beliebigen, von Ihnen bevorzugten Kommunikationsformat an Kunden verteilen (z. B. zielgruppenorientierte Benachrichtigungen, E-Mail, SMS-Nachricht oder gedruckte Karten). Die Kommunikation sollte Folgendes enthalten:
    -   Eine Erklärung, für welche App bzw. welches Add-On der Werbecode vorgesehen ist, und optional eine Beschreibung, warum der Kunde den Code erhält.
    -   Die einlösbare URL für den Code.
    -   Anweisungen zum Aufrufen der einlösbaren URL, Anmelden mit dem Microsoft-Konto und Befolgen der Anweisungen zum Herunterladen und Installieren Ihrer App.


## <a name="code-redemption-user-experience"></a>Benutzererfahrung bei Einlösung des Codes

Nachdem Sie einen Angebotscode (oder dessen Gegenwert URL) für einen Kunden verteilen, können sie die URL, um das Produkt kostenlos klicken. Durch das Klicken der einlösbaren URL wird eine authentifizierte Seite für die **Codeeinlösung** unter <https://account.microsoft.com/billing/redeem> geöffnet. Diese Seite enthält eine Beschreibung der App, für die der Benutzer den Code einlöst. Wenn der Kunde nicht mit seinem Microsoft-Konto angemeldet ist, kann er dazu aufgefordert werden. Ihre Kunden kann ebenfalls <https://account.microsoft.com/billing/redeem> besuchen und den Code direkt eingeben.

> [!IMPORTANT]
> Es wird empfohlen, dass Sie Werbecodes erst an Ihre Kunden verteilen, wenn Ihr Produkt den Veröffentlichungsprozess abgeschlossen hat (auch wenn Sie **Make this product available but not discoverable in the Store** ausgewählt haben). Kunden wird einen Fehler angezeigt, wenn sie versuchen, einen Werbecode für ein Produkt zu verwenden, das noch nicht veröffentlicht wurde.

Wenn der Kunde auf **Einlösen** klickt, wird die Übersicht der App im Microsoft Store geöffnet (auf einem Windows 10- oder Windows 8.1-Gerät). Dort kann er auf **Installieren** klicken, um die App kostenlos herunterzuladen und zu installieren. Wenn sich der Kunde an einem Computer oder einem Gerät befindet, auf dem Microsoft Store nicht installiert ist, öffnet der Link die Microsoft Store-Webseite für die App. Der Code wird auf das Microsoft-Konto des Kunden angewendet, damit er die App später kostenlos auf einem Windows-Gerät herunterladen kann (das mit dem gleichen Microsoft-Konto verknüpft ist).

> [!NOTE]
> In einigen Fällen kann ein Kunde finden Sie eine **kaufen** Schaltfläche anstelle von **installieren**, auch wenn die app über den Angebotscode erfolgreich eingelöst wurde. Der Kunde kann dann zum kostenlosen Installieren der App auf **Kaufen** klicken.


## <a name="review-your-promotional-codes"></a>Überprüfen der Werbecodes

Um eine detaillierte Zusammenfassung der Angebotscode Bestellungen für Ihre apps und -Add-Ons zu überprüfen, wechseln Sie zu der **Promotioncodes** Seite (Erweitern Sie im linken Navigationsmenü der Partner Center **Attract** und wählen Sie dann **Aktionscodes**). Sie können die folgenden detaillierten Informationen für Ihre aktuellen und inaktiven Werbecodes überprüfen:
-   Bestellungsname
-   App oder Add-On
-   Startdatum
-   Ablaufdatum
-   Verfügbar
-   Eingelöst

Sie können auch eine Bestellung aus dieser Tabelle [herunterladen](#download-and-distribute-promotional-codes).

 
