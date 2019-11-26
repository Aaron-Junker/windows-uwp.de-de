---
title: Reagieren auf Kundenfeedback
description: Sie können direkt auf Feedback reagieren, das Ihre Kunden im Feedback-Hub hinterlassen.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 04983b80-2a18-4ace-93d3-e8c33c04bfb9
ms.localizationpriority: medium
ms.openlocfilehash: 9bc305834381fcdddac6e4eacf12144fd9f189c2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259946"
---
# <a name="respond-to-customer-feedback"></a>Reagieren auf Kundenfeedback

Sie können den [Feedbackbericht](feedback-report.md) verwenden, um das Feedback zu prüfen, das Ihre Windows 10-Kunden zu Ihrer App im Feedback-Hub hinterlassen haben, und dann direkt auf dieses Feedback antworten. Sie können Ihre Antworten im Feedback-Hub für alle Benutzer veröffentlichen (entweder als einzelne Kommentare oder durch Aktualisieren des Status eines Feedbacks und Hinzufügen einer Beschreibung), um die Kunden über neue Funktionen und Fehlerkorrekturen zu informieren oder um detaillierteres Feedback zur Verbesserung Ihrer App zu bitten. Sie können Ihre Antwort auch per E-Mail direkt an den Kunden senden, der das Feedback abgegeben hat.

> [!TIP]
> Über die Feedback-API im [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) können Sie es Kunden ermöglichen, Feedback zu geben. Dabei wird ein Steuerelement hinzugefügt, über das Kunden direkt [den Feedback-Hub von Ihrer UWP-App starten können](../monetize/launch-feedback-hub-from-your-app.md). Bedenken Sie, dass jeder Kunde, der Ihre App auf einem Windows 10-Gerät heruntergeladen hat, das den Feedback-Hub unterstützt, mithilfe der Feedback-Hub-App ein Feedback abgeben kann. Aus diesem Grund wird in diesem Bericht möglicherweise Feedback von Kunden angezeigt, auch wenn Sie in Ihrer App nicht explizit um Feedback gebeten haben.

Um zu einem Feedback eine Antwort zu geben, klicken Sie auf den Link **Feedback beantworten**, der neben dem Feedback in Ihrem **Feedbackbericht** angezeigt wird.

