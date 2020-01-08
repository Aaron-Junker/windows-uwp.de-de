---
Description: Sprechen Sie für bessere Interaktion, Kundenbindung und Monetarisierung bestimmte Segmente Ihrer Kunden mit personalisiertem Inhalt an.
title: Verwenden Sie zielgerichtete Angebote, um Interaktionen und Abschlüsse zu maximieren.
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, gezielte Angebote, Angebote, Benachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: fd4cf135e5129ed4f3dbdbcebfb2003e7573182a
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685099"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Verwenden Sie zielgerichtete Angebote, um Interaktionen und Abschlüsse zu maximieren.

Sprechen Sie für bessere Interaktion, Kundenbindung und Monetarisierung bestimmte Segmente Ihrer Kunden mit attraktivem, personalisiertem Inhalt an.

> [!IMPORTANT]
> Zielgerichtete Angebote können nur mit UWP-Apps verwendet werden, die Add-Ons enthalten.

## <a name="targeted-offer-overview"></a>Übersicht über zielgerichtete Angebote

Allgemein gesagt müssen Sie drei durchführen, um die zielgerichteten Angebote zu verwenden:

1. **Erstellen Sie das Angebot im [Partner Center](https://partner.microsoft.com/dashboard).** Navigieren Sie zur Seite **Bewerben > zielgerichtete Angebote**, um Angebote zu erstellen. Weitere Informationen zu diesem Prozess sind im Folgenden beschrieben.
2. **Implementieren Sie das in-App-Angebot.** Verwenden Sie die *Microsoft Store zielgerichteten Angebote-API* im Code Ihrer APP, um die verfügbaren Angebote für einen bestimmten Benutzer abzurufen. Sie müssen auch die In-App-Umgebung für das gezielte Angebot erstellen. Weitere Informationen finden Sie unter [Verwalten gezielter Angebote mithilfe von Store-Diensten](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Senden Sie Ihre APP an den Store.** Ihre App muss mit dem integrierten In-App-Angebot veröffentlicht werden, damit die Angebote für Kunden verfügbar sind.

Nachdem Sie diese Schritte abgeschlossen haben, werden Kunden, die Ihre App verwenden, die Angebote angezeigt, die zu diesem Zeitpunkt für ihn verfügbar sind, basierend auf seiner Mitgliedschaft in dem diesem Angebot zugeordneten Segment. Beachten Sie, dass wir zwar bemüht sind, alle verfügbaren Angebote für Ihre Kunden anzuzeigen, es jedoch gelegentlich zu Problemen kommen kann, die die Angebotsverfügbarkeit beeinträchtigen.


## <a name="to-create-and-send-a-targeted-offer"></a>So erstellen und senden Sie ein gezieltes Angebot

1.  Erweitern Sie im [Partner Center](https://partner.microsoft.com/dashboard)im linken Navigationsmenü die Option **einbinden** , und wählen Sie dann **gezielte Angebote**aus.
2.  Überprüfen Sie auf der Seite **zielgerichtete Angebote** die verfügbaren Angebote. Wählen Sie **Create new offer** für ein Angebot, das Sie implementieren möchten.

    > [!NOTE]
    > Die angezeigten verfügbaren Angebote ändern sich im Laufe der Zeit und auf der Grundlage von Kontokriterien.

3.  Wählen Sie in der neuen Zeile, die unter den verfügbaren Angebote angezeigt wird, das Produkt (App) aus, in dem das Angebot zur Verfügung steht. Wählen Sie dann das Add-On aus, das Sie dem Angebot zuordnen möchten.
4.  Wiederholen Sie die Schritte 2 und 3, wenn Sie weitere Angebote erstellen möchten. Sie können den gleichen Angebotstyp mehrmals in derselben App implementieren, solange Sie für jedes Angebot andere Add-Ons auswählen. Darüber hinaus können Sie das gleiche Add-On mehr als einem Angebotstyp zuordnen.
5.  Klicken Sie auf **Speichern**, wenn Sie mit dem Erstellen der Angebote fertig sind.

Nachdem Sie Ihre Angebote implementiert haben, können Sie zur Seite " **Ziel Angebote** " im Partner Center zurückkehren, um die gesamten Konvertierungen für jedes Angebot anzuzeigen.

Wenn Sie sich entscheiden, kein Angebot zu verwenden (oder wenn Sie es nicht mehr verwenden möchten), klicken Sie auf **Löschen**.

> [!IMPORTANT]
> Achten Sie darauf, dass Sie den Code veröffentlichen, um die verfügbaren Angebote für einen bestimmten Benutzer abzurufen und die In-App-Umgebung zu erstellen. Weitere Informationen finden Sie unter [Verwalten gezielter Angebote mithilfe von Store-Diensten](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Beachten Sie bei den Inhalten gezielter Angebote, dass sie wie der gesamte App-Inhalt die [Inhaltsrichtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies) des Stores erfüllen müssen.
>
> Beachten Sie außerdem Folgendes: Wenn ein Kunde, der Ihre App verwendet (und zum Zeitpunkt, zu dem die Segmentmitgliedschaft bestimmt wird, mit seinem Microsoft-Konto angemeldet ist) das Gerät später an eine andere Person übergibt, sieht die andere Person unter Umständen das Angebot, das für den ursprünglichen Kunden bestimmt war.
