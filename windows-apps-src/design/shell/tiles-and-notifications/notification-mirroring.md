---
Description: Erfahren Sie, wie Sie die Benachrichtigungs Spiegelung für ihre Popup Benachrichtigungen verwenden.
title: Benachrichtigungsspiegelung
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, Aktions Center in der Cloud, Benachrichtigungs Spiegelung, Benachrichtigung, Geräte übergreifend
ms.localizationpriority: medium
ms.openlocfilehash: b897c6574f6cbfe78406d1c624f2e3b7286ef582
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971055"
---
# <a name="notification-mirroring"></a>Benachrichtigungsspiegelung

Mithilfe von Benachrichtigungs Spiegelung, die durch das Wartungs Center in der Cloud unterstützt wird, können Sie Ihre Telefon Benachrichtigungen auf Ihrem PC anzeigen.

> [!IMPORTANT]
> **Erfordert ein Anniversary Update**: Sie müssen Build 14393 oder höher ausführen, um die Benachrichtigungs Spiegelung anzuzeigen. Wenn Sie Ihre APP für die Benachrichtigungs Spiegelung entscheiden möchten, müssen Sie für den Zugriff auf die Spiegelungs-APIs das SDK 14393 als Ziel festlegen.

Mit der Benachrichtigungs Spiegelung und Cortana können Benutzer Ihre Telefon Benachrichtigungen (Windows Mobile und Android) von der benutzerfreundlichen Anmeldung aus empfangen und darauf reagieren. Als Entwickler müssen Sie nichts tun, um die Benachrichtigungs Spiegelung zu aktivieren. die Spiegelung funktioniert automatisch. Wenn Sie auf Schaltflächen auf dem gespiegelten Popup klicken, wie z. b. Message Quick-Antworten, werden Sie zurück an das Telefon weitergeleitet, Sie rufen den Hintergrund Task auf oder starten Ihre

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Entwickler haben zwei Vorteile von der Benachrichtigungs Spiegelung: die gespiegelten Benachrichtigungen führen zu mehr Benutzerinteraktionen mit Ihrem Dienst und unterstützen außerdem Benutzer beim Entdecken Ihrer Microsoft Store Desktop-App! Ihre Benutzer wissen möglicherweise nicht einmal, dass Sie über eine beeindruckende Windows-App für Ihren Windows 10-Desktop verfügen. Wenn Benutzer die gespiegelte Benachrichtigung von Ihrem Telefon erhalten, können Benutzer auf die Popup Benachrichtigung klicken, um zum Microsoft Store zu gelangen, in dem Sie Ihre Windows-App installieren können.

Die Spiegelung funktioniert sowohl mit Windows Phone als auch mit Android. Benutzer müssen bei Cortana sowohl auf Ihrem Telefon als auch auf dem Desktop angemeldet sein, damit die Benachrichtigungs Spiegelung funktioniert.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>Was geschieht, wenn die APP auf beiden Geräten installiert wird?

Wenn der Benutzer Ihre APP bereits auf dem PC hat, wird die gespiegelte Telefon Benachrichtigung automatisch stumm geschaltet, sodass keine doppelten Benachrichtigungen angezeigt werden. Gespiegelte Benachrichtigungen werden basierend auf den folgenden Kriterien automatisch stumm geschaltet...

1. Eine APP auf dem PC ist mit dem **gleichen anzeigen Amen oder dem gleichen PFN** (Paket Familienname) vorhanden.
2. Diese PC-APP hat eine Popup Benachrichtigung gesendet.

Wenn die PC-APP noch keinen Toast gesendet hat, zeigen wir weiterhin die Telefon Benachrichtigungen an, da die Wahrscheinlichkeit ist, dass der Benutzer die PC-APP noch nicht gestartet hat).


## <a name="how-to-opt-out-of-mirroring"></a>Ablehnen der Spiegelung

Entwickler, Unternehmen und Benutzer von Windows-Apps können die Benachrichtigungs Spiegelung deaktivieren.

> [!NOTE]
> Durch das Deaktivieren der Spiegelung wird auch das [universelle verwerfen](universal-dismiss.md)deaktiviert.


### <a name="as-a-developer-opt-out-an-individual-notification"></a>Als Entwickler können Sie eine einzelne Benachrichtigung ablehnen.

Gelegentlich haben Sie möglicherweise eine gerätespezifische Benachrichtigung, dass Sie nicht auf andere Geräte gespiegelt werden möchten. Sie können verhindern, dass eine bestimmte Benachrichtigung gespiegelt wird, indem Sie die **Spiegelungs** Eigenschaft in der Popup Benachrichtigung festlegen. Diese Spiegelungs Eigenschaft kann derzeit nur für lokale Benachrichtigungen festgelegt werden (Sie kann nicht angegeben werden, wenn eine WNS-Pushbenachrichtigung gesendet wird).

**Bekanntes Problem**: Wenn Sie die Spiegelungs Eigenschaft über `ToastNotificationHistory.GetHistory()` die der API abrufen, wird immer der Standardwert (**zulässig**) anstelle der von Ihnen angegebenen Option zurückgegeben. Keine Sorge, alles ist funktionsfähig. es wird nur der Wert abgerufen, der beschädigt ist.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>Als Entwickler, vollständig ablehnen

Einige Entwickler entscheiden sich möglicherweise dafür, Ihre APP aus der Benachrichtigungs Spiegelung vollständig zu abonnieren Obwohl wir der Meinung sind, dass alle apps von der Spiegelung profitieren würden, ist es einfach, sich zu entscheiden. Nennen Sie einfach die folgende Methode einmal, und Ihre APP wird deaktiviert. Beispielsweise können Sie diesen-Befehl in den Konstruktor Ihrer APP einfügen `App.xaml.cs`...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>Wie melde ich mich als Unternehmen an?

Unternehmen können die Benachrichtigungs Spiegelung vollständig deaktivieren. Zu diesem Zweck bearbeiten Sie einfach die Gruppenrichtlinie, um die Benachrichtigungs Spiegelung zu deaktivieren.


### <a name="as-a-user-how-do-i-opt-out"></a>Wie wähle ich als Benutzer aus?

Benutzer sind in der Lage, einzelne apps zu deaktivieren oder vollständig zu deaktivieren, indem Sie die Funktion deaktivieren. Möglicherweise möchten Sie nicht, dass die Benachrichtigungen einer bestimmten app auf Ihrem Desktop gespiegelt werden, sodass Sie die jeweilige App einfach deaktivieren können. Sie finden diese Optionen sowohl auf Ihrem Telefon als auch auf dem PC in den Einstellungen von Cortana.
