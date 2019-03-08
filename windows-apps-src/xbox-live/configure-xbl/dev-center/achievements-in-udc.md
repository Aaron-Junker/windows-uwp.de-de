---
title: Erfolge 2017
description: Beschreibt, wie Sie im Partner Center zum Übermitteln von Boni Erfolge konfigurieren können.
ms.assetid: ''
ms.date: 11/10/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, udc, universal-Entwicklercenter
ms.openlocfilehash: 9ef2365ee560e273108c38570697d599adde4c0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655575"
---
# <a name="configure-achievements-2017-in-partner-center"></a>Konfigurieren Sie im Partner Center Auszeichnungen 2017

> [!IMPORTANT]
> Erfolge gelten nur für ID@Xbox oder Partner verwaltet. Spiele Xbox Live Creators-Programm teilnehmen, werden nicht unterstützt.

Sie können [Partner Center](https://partner.microsoft.com/dashboard) so konfigurieren Sie die [Auszeichnungen 2017](../../achievements-2017/simplified-achievements.md) , Ihr Spiel zugeordnet sind. Fügen Sie eine neue Auszeichnung wie folgt hinzu:

1. Navigieren Sie zum Abschnitt Erfolge für Titel, befindet sich im **Services** > **Xbox Live** > **Erfolge**.
2. Klicken Sie auf die **neue Auszeichnung** Schaltfläche, und füllen Sie das Formular.  Sobald Sie abgeschlossen ist, klicken Sie auf **speichern**.

![Screenshot zum Erstellen einer neuen Auszeichnung im Partner Center](../../images/dev-center/achievement-table.png)

## <a name="description"></a>Beschreibung
Der Abschnitt "Beschreibung" ist, in dem Sie die Grundlagen der Ihre Leistung wird z. B. den Namen und Beschreibungen gesperrt/entsperrt eingeben können. Sie können Unterstützung für die Lokalisierung in Erfolge hinzufügen, indem Sie den Zugriff auf die **lokalisierte Zeichenfolgen** Konfigurationsabschnitt aus service [Partner Center](https://partner.microsoft.com/dashboard).

![Screenshot der Beschreibungsfelder, wenn Sie eine neue Auszeichnung im Partner Center zu konfigurieren:](../../images/dev-center/achievements-2.png)

Die **Auszeichnung Namen** Feld ist, den öffentlichen Namen des Erfolgs.

Die **gesperrt Beschreibung** Feld ist die Beschreibung, Player angezeigt werden, wenn sie nicht das Erreichen der Zertifikation entsperrt haben. Sie verfügt über maximal 100 Zeichen.

Die **entsperrt Beschreibung** Feld ist die Beschreibung, die Spieler angezeigt wird, nachdem sie das Erreichen der Zertifikation entsperrt haben. Sie verfügt über maximal 100 Zeichen.

## <a name="details"></a>Details
Im Detailbereich wird verwendet, um wichtige Informationen wie z. B. das Bild, den Typ der Auszeichnung, der Gamerscore nutzen (sofern vorhanden) zuordnen, und gibt an, ob das Erreichen der Zertifikation ausgeblendet werden soll, bis entsperrt.

![Screenshot der Detailfelder, wenn Sie eine neue Auszeichnung im Partner Center zu konfigurieren](../../images/dev-center/achievements-3.png)

Die **Bildsymbol** Feld ist das Bild, das zusammen mit das Erreichen der Zertifikation angezeigt wird. Es muss es sich um eine 1920 x 1080 PNG-Datei sein.

**Basis-Erfolge** stehen Ihre Spieler, wenn das erste Spiel freigegeben wurde. **Nicht-Base Erfolge** sind verfügbar, nachdem das erste Spiel (z. B. mit neuen-DLC-Inhalt) veröffentlicht wurde.

Die **Gamerscore** Feld ist die Menge von Punkten Gamerscore, die Ihre Auszeichnung ausgezeichnet werden, nachdem die Sperre aufgehoben. Jede Auszeichnung kann zwischen 0 und 200 belohnen Punkte.  

**Öffentliche** Erfolge für alle Spieler, unabhängig davon, ob sie das Erreichen der Zertifikation oder nicht entsperrt haben sichtbar sind. **Geheimnis** Erfolge werden ausgeblendet, bis sie nicht gesperrt wurden.

**Deep-Link Auszeichnung** ist eine Möglichkeit für Sie einen Parameter aus der Auszeichnung abzurufen, mit dem Sie verknüpfen, um eine Stelle in Ihr Spiel auf, in dem die-Auszeichnung erworben werden kann. Der deep-Link in der Antwort der API zum Abrufen des zurückgegeben. Die angegebene URL darf `ms-xbl-{titleID}://` Präfix.

> [!TIP]
> Auszeichnung Deep-Links müssen die hexadezimale TitleId Ihres Spiels. Sie finden es auf [Xbox Live-Setup](xbox-live-setup.md) -Bildschirms in [Partner Center](https://developer.microsoft.com/dashboard).

## <a name="additional-rewards"></a>Zusätzliche Vorteile
In einigen Fällen empfiehlt es sich um eine in-Game-Reward oder eine Grafik bieten, wenn ein Player einen Erfolg entsperrt. Sie können definieren die Boni (sofern vorhanden), die einen Erfolg in zugeordnet sind die **zusätzliche Rewards** Abschnitt. Eine Auszeichnung kann es sich um zwei zusätzliche belohnungen – eins von jedem Typ Reward enthalten. Erfahren Sie mehr auf die [Auszeichnung Rewards](../../achievements-2017/achievement-rewards.md) Artikel.

Um eine neue Reward zu erstellen, klicken Sie auf die **Hinzufügen von Reward** Schaltfläche der **zusätzliche Rewards** Abschnitt, und füllen Sie das Formular.

![Screenshot zum Hinzufügen von Boni auf einen Erfolg im Partner Center](../../images/dev-center/achievement-reward.png)

### <a name="reward-details"></a>Reward-Details
Füllen Sie die Belohnung-Details, um eine neue Reward zuzuordnen. Sobald Sie abgeschlossen ist, klicken Sie auf **hinzufügen**.

![Screenshot der Konfiguration von Award-Details für einen Erfolg in Partner Center](../../images/dev-center/achievements-5.png)

Es gibt zwei Arten von Boni für Auszeichnung, die erstellt werden können. Diese Berichte sind:

1. Die **Art** Typ verwendet werden kann, wenn der Spieler mit Dingen zu belohnen, z. B. mit hoher Auflösung Konzept Kunst, frühen Entwurf-Zeichnungen, speziell Grafikobjekte oder anderen digitalen Grafiken erstellt werden sollen. Grafikobjekte im Xbox One-Dashboard angezeigt werden und werden auch im angezeigt werden können Companion Erfahrungen durch Abfragen des Erfolge-Diensts.
2. Die **In-Game** Typ kann verwendet werden, wenn den Spieler mit benutzerdefinierter in-Game-Rewards belohnen, ohne den Titel aktualisieren möchten. Einige möglichen Szenarien sind zusätzliche in-Game-Währung/Punkte oder Zugriff auf Sonderzeichen, Waffen oder Zuordnungen.

Die **Anzeigenamen** Feld enthält den Namen des der nutzen, der Spieler angezeigt wird. Es ist 57 Zeichen beschränkt.

Die **Beschreibung** Feld ist die Beschreibung der nutzen, dass der Spieler werden angezeigt und enthalten Anweisungen zum einlösen. Sie verfügt über maximal 90 Zeichen.

Die **Image** oder **Art** Feld ist, das die Belohnung zugeordnete Bild. Wenn der Typ auf Grafiken festgelegt ist, wird dies die Grafik sein, dass sie belohnt werden. Andernfalls wird es den in-Game-nutzen darstellen, den erhalten sie. Das Image muss es sich um eine 1920 x 1080 PNG-Datei sein.

Die **In-Game-Wert** Feld ist nur sichtbar, wenn Sie die Belohnung auswählen **im Spiel**. Es wird verwendet, um den Wert anzugeben, der die Einnahmen durch Spiele-Code übergeben werden, die verwendet werden kann, den in-Game-nutzen zu entsperren.
