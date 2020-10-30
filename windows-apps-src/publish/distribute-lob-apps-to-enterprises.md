---
description: Sie können Lob-Apps (Line-of-Business) direkt in Unternehmen veröffentlichen, um Sie über die Microsoft Store für Unternehmen oder Microsoft Store for Education zu erwerben, ohne dass die apps im Store allgemein verfügbar gemacht werden.
title: Verteilen von branchenspezifischen Apps an Unternehmen
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: Windows 10, UWP, Lob, Line-of-Business, Unternehmens-apps, Store für Unternehmen, Store für Bildungseinrichtungen, Enterprise
ms.localizationpriority: medium
ms.openlocfilehash: b9bb384ac8514431899fd6ac37cf56303a80d214
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033923"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Verteilen von branchenspezifischen Apps an Unternehmen

Sie haben mehrere Optionen für die Verteilung von Branchen-Apps (Line of Business, Lob) an die Benutzer Ihrer Organisation, die [msix-Pakete](/windows/msix/) verwenden, ohne dass die apps öffentlich verfügbar sind. Sie können Geräte Verwaltungs Tools verwenden, eine APP-Installer-basierte Bereitstellung konfigurieren, die apps direkt querladen oder die apps im Microsoft Store für Unternehmen oder Microsoft Store für Bildungseinrichtungen veröffentlichen.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft Endpoint Configuration Manager und Microsoft InTune

Wenn Ihre Organisation Microsoft Endpoint Configuration Manager oder Microsoft InTune verwendet, um Geräte zu verwalten, können Sie Lob-Apps mithilfe dieser Tools bereitstellen. Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

* [Einführung in die Anwendungsverwaltung in Configuration Manager](/configmgr/apps/understand/introduction-to-application-management)
* [Übersicht über den App-Lebenszyklus in Microsoft Intune](/intune/apps/app-lifecycle)

## <a name="app-installer"></a>App-Installer

Mit dem APP-Installer können Windows 10-apps installiert werden, indem Sie direkt auf ein msix-App-Paket doppelklicken oder indem Sie auf eine appinstaller-Datei doppelklicken, mit der das App-Paket von einem Webserver installiert wird. Dies bedeutet, dass Benutzer nicht PowerShell oder andere Entwicklertools verwenden müssen, um Branchen-apps zu installieren. APP-Installer können auch APP-Pakete installieren, die optionale Pakete und zugehörige Sätze enthalten.

