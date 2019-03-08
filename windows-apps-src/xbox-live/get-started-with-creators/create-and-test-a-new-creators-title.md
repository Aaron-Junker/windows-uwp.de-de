---
title: Erstellen Sie und Testen Sie einen neuen Creators-Titel
description: Informationen Sie zum Erstellen eines neuen Titels für die Xbox Live Creators-Programm, und klicken Sie in der testumgebung veröffentlichen.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Ersteller, testen
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594595"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>Erstellen eines neuen Titels für die Xbox Live Creators-Programm, und veröffentlichen Sie in der testumgebung

## <a name="introduction"></a>Einführung

Vor dem Xbox Live Code schreiben zu müssen, müssen Sie einen neuen Titel in Ihrem Konfiguration einrichten.  Weitere Informationen finden Sie Informationen zur Dienstkonfiguration in [Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md).

In diesem Artikel begleitet, um einen Titel, die im konfigurierten abrufen [Partner Center](https://partner.microsoft.com/dashboard), ein neues Projekt erstellt sowie zum Vorbereiten der Xbox Live zu Testzwecken. In diesem Artikel wird Folgendes vorausgesetzt:

1. Verwenden Sie das Xbox Live Creators-Programm.
2. Entwickeln Sie einen Titel für die universelle Windows-Plattform (UWP).  UWP-Titel können auf eine Xbox, Windows 10-desktop-PCs und Mobile ausführen.
3. Konfigurieren Sie Ihre Titel in [Partner Center](https://partner.microsoft.com/dashboard).
4. Entwicklungscomputer wird Windows 10 ausgeführt werden.

> [!NOTE]
> Wenn Sie Teil der Xbox Live Creators-Programm sind, die oben aufgeführten Annahmen gelten für Sie und sollten Sie zusammen mit den in diesem Artikel befolgen

## <a name="partner-center-setup"></a>Partner Center-setup

Sie benötigen einen Titel ein Xbox Live aktiviert in erstellt [Partner Center](https://partner.microsoft.com/dashboard) als erforderliche Komponente alle Xbox Live-Funktionen verwenden.

### <a name="create-a-microsoft-account"></a>Microsoft-Konto erstellen
Wenn Sie nicht über ein Microsoft-Account (auch bekannt als ein MSA) verfügen, müssen Sie zunächst nacheinander erstellen [Microsoft Account - Anmeldung](https://go.microsoft.com/fwlink/p/?LinkID=254486). Wenn Sie über ein Office 365-Konto verfügen, Outlook.com, oder haben Sie ein Konto auf der Xbox Live - haben Sie wahrscheinlich bereits eine MSA.

### <a name="register-as-an-app-developer"></a>Registrieren Sie sich als ein App-Entwickler
Sie müssen als ein App-Entwickler zu registrieren, bevor Sie einen neuen Titel im Partner Center erstellen können.

Registrieren Sie sich auf [als app-Entwickler registrieren](https://developer.microsoft.com/store/register) und folgen Sie den Registrierungsvorgang ab.

### <a name="create-a-new-uwp-title"></a>Erstellen Sie einen neuen UWP-Titel
Sie benötigen einen UWP-Titel, der in Partner Center definiert. Sie dies, indem die erste jetzt [Partner Center](https://partner.microsoft.com/dashboard).

Als Nächstes erstellen Sie einen neuen Titel ein. Sie müssen einen Namen zu reservieren.

![](../images/getting_started/first_xbltitle_newapp.png)

Der Screenshot zeigt der Hauptseite, in dem Xbox Live, befindet sich im Konfigurieren Sie müssen die **Services** > **Xbox Live** Menü.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Xbox Live-Dienste aktivieren
Beim Klicken auf die **Xbox Live** link **Services** zum ersten Mal für ein Produkt, werden Sie auf der Seite Programm aktivieren Xbox Live Creators weitergeleitet werden.  Klicken Sie anschließend die **aktivieren** Schaltfläche, um das Xbox Live-Setup-Dialogfeld zu öffnen.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

Wählen Sie im Dialogfeld "Setup" die Plattformen, die Sie, aktivieren Sie die Xbox Live-Dienste für möchten (Xbox One und Windows 10-PCs sind standardmäßig ausgewählt).  Klicken Sie auf die **bestätigen** Schaltfläche, um für Ihr Spiel Xbox Live Creators-Programm zu aktivieren.

> [!IMPORTANT]
> Xbox Live ist nur für Spiele unterstützt. Um Ihr Spiel mit Xbox Live wurde erfolgreich zu veröffentlichen, müssen Sie Ihr Produkttyp auf "Game" im Abschnitt mit Eigenschaften der Übermittlung festlegen.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>Testen Sie in Ihrem Spiel Xbox Live-Dienstkonfiguration
Wenn Sie Änderungen an der Xbox Live-Konfiguration für Ihr Spiel vornehmen, müssen Sie die Änderungen in einer bestimmten Umgebung veröffentlichen, bevor sie vom Rest des Xbox Live übernommen und werden, indem Sie Ihr Spiel angezeigt können.

Wenn Sie weiterhin auf Ihr Spiel funktioniert, veröffentlichen Sie in Ihre Entwicklung Sandbox ein.  Die Sandbox für die Entwicklung können Sie Änderungen an Ihrem Spiel in einer isolierten Umgebung arbeiten.

Wenn Ihr Spiel öffentlich freigegeben wird, wird die Xbox Live-Konfiguration automatisch mit der Sandbox RETAIL veröffentlicht werden.

Sind standardmäßig ein Xbox-Konsolen und Windows 10-PCs in der Sandbox Einzelhandel.

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Veröffentlichen von Xbox Live-Konfiguration in der testumgebung

Wenn Sie Xbox Live-Dienste aktivieren, und nehmen Sie Änderungen an der Dienstkonfiguration Xbox Live, müssen um die Änderungen wirksam werden, werden diese Änderungen in Ihre Entwicklung Sandbox zu veröffentlichen.

Klicken Sie auf der Xbox Live-Konfigurationsseite, auf die **Test** Schaltfläche, um die aktuelle Konfiguration von Xbox Live in Ihre Entwicklung Sandbox zu veröffentlichen.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>Autorisieren von Geräten und Benutzern für den Sandkasten für Entwicklung

Nur autorisierte Geräte und Benutzer können die Xbox Live-Konfiguration für das Spiel in Ihre Entwicklung Sandbox zugreifen.

Standardmäßig haben alle die Xbox One-Entwicklung-Konsolen, die Sie mit Ihrem Partner Center-Konto hinzugefügt haben Zugriff auf Ihre Entwicklung Sandbox.  Wenn eine Xbox One-Konsole hinzufügen möchten, wechseln Sie zu [verwalten deiner Xbox One Konsolen](https://partner.microsoft.com/xboxconfig/devices). Wenn Sie bereits in Ihrem Partner Center-Konto haben, wechseln Sie zu **Kontoeinstellungen** > **Kontoeinstellungen** > **Dev Geräte**  >  **Xbox One Entwicklung Konsolen**.

Sie können auch normale Xbox Live-Konten Zugriff auf Ihre Entwicklung Sandbox autorisieren.  Wechseln Sie zu, um Xbox Live des Zugriffs auf Ihre Entwicklung Sandbox zu autorisieren, [Verwalten von Konten](https://developer.microsoft.com/xboxtestaccounts/configurecreators).

## <a name="next-steps"></a>Nächste Schritte
Nun, da Sie einen neuen Titel erstellt haben, können Sie jetzt einen Xbox Live aktiviert Titel in Ihre Spiele-Engine, die Visual Studio oder die Build-Umgebung Ihrer Wahl einrichten.

Finden Sie unter [Schritt für Schritt-Anleitung zum Integrieren von Xbox Live](creators-step-by-step-guide.md)
