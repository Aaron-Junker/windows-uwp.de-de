---
description: Richten Sie bestimmte Segmente der Kunden mit personalisierten Inhalten ein, um Engagement, Beibehaltung und Monetarisierung zu steigern.
title: Verwenden gezielter Angebote zum Maximieren von Engagement und Konvertierungen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, gezielte Angebote, Angebote, Benachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: 744ea22f741cd3f8a0e43d3eb88b1a7c6f4ce68b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034933"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Verwenden gezielter Angebote zum Maximieren von Engagement und Konvertierungen

Richten Sie bestimmte Segmente Ihrer Kunden mit ansprechenden, personalisierten Inhalten ein, um Engagement, Beibehaltung und Monetarisierung zu steigern.

> [!IMPORTANT]
> Gezielte Angebote können nur mit UWP-Apps verwendet werden, die Add-ons enthalten.

## <a name="targeted-offer-overview"></a>Übersicht über das gezielte Angebot

Auf hoher Ebene müssen Sie drei Dinge durchführen, um gezielte Angebote zu verwenden:

1. **Erstellen Sie das Angebot im [Partner Center](https://partner.microsoft.com/dashboard).** Navigieren Sie zur Seite **einbinden > Ziel Angebote** , um Angebote zu erstellen. Weitere Informationen zu diesem Prozess werden unten beschrieben.
2. **Implementieren Sie das in-App-Angebot.** Verwenden Sie die *Microsoft Store zielgerichteten Angebote-API* im Code Ihrer APP, um die verfügbaren Angebote für einen bestimmten Benutzer abzurufen. Außerdem müssen Sie die in-App-Funktion für das Ziel Angebot erstellen. Weitere Informationen finden Sie unter [Verwalten von zielgerichteten angeboten mithilfe von Store Services](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Senden Sie Ihre APP an den Store.** Ihre APP muss mit der in-App-Angebots Funktion veröffentlicht werden, damit die Angebote den Kunden zur Verfügung gestellt werden können.

Nachdem Sie diese Schritte ausgeführt haben, sehen Kunden, die Ihre APP verwenden, die Angebote, die Ihnen zu diesem Zeitpunkt zur Verfügung stehen, basierend auf Ihrer Mitgliedschaft in den Segmenten, die ihren angeboten zugeordnet sind. Bitte beachten Sie, dass Sie bei jedem Versuch, alle verfügbaren Angebote für Ihre Kunden anzuzeigen, gelegentlich Probleme auftreten, die sich auf die Verfügbarkeit des Angebots auswirken.


## <a name="to-create-and-send-a-targeted-offer"></a>So erstellen und senden Sie ein Ziel Angebot

1.  Erweitern Sie im [Partner Center](https://partner.microsoft.com/dashboard)im linken Navigationsmenü die Option **einbinden** , und wählen Sie dann **gezielte Angebote** aus.
2.  Überprüfen Sie die verfügbaren Angebote auf der Seite mit den **Ziel angeboten** . Wählen Sie **Neues Angebot** für alle Angebote erstellen aus, die Sie implementieren möchten.

    > [!NOTE]
    > Die verfügbaren Angebote können sich im Laufe der Zeit und basierend auf den Konto Kriterien unterscheiden.

3.  Wählen Sie in der neuen Zeile, die unterhalb der verfügbaren Angebote angezeigt wird, das Produkt (app) aus, in dem das Angebot verfügbar sein soll. Wählen Sie dann das Add-on aus, das Sie dem Angebot zuordnen möchten.
4.  Wiederholen Sie die Schritte 2 und 3, wenn Sie weitere Angebote erstellen möchten. Sie können denselben Angebotstyp mehrmals für dieselbe App implementieren, solange Sie für jedes Angebot verschiedene Add-ons auswählen. Darüber hinaus können Sie dasselbe Add-on mit mehr als einem Angebotstyp verknüpfen.
5.  Wenn Sie die Erstellung der Angebote abgeschlossen haben, klicken Sie auf **Speichern** .

Nachdem Sie Ihre Angebote implementiert haben, können Sie zur Seite " **Ziel Angebote** " im Partner Center zurückkehren, um die gesamten Konvertierungen für jedes Angebot anzuzeigen.

Wenn Sie ein Angebot nicht verwenden möchten (oder wenn Sie es nicht mehr verwenden möchten, klicken Sie auf **löschen.**

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie den Code veröffentlicht haben, um die verfügbaren Angebote für einen bestimmten Benutzer abzurufen und die app in der APP zu erstellen. Weitere Informationen finden Sie unter [Verwalten von zielgerichteten angeboten mithilfe von Store Services](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Beachten Sie bei der Überlegung des Inhalts Ihrer Ziel Angebote, dass der Inhalt in ihren angeboten wie bei allen APP-Inhalten den Store- [Inhaltsrichtlinien](/legal/windows/agreements/store-policies)entsprechen muss.
>
> Beachten Sie auch, dass ein Kunde, der Ihre APP verwendet (und bei der Festlegung der Segment Mitgliedschaft mit seiner Microsoft-Konto angemeldet ist), das Gerät an andere Personen weitergeben kann, die auf den ursprünglichen Kunden ausgerichtet waren.
