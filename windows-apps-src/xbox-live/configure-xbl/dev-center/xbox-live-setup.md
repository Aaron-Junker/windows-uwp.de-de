---
title: Xbox Live-Setup-Konfiguration
description: Beschreibt, wie Sie die Xbox Live-Setup im Partner Center konfigurieren können.
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, die Partner Center, Xbox Live-Setup
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612825"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>Konfigurieren von Xbox Live-Setup im Partner Center

Sie können [Partner Center](https://developer.microsoft.com/dashboard) so konfigurieren Sie den anfänglichen Satz von Xbox Live-Eigenschaften, die Ihr Spiel zugeordnet sind. Fügen Sie Konfiguration wie folgt hinzu:

1. Navigieren Sie zu der **Xbox Live-Setup** Titel, befindet sich im Abschnitt **Services** > **Xbox Live** > **Xbox Live-Setup** .
2. Auf dieser Seite können Sie die Titelnamen, Standardgebietsschema, Produkttyp, gerätefamilien und das Datum Embargo festlegen. Sobald Sie fertig sind Ihre Konfiguration wird festgelegt, klicken Sie auf die **speichern** Schaltfläche, um die Änderungen zu übermitteln.

## <a name="title-names"></a>, Titelnamen
Durch Klicken auf **hinzufügen lokalisierte Titel**, Sie geben Sie einen Namen für Ihr Produkt können, und wählen Sie eine Sprache aus, um es zu lokalisieren. Beachten Sie hier, dass es sich bei der Namen des lokalisierten Produktnamen zuordnen soll, die Sie auf der Eigenschaftenseite der Übermittlung definiert haben. Standardwert ist Englisch (En-US).

![Bild des Dialogfelds lokalisierte Titel "hinzufügen" im Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>Standardgebietsschema
Diese Option können Sie die Standardsprache verwendet werden, so konfigurieren Sie alle Zeichenfolgen in der Konfiguration für die Xbox Live-Dienst festgelegt. Z. B. Wenn Sie das Standardgebietsschema auf Spanisch (es-ES) festlegen und einen Erfolg konfigurieren möchten, müssten klicken Sie dann mindestens Auszeichnung Name und Beschreibung in Spanisch werden. Das heißt, Sie können keine legen Sie diese Option auf Spanisch geben jedoch nur die Auszeichnung Informationen in englischer Sprache. Alle Ihre Xbox Live-Service-Konfiguration muss in der gleichen Version wie für das Standardgebietsschema angegeben werden. Standardmäßig wird das Standardgebietsschema auf Englisch (En-US) festgelegt.
> [!NOTE]
> Darüber hinaus können alle Zeichenfolgen auf der Seite der lokalisierte Zeichenfolgen lokalisiert werden.  

![Bild der Dropdownliste aus, um das Standardgebietsschema wählen Sie im Partner Center auswählen](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>Produkttyp
Klicken Sie im Dropdown-Menü ermöglicht, dass Sie den Typ des Produkts zu ändern. Wird standardmäßig in den Typ **Spiel**. Die von Ihnen gewählten wirkt sich auf der Xbox Live-Funktionen zur Verfügung. Sie haben drei Möglichkeiten zur Auswahl:
1. App 
2. Game 
3. Spiele-demo 

![Bild der Dropdownliste aus, um Ihr Produkttyp wählen Sie im Partner Center auswählen](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>Gerätefamilien
Diese Konfiguration können Sie den Typ von Geräten auswählen, der Titel des Xbox Live zugreifen kann. Standardmäßig werden allen gerätefamilien aktiviert. Sehen Sie die Geräte aus, um sie zu aktivieren.

![Abbildung der Auswahl Kontrollkästchen die gerätefamilien im Partner Center auswählen](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>Embargo Datum
Das Datum an, das Sie auswählen, wird bestimmt, wenn Ihre Xbox Live-Konfiguration für die Öffentlichkeit online geschaltet wird. Es ist wichtig zu beachten, dass selbst wenn Sie die Änderungen auf "RETAIL" veröffentlicht sie nicht live geschaltet werden, wenn das Datum Embargo erreicht wurde. Um weitere Informationen finden Sie:
1. Wenn Sie ein Datum in der Zukunft auswählen, werden die Änderungen an diesem Datum öffentlich zur Verfügung gestellt.
2. Wenn Sie ein Datum in der Vergangenheit auswählen, werden die Änderungen für die Öffentlichkeit verfügbar, sobald Sie Ihre Änderungen auf "RETAIL" veröffentlichen.

Klicken Sie auf die Datum / Uhrzeit-Auswahl, und es wird erweitert, um das genaue Datum und Uhrzeit auswählen. Nach dem Klicken auf **OK**, wird das Datum Embargo festgelegt werden.

![Bild festlegen des Datums Embargo im Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>Erweiterte Einstellungen

Klicken Sie auf **Optionen anzeigen** Festlegen der **MPOP**. MPOP ermöglicht des gleichen Benutzers mit zu Xbox Live über mehrere Geräte gleichzeitig anmelden. Xbox Live-Features wie z. B., Erfolge und Multiplayer werden eingeschränkten Zugriff haben. Diese Option sollte daher nicht für Spiele. Aktivieren Sie diese Option, indem Sie das Kontrollkästchen aktivieren.