[Partner Center](https://partner.microsoft.com/dashboard) unterstützt drei Optionen für die Reaktion auf Kunden, die Feedback zu Ihrer APP bereitstellen. Unabhängig davon, welche Option Sie wählen, denken Sie daran, dass maximal 1.000 Zeichen für jede Antwort zulässig sind.

## <a name="public-comments-in-feedback-hub"></a>Öffentliche Kommentare im Feedback-Hub

Standardmäßig ist das Optionsfeld für **Kommentar** ausgewählt, wenn Sie auf **Feedback beantworten** klicken. Um eine öffentliche Antwort auf das Feedback des Kunden zu veröffentlichen, lassen Sie dieses Feld ausgewählt. Geben Sie Ihren Kommentar in das Feld ein, und klicken Sie dann auf **Senden**.

Der eingegebene Kommentar wird als Kommentar im Feedback-Hub angezeigt, zusammen mit den Kommentaren, die von anderen Kunden übermittelt wurden. Ihr Herausgebername und der Name der App werden mit dem Kommentar angezeigt, um Sie als den Entwickler der App zu identifizieren. Es gibt keine Beschränkung für die Anzahl der Kommentare, die Sie für ein Feedback schreiben können. Beachten Sie jedoch, dass Kommentare nicht mehr bearbeitet oder gelöscht werden können, nachdem sie übermittelt wurden. Die fünf neuesten Kommentare zu einem Feedback werden in Ihrem **Feedbackbericht** angezeigt (ebenso wie im Feedback-Hub). Wenn mehr als fünf Kommentare vorhanden sind, können Sie auf **Alle Kommentare anzeigen** klicken, um alle Kommentare im Feedback-Hub anzuzeigen.


## <a name="private-responses-via-email"></a>Private Antworten per E-Mail

Wenn Sie es bevorzugen, keine öffentliche Antwort zu posten, aktivieren Sie das Kontrollkästchen **Kommentar als E-Mail senden**, um eine private Antwort direkt an den Kunden zu senden (sofern dieser eine E-Mail-Adresse angegeben und sich für den Empfang von Antworten per E-Mail entschieden hat). In diesem Fall sendet Microsoft in Ihrem Auftrag eine E-Mail an den Kunden. Die E-Mail enthält das ursprüngliche Feedback sowie Ihre Antwort darauf.

Nachdem Sie das Kontrollkästchen **Kommentar als E-Mail senden** aktiviert haben, geben Sie den Kommentar ein, und klicken Sie dann auf **Senden**. Beachten Sie, dass Sie im Feld **E-Mail Support-Kontakt** eine E-Mail-Adresse angeben müssen, wenn Sie diese Option verwenden. Standardmäßig verwenden wir die E-Mail-Adresse, die Sie in den Kontaktinformationen für Ihr Konto bereitgestellt haben. Wenn Sie eine andere E-Mail-Adresse verwenden möchten, können Sie die **E-Mail Support-Kontakt** ändern. Der Kunde, der Ihre Antwort erhält, kann direkt an diese E-Mail-Adresse antworten.


## <a name="public-status-updates-and-descriptions-in-feedback-hub"></a>Öffentliche Statusaktualisierungen und Beschreibungen im Feedback-Hub

Eine dritte Option für eine öffentliche Antwort ist, den Status eines Feedbacks anzugeben, um z. B. Ihren Kunden mitzuteilen, dass Sie an dem Problem arbeiten oder es gelöst haben. Wenn Sie den Status zu einem Feedback aktualisieren, wird dieser zusammen mit dem Feedback im Feedback-Hub angezeigt.

Um diese Option zu verwenden, wählen Sie das Optionsfeld **Status aktualisieren** aus. Wählen Sie dann eine der folgenden Optionen aus:

- **Wird untersucht**: Sie sind sich des Problems bewusst und gehen ihm nach.
- **Wir arbeiten daran**: Sie sind dabei, ein Problem zu lösen oder eine angeforderte Funktion hinzuzufügen.
- **Abgeschlossen**: Sie haben ein Update veröffentlicht, um das Problem zu lösen oder die angeforderte Funktion hinzuzufügen.

Neben dem Aktualisieren des Status können Sie auch einen Kommentar eingeben, um weitere Informationen bereitzustellen, z. B. eine Schätzung, wann das Problem gelöst sein wird, oder weitere Informationen zu den neuesten Änderungen. Diese Beschreibung wird am oberen Rand der Liste der Kommentare angezeigt (im Feedbackbericht werden der aktuelle Status und die Beschreibung angezeigt).

Mit der Option **Status aktualisieren** können Sie den Status jederzeit ändern (und aktualisierte Beschreibungen für jede Statusänderung hinzufügen). Wenn Sie den Status eines Feedbacks ändern, wird der Status auch im Feedback-Hub aktualisiert, damit den Kunden, die Ihre Antwort lesen, der aktuelle Status angezeigt wird.


## <a name="guidelines-for-responses"></a>Richtlinien für Antworten

Unabhängig davon, welche Methode Sie verwenden, um auf das Feedback der Kunden zu reagieren, müssen Sie folgende Richtlinien für alle Antworten befolgen.
- Antworten dürfen maximal 1.000 Zeichen umfassen.
- Sie dürfen Benutzern für ihre öffentlichen Kommentare keine Gegenleistungen, einschließlich digitaler Apps, anbieten.
- Ihre Antwort sollte keine Marketing- oder Werbeinhalte enthalten. Beachten Sie, dass die Person, die ein Feedback abgegeben hat, bereits Ihr Kunde ist.
- Bewerben Sie in Ihrer Antwort keine anderen Apps oder Dienste.
- Ihre Antwort muss sich direkt auf die jeweilige App und das damit verbundene Feedback beziehen.
- Ihre Antwort sollte keine profanen, aggressiven, persönlichen oder bösartigen Inhalte enthalten. Bleiben Sie stets höflich und denken Sie daran, dass zufriedene Kunden die wahrscheinlich beste Werbung für Ihre App sind.

> [!NOTE]
> Kunden können eine unangemessene Antwort, mit der ein Entwickler auf ein Feedback reagiert, an Microsoft melden. Sie können auch entscheiden, keine Antworten auf Feedback per E-Mail zu erhalten.

Sie alleine sind für die Kommunikation mit Ihren Kunden verantwortlich. Microsoft beteiligt sich nicht an Meinungsverschiedenheiten zwischen Entwicklern und Kunden. Wenn Sie jedoch der Meinung sind, dass der Inhalt eines Feedbacks für Ihr Produkt unangebracht ist, reichen Sie bitte ein [Supportticket](https://developer.microsoft.com/windows/support) ein.
