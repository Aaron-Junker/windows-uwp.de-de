---
Description: Enthält Informationen zum Erstellen von bekannten Benutzergruppen für Flight-Pakete und vieles mehr.
title: Erstellen bekannter Benutzergruppen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Zielgruppe, Kunden, Flight-Gruppe, Benutzergruppen, bekannte Benutzer
ms.localizationpriority: medium
ms.openlocfilehash: 1fcb111121511553bba22cef55f94125d47e9f21
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605285"
---
# <a name="create-known-user-groups"></a>Erstellen bekannter Benutzergruppen

Mit bekannten Benutzergruppen können Sie bestimmte Kontakte zu einer Gruppe hinzufügen, indem Sie die E-Mail-Adresse deren Microsoft-Kontos verwenden. Diese bekannten Benutzergruppen werden oft verwendet, um spezifische Pakete an eine ausgewählte Benutzergruppe mit [Flight-Paketen](package-flights.md) zu verteilen, oder für die Verteilung einer Übermittlung an eine [private Zielgruppe](choose-visibility-options.md#audience). Sie können für Interaktionskampagne verwendet werden, um [benutzerorientierte Benachrichtigungen](send-push-notifications-to-your-apps-customers.md) zu senden oder [benutzerorientierte Angebote](use-targeted-offers-to-maximize-engagement-and-conversions.md) an eine spezifische Gruppe von Kunden zu senden.

Um als Mitglied der Gruppe gezählt zu werden, muss jede Person mit dem Store mithilfe der E-Mail-Adresse des Microsoft-Kontos authentifiziert werden, die Sie bereitstellen. Um die App mit dem Test-Flighting-Paket herunterzuladen, müssen Mitglieder eine Version von Windows 10 verwenden, die Flight-Pakete unterstützt (Windows.Desktop Build 10586 oder höher, Windows.Mobile Build 10586.63 oder höher, oder Xbox One). Bei Übermittlungen an private Zielgruppen müssen Mitglieder Windows 10, Version 1607 oder höher (einschließlich Xbox One) verwenden.

## <a name="to-create-a-known-user-group"></a>So erstellen Sie eine bekannte Benutzergruppe

1. In [Partner Center](https://partner.microsoft.com/dashboard), erweitern Sie **einbinden** im linken Navigationsmenü, und klicken Sie dann wählen **Kundengruppen**. 
2. Wählen Sie im Abschnitt **Meine Kundengruppen** die Option **Neue Gruppe erstellen**.
3. Geben Sie auf der nächsten Seite im Feld **Gruppennamen** einen Namen für die bekannte Benutzergruppe ein.
4. Stellen Sie sicher, dass das Optionsfeld **Neue Benutzergruppe** ausgewählt ist.
5. Geben Sie die E-Mail-Adressen der Kontakte an, die Sie der Gruppe hinzufügen möchten. Sie müssen mindestens eine E-Mail-Adresse mit maximal 10.000 Kontakten hinzufügen. Geben Sie E-Mail-Adressen direkt in das Feld ein (getrennt durch Leerzeichen, Kommas, Semikolons oder Zeilenumbrüche), oder klicken Sie auf den Link **CSV importieren**, um die Test-Flight-Gruppe aus einer Liste von E-Mail-Adressen in einer CSV-Datei zu erstellen.
6. Wählen Sie **Speichern**.

Die Gruppe ist jetzt für die Verwendung verfügbar.

Sie können auch eine bekannte Benutzergruppe erstellen, indem Sie **Flight-Gruppe erstellen** aus der [Flight-Paket](package-flights.md) Seite für die Erstellung auswählen. Beachten Sie, dass Sie alle Informationen, die Sie bereits auf der Paketseite der Flight-Erstellung bereitgestellt haben, erneut eingeben müssen, wenn Sie dies durchführen.

> [!IMPORTANT]
> Wenn Sie bekannte Benutzergruppen mit Flight-Paketen verwenden, vergewissern Sie sich, dass Sie alle erforderlichen Einverständniserklärungen von den Personen eingeholt haben, die Sie Ihrer Test-Flight-Gruppe hinzufügen, und dass diese Personen sich bewusst sind, dass sie andere Pakete als die aus Ihrer Übermittlung ohne Test-Flight erhalten. 

## <a name="to-edit-a-known-user-group"></a>So bearbeiten Sie eine bekannte Benutzergruppe

Sie können keine bekannten Benutzergruppe von Partner Center zu entfernen (oder ändern Sie den Namen), nachdem dieses erstellt wurde, aber Sie die Mitgliedschaft, zu einem beliebigen Zeitpunkt bearbeiten können.

Um Ihre bekannten Benutzergruppen zu überprüfen und zu bearbeiten, erweitern Sie das Menü **Einbeziehen** im linken Navigationsmenü und wählen Sie **Kundengruppen** aus. Wählen Sie unter **Meine Kundengruppen** den Namen der Gruppe aus, die Sie bearbeiten möchten. Sie können ebenfalls eine bekannte Benutzergruppe in der Erstellungsseite für Flight-Pakete bearbeiten, indem Sie **Vorhandene Gruppen anzeigen und verwalten** beim Erstellen eines neuen Test-Flights auswählen oder den Namen der Test-Flight-Gruppe in der Übersicht eines Flight-Pakets auswählen. 

Nachdem Sie die Gruppe, die Sie bearbeiten möchten, ausgewählt haben, können Sie E-Mail-Adressen direkt in dem Feld hinzufügen oder daraus entfernen.

Wählen Sie für größere Änderungen **Export .csv** aus, um die Mitgliedschafts-Gruppeninformationen in einer CSV-Datei zu speichern. Nehmen Sie in dieser Datei Ihre Änderungen vor, und klicken Sie dann auf **CSV importieren**, um die Gruppenmitgliedschaft mit der neuen Version zu aktualisieren.

Beachten Sie, dass es bis zu 30 Minuten dauern kann, bis Änderungen an der Mitgliedschaft implementiert werden. Sie müssen keine neue Übermittlung veröffentlichen, damit neue Mitglieder der Gruppe Zugriff auf Ihre Übermittlung über das Flight-Pakete oder über private Zielgruppen haben. Diese haben Zugriff, sobald die Änderungen implementiert sind. 