App-Installer kann zur Offlineverwendung im Unternehmen vom Webportal [Microsoft Store für Unternehmen](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) heruntergeladen werden. Weitere Informationen zum APP-Installer finden Sie unter [Installieren von Windows 10-apps mit dem APP-Installer](/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Sideloading

Eine weitere Option zum direkten Verteilen von Lob-apps an Benutzer in Ihrer Organisation ist Sideloading. Diese Option ähnelt der APP-Installations basierten Bereitstellung, da Sie Benutzern die Installation von msix-App-Paketen direkt ermöglicht. Ab Windows 10, Version 2004, ist Sideload standardmäßig aktiviert, und Benutzer können apps installieren, indem Sie auf signierte msix-App-Pakete doppelklicken. Unter Windows 10, Version 1909 und früher, erfordert Sideload eine zusätzliche Konfiguration und die Verwendung eines PowerShell-Skripts. Weitere Informationen finden Sie unter [Querladen von Branchen-Apps in Windows 10](/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store für Unternehmen oder Microsoft Store für Bildungseinrichtungen

Sie können Lob-Apps (Line-of-Business) direkt in Unternehmen veröffentlichen, um die Volumen Übernahme über Microsoft Store für Unternehmen oder Microsoft Store für Bildungseinrichtungen zu ermöglichen, ohne dass die apps im Store allgemein verfügbar sind. Wenn Sie diese Option verwenden, werden die apps vom Store signiert und müssen den standardmäßigen Store-Richtlinien entsprechen.

> [!NOTE]
> Zu diesem Zeitpunkt können nur Kostenlose Apps ausschließlich über Microsoft Store für Unternehmen oder Microsoft Store für Bildungseinrichtungen an Unternehmen verteilt werden. Wenn Sie eine kostenpflichtige app als Lob einreichen, ist Sie für das Unternehmen nicht verfügbar. 

> [!IMPORTANT]
> Sie können die Microsoft Store Übermittlungs- [API](../monetize/create-and-manage-submissions-using-windows-store-services.md) nicht verwenden, um Lob-apps direkt in Unternehmen zu veröffentlichen. Alle Einsendungen für Branchen-apps müssen über Partner Center veröffentlicht werden.

### <a name="set-up-the-enterprise-association"></a>Einrichten der Unternehmens Zuordnung

Der erste Schritt beim exklusiven Veröffentlichen von branchenspezifischen Apps für ein Unternehmen besteht darin, eine Zuordnung zwischen Ihrem Konto und dem privaten Store des Unternehmens einzurichten.

> [!IMPORTANT]
> Dieser Zuordnungs Prozess muss vom Unternehmen initiiert werden und muss die e-Mail-Adresse verwenden, die dem Microsoft-Konto zugeordnet ist, der zum Erstellen des Entwickler Kontos verwendet wurde. Weitere Informationen finden Sie unter [Arbeiten mit LOB-Apps](/microsoft-store/working-with-line-of-business-apps).

Wenn ein Unternehmen Sie zum Veröffentlichen von Apps für die exklusive Nutzung in diesem Unternehmen einlädt, erhalten Sie eine E-Mail mit einem Link, über den Sie die Zuordnung bestätigen können. Sie können diese Zuordnungen auch überprüfen, indem Sie den Abschnitt **Enterprise-Zuordnungen** Ihrer **Kontoeinstellungen** aufrufen (sofern Sie mit dem Microsoft-Konto angemeldet sind, das zum Öffnen des Entwickler Kontos verwendet wurde).

Um die Zuordnung zu bestätigen, klicken Sie auf **Akzeptieren** . Über Ihr Konto können dann Apps zur exklusiven Nutzung durch dieses Unternehmen veröffentlicht werden.

### <a name="submit-lob-apps"></a>Branchen-apps übermitteln

Wenn Sie eine App für die exklusive Nutzung durch ein Unternehmen veröffentlichen möchten, ist das Verfahren vergleichbar mit dem Übermitteln von anderen Apps. Die APP durchläuft denselben [Zertifizierungsprozess](the-app-certification-process.md)und muss alle [Microsoft Store Richtlinien](store-policies.md)einhalten. Dieser Prozess weicht nur bei einigen wenigen Schritten ab.

#### <a name="visibility"></a>Sichtbarkeit

Nachdem Sie eine Unternehmens Zuordnung eingerichtet haben, wird jedes Mal, wenn Sie eine APP einreichen, ein Dropdown-Feld im **Sichtbarkeits** Abschnitt der Seite **Preise und Verfügbarkeit** der Übermittlung angezeigt. Dies ist standardmäßig auf **Einzelhandel – Distribution** festgelegt. Damit die App exklusiv für ein Unternehmen bereitgestellt wird, müssen Sie **Branche – Distribution** auswählen.

Wenn die **Verteilung von Line-of-Business (LOB)** ausgewählt ist, werden die üblichen **Sichtbarkeits** Optionen durch eine Liste der Unternehmen ersetzt, in denen Sie exklusive apps veröffentlichen können. Niemand außerhalb der Unternehmen, die Sie auswählen, kann die App anzeigen oder herunterladen.

Sie müssen mindestens ein Unternehmen auswählen, um eine App als branchenspezifische APP zu veröffentlichen.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Organisationslizenzierung

Standardmäßig ist das Kontrollkästchen **App für Organisationen mit vom Store verwalteter (Online-)Volumenlizenzierung verfügbar machen** aktiviert, wenn Sie eine App übermitteln. Wenn Sie Lob-apps veröffentlichen, muss dieses Kontrollkästchen aktiviert bleiben, damit das Unternehmen Ihre APP auf dem Volume abrufen kann. Dadurch wird die APP nicht mehr für Personen außerhalb der Unternehmen verfügbar gemacht, die Sie im Abschnitt **Verteilung und Sichtbarkeit** ausgewählt haben.

Wenn Sie die App für das Unternehmen über Offline-Lizenzierung verfügbar machen möchten, können Sie zusätzlich das Kontrollkästchen **Getrennte Lizenzierung (offline) für Käufe von Organisationen zulassen** aktivieren.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).

#### <a name="age-ratings"></a>Altersfreigaben

Beim Übermittlungsvorgang von branchenspezifischen Apps ist der Schritt für die [Altersfreigabe](age-ratings.md) mit dem für Einzelhandels-Apps identisch. Sie verfügen jedoch über eine zusätzliche Option, mit der Sie die Store-Altersfreigabe Ihrer App manuell angeben können, anstatt den Fragebogen auszufüllen oder eine vorhandene Freigabe-ID der IARC zu importieren. Diese manuelle Bewertung kann nur mit der Lob-Verteilung verwendet werden. Wenn Sie also jemals die **Sichtbarkeits** Einstellung der app in die **Einzelhandels Verteilung** ändern, müssen Sie den Fragebogen für Alters Bewertungen übernehmen, bevor Sie die Übermittlung veröffentlichen können.

### <a name="enterprise-deployment-of-lob-apps"></a>Unternehmensbereitstellung von branchenspezifischen Apps

Wenn Sie auf **An Store übermitteln** klicken, durchläuft die App den Zertifizierungsprozess. Sobald er bereit ist, muss ein Administrator für das Unternehmen ihn dem privaten Speicher im Microsoft Store for Business oder Microsoft Store for Education Portal hinzufügen. Das Unternehmen kann die App dann für seine Benutzer bereitstellen.

> [!NOTE]
> Um Ihre Branchen-APP zu erhalten, muss sich die Organisation auf einem [unterstützten Markt](/windows/whats-new/windows-store-for-business-overview#supported-markets)befinden, und Sie müssen [diesen Markt](./define-market-selection.md) beim Einreichen Ihrer APP nicht ausschließen. 

Weitere Informationen finden Sie unter [Arbeiten mit Branchen-Apps](/microsoft-store/working-with-line-of-business-apps) und [Verteilen von Apps über den privaten Store](/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Aktualisieren von Lob-apps

Wenn Sie Updates für eine App veröffentlichen möchten, die bereits als branchenspezifische App veröffentlicht wurde, erstellen Sie einfach eine neue Übermittlung. Sie können neue Pakete hochladen oder andere Änderungen vornehmen. Klicken Sie dann auf **An Store übermitteln** , um die aktualisierte Version verfügbar zu machen. Stellen Sie sicher, dass Sie die Unternehmens-Auswahl auf die gleiche **Sichtbarkeit** behalten, es sei denn, Sie möchten absichtlich Änderungen vornehmen, z. b. das Auswählen eines zusätzlichen Unternehmens zum Abrufen der APP oder das Entfernen eines der Unternehmen, in das Sie es zuvor verteilt haben

Wenn Sie eine App, die Sie zuvor als branchenspezifische App veröffentlicht haben, nicht mehr anbieten möchten und keine neuen Käufe möglich sein sollen, müssen Sie eine neue Übermittlung erstellen. Zuerst müssen Sie Ihre **Sichtbarkeits** Auswahl von der **Line-of-Business (LOB)-Verteilung** in die **Einzelhandels Verteilung** ändern. Wählen Sie dann im Abschnitt [Erkennbarkeit](choose-visibility-options.md#discoverability) die Option **dieses Produkt verfügbar, aber nicht im Store** mit der Option für die **Beendigung des Erwerbs** aus.

Nachdem die Übermittlung den Zertifizierungsprozess durchlaufen hat, kann die App nicht mehr neu erworben werden. Benutzer, die sie bereits erworben haben, können sie jedoch weiter verwenden.

> [!NOTE]
> Wenn Sie eine app in die **Einzelhandels Verteilung** ändern, müssen Sie den [Alters Bewertungs-Fragebogen](age-ratings.md) vervollständigen, falls Sie dies noch nicht getan haben, auch wenn die APP nicht für neue Akquisitionen verfügbar ist.
