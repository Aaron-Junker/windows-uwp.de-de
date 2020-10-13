---
description: Erfahren Sie, wie Sie mithilfe von Kacheln, Signalen, Popups und Benachrichtigungen Einstiegspunkte in Ihre App bereitstellen und Benutzer auf dem neuesten Stand halten können.
title: Signalbenachrichtigungen für Windows-Apps
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a49d771b7efdbb7e787db0cbadea45c255a1120e
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984596"
---
# <a name="badge-notifications-for-windows-apps"></a>Signalbenachrichtigungen für Windows-Apps

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Eine Kachel, die mit dem numerischen Signal „63“<br/> auf 63 ungelesene E-Mails hinweist.</div>

Ein Benachrichtigungssignal enthält eine Zusammenfassung oder Statusinformationen für Ihre App. Diese Informationen können numerisch (1–99) oder eine Gruppe der vom System bereitgestellten Glyphen sein. Beispiele für Informationen, die am besten über ein Signal vermittelt werden, sind der Netzwerkverbindungsstatus in einem Onlinespiel, der Benutzerstatus in einer Nachrichten-App, die Anzahl ungelesener Nachrichten in einer E-Mail-App und die Anzahl neuer Beiträge in einer Social-Media-App. 

Benachrichtigungssignale werden unabhängig davon, ob die App gerade ausgeführt wird, auf dem Taskleisten-Symbol Ihrer App und in der unteren rechten Ecke der zugehörigen Kachel angezeigt. Signale können auf allen Kachelgrößen angezeigt werden.  

> [!NOTE]
> Es ist nicht möglich, ein eigenes Signalbild bereitzustellen. Sie können nur die vom System bereitgestellten Signalbilder verwenden.


## <a name="numeric-badges"></a>Numerische Signale

Wert | Badge | XML
--|--|--
Eine Zahl zwischen 1 und 99 Ein Nullwert entspricht dem Glyphenwert "none" und führt dazu, dass das Signal gelöscht wird. | <img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /> | `<badge value="1"/>`
Eine beliebige Zahl über 99 | <img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td> | `<badge value="100"/>`

## <a name="glyph-badges"></a>Glyphensignale
Anstelle einer Zahl kann in einem Signal eine der nicht erweiterbaren Statusglyphen angezeigt werden. 

Status | Glyphe | XML
--|--|--
Keine | (Es wird kein Signal angezeigt.) | `<badge value="none"/>`
activity | <img src="images/badges/badge-activity.png" alt="Glyph" /> | `<badge value="activity"/>`
Alarm | <img src="images/badges/badge-alarm.png" alt="Glyph" /> | `<badge value="alarm"/>`
Warnung | <img src="images/badges/badge-alert.png" alt="Glyph" /> | `<badge value="alert"/>`
Achtung | <img src="images/badges/badge-attention.png" alt="Glyph" /> | `<badge value="attention"/>`
verfügbar | <img src="images/badges/badge-available.png" alt="Glyph" /> | `<badge value="available"/>`
abwesend | <img src="images/badges/badge-away.png" alt="Glyph" /> | `<badge value="away"/>`
beschäftigt | <img src="images/badges/badge-busy.png" alt="Glyph" /> | `<badge value="busy"/>`
error | <img src="images/badges/badge-error.png" alt="Glyph" /> | `<badge value="error"/>`
newMessage | <img src="images/badges/badge-newMessage.png" alt="Glyph" /> | `<badge value="newMessage"/>`
angehalten | <img src="images/badges/badge-paused.png" alt="Glyph" /> | `<badge value="paused"/>`
Wiedergabe | <img src="images/badges/badge-playing.png" alt="Glyph" /> | `<badge value="playing"/>`
nicht verfügbar | <img src="images/badges/badge-unavailable.png" alt="Glyph" /> | `<badge value="unavailable"/>`</td>

## <a name="create-a-badge"></a>Erstellen eines Signals

In diesen Beispielen wird gezeigt, wie Sie ein Badge-Update erstellen.

### <a name="create-a-numeric-badge"></a>Erstellen eines numerischen Signals

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>Erstellen eines Glyphensignals
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>Löschen eines Signals

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

* [Benachrichtigungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)<br/> Zeigt, wie Sie Live-Kacheln erstellen, Signalupdates senden und Popupbenachrichtigungen anzeigen können. 

## <a name="related-articles"></a>Verwandte Artikel

* [Adaptive und interaktive Popupbenachrichtigungen](adaptive-interactive-toasts.md)
* [Erstellen von Kacheln](creating-tiles.md)
* [Erstellen adaptiver Kacheln](create-adaptive-tiles.md)
