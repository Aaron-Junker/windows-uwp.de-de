---
Description: In diesem Artikel werden bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen beschrieben.
title: Richtlinien für App-Einstellungen
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9c35bc6cfcf36543482bcb42d0ccda7fd3102ac9
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970035"
---
# <a name="guidelines-for-app-settings"></a>Richtlinien für App-Einstellungen

App-Einstellungen sind die vom Benutzer anpassbaren Teile Ihrer Windows-App, auf die über eine App-Einstellungenseite zugegriffen wird. Beispielsweise kann ein Benutzer in einer Newsreader-App angeben, welche neuen Quellen oder wie viele Spalten auf dem Bildschirm angezeigt werden sollen, und eine Wetter-App könnte dem Benutzer ermöglichen, zwischen Celsius und Fahrenheit zu wählen. Dieser Artikel enthält Empfehlungen und bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen.

## <a name="when-to-provide-a-settings-page"></a>Wann sollte eine Einstellungenseite angegeben werden?

Hier sind Beispiele für App-Optionen, die zu einer Seite für App-Einstellungen gehören:

- Konfigurationsoptionen, die sich auf das Verhalten der App auswirken und keine häufige erneute Anpassung erfordern (z. B. die Auswahl der Standardtemperatureinheit Celsius oder Fahrenheit in einer Wetter-App, das Ändern der Kontoeinstellungen für eine E-Mail-App, Einstellungen für Benachrichtigungen oder Barrierefreiheitsoptionen).
- Optionen, die von den bevorzugten Benutzereinstellungen abhängen, wie Musik, Soundeffekte oder Farbdesigns.
- App-Infos, die eher selten benötigt werden, z. B. Datenschutzrichtlinie, Hilfe, App-Version oder Copyright-Informationen.

Befehle, die Teil des typischen App-Workflows sind (z. B. das Ändern der Pinselgröße in einer Zeichen-App), sollten sich nicht auf einer Einstellungsseite befinden. Weitere Informationen zur Platzierung von Befehlen finden Sie unter [Befehlsdesigngrundlagen](https://docs.microsoft.com/windows/uwp/layout/commanding-basics).

## <a name="general-recommendations"></a>Allgemeine Empfehlungen

- Halten Sie die Einstellungsseiten möglichst einfach, und verwenden Sie binäre Steuerelemente (an/aus). Ein [Umschalter](../controls-and-patterns/toggles.md) ist in der Regel das beste Steuerelement für binäre Einstellungen.
- Verwenden Sie [Optionsfelder](../controls-and-patterns/radio-button.md) für Einstellungen, mit denen Benutzer aus bis zu fünf zusammenhängenden Optionen, die sich gegenseitig ausschließen, eine auswählen können.
- Erstellen Sie einen Einstiegspunkt für alle App-Einstellungen auf der Einstellungsseite Ihrer App.
- Verwenden Sie einfache Einstellungen. Definieren Sie intelligente Standardwerte, und halten Sie die Anzahl von Einstellungen so gering wie möglich.
- Wenn ein Benutzer eine Einstellung ändert, sollte die App die Änderung sofort wiedergeben.
- Fügen Sie keine Befehle hinzu, die zu einem häufig verwendeten App-Workflow gehören.

## <a name="entry-point"></a>Einstiegspunkt

Die Methode, mit der Benutzer auf die Einstellungsseite Ihrer App zugreifen, sollte auf dem Layout Ihrer App basieren.

**Navigationsbereich**

Bei einem Navigationsbereichs-Layout sollten App-Einstellungen das letzte Element in der Navigationsliste und am Ende der Liste angeheftet sein:

![App-Einstellungen-Einstiegspunkt für Navigationsbereich](images/appsettings-entrypoint-navpane.png)

**App-Leiste**

Bei Verwendung einer [App-Leiste](../controls-and-patterns/app-bars.md) oder Toolleiste sollte der Einstiegspunkt für die Einstellungen das letzte Element im Überlaufmenü „Mehr“ sein. Wenn Sie den Einstiegspunkt für die Einstellungen in Ihrer App intuitiver positionieren möchten, platzieren Sie ihn direkt auf der App-Leiste und nicht im Überlaufmenü.

![App-Einstellungen-Einstiegspunkt für die App-Leiste](images/appsettings-entrypoint-tabs.png)

**Hub**

Wenn Sie ein Hublayout verwenden, sollte sich der Einstiegspunkt für App-Einstellungen im Überlaufmenü „Mehr“ einer App-Leiste befinden.

**Registerkarten/Pivots**

Bei einem Registerkarten- oder Pivots-Layout raten wir davon ab, den Einstiegspunkt für App-Einstellungen als eines der Elemente der obersten Ebene in der Navigation zu platzieren. Stattdessen sollte der Einstiegspunkt für App-Einstellungen im Überlaufmenü „Mehr“ einer App-Leiste platziert werden.

**Master/Details**

Anstatt den Einstiegspunkt für App-Einstellungen innerhalb eines Master/Details-Bereichs zu u verstecken, machen Sie ihn als letztes angeheftete Element auf der obersten Ebene des Masterbereichs verfügbar.

## <a name="layout"></a>Layout


Auf Desktops und Mobilgeräten sollte sich das Fenster der App-Einstellungen im Vollbildmodus öffnen und das gesamte Fenster ausfüllen. Wenn das App-Einstellungsmenü über bis zu vier Gruppen auf oberster Ebene verfügt, sollten diese Gruppen eine Spalte nach unten kaskadieren.

Desktop:

![Layout der Seite für App-Einstellungen auf Desktops](images/appsettings-layout-navpane-desktop.png)

Mobiltelefon:

![Layout der Seite für App-Einstellungen auf Smartphones](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>Einstellungen für den Farbmodus


Wenn Ihre App Benutzern die Auswahl des Farbmodus ermöglicht, machen Sie diese Optionen mit [Optionsfeldern](../controls-and-patterns/radio-button.md) oder einem [Kombinationsfeld](../controls-and-patterns/combo-box.md) mit dem Header „Wählen Sie einen App-Modus“ verfügbar. Die Optionen sollten lauten:
- Leicht
- Dunkel
- Standard (Windows)

Wir empfehlen außerdem, einen Hyperlink zur Seite „Farben” der Windows-Einstellungen-App hinzuzufügen. Dort können Benutzer auf den aktuellen App-Standardmodus zugreifen und ihn ändern. Verwenden Sie die Zeichenfolge „Windows-Farbeinstellungen“ als Text für den Link.

![Abschnitt für die Modusauswahl](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](https://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>Abschnitt „Info” und Feedbackschaltfläche


Wir empfehlen Ihnen, den Abschnitt „Info/Über diese App” entweder als eigene Seite oder in einem eigenen Abschnitt in Ihrer App zu platzieren. Wenn Sie eine „Feedback senden”-Schaltfläche benötigen, platzieren Sie diese unten auf der Seite „Info/Über diese App”.

Unter einer Unterüberschrift „Rechtliches” können Sie alle „Nutzungsbedingungen” und „Datenschutzerklärung” (sollten [Hyperlinkschaltflächen](../controls-and-patterns/hyperlinks.md) mit Textumbruch sein) sowie weitere rechtliche Informationen, wie z. B. das Urheberrecht, einfügen.

![Abschnitt „Info“ mit Schaltfläche „Feedback“](images/appsettings-about.png)


## <a name="recommended-page-content"></a>Empfohlene Seiteninhalte


Wenn Sie eine Liste der gewünschten Elemente auf der Seite für App-Einstellungen erstellt haben, berücksichtigen Sie die folgenden Richtlinien:

- Gruppieren Sie ähnliche oder verwandte Einstellungen unter einer Einstellungsbezeichnung.
- Versuchen Sie, die Gesamtanzahl der Einstellungen auf maximal vier oder fünf zu begrenzen.
- Zeigen Sie unabhängig vom App-Kontext dieselben Einstellungen an. Sind einige Einstellungen in einem bestimmten Kontext nicht relevant, deaktivieren Sie sie im Einstellungen-Flyout der App.
- Verwenden Sie für Einstellungen informative Beschriftungen, die nur aus einem Wort bestehen. Nennen Sie für kontobezogene Einstellungen die Einstellung „Konten“ statt „Kontoeinstellungen“. Wenn Sie nur eine Option für Ihre Einstellungen festlegen möchten und die Einstellungen selbst sich nicht als Beschriftung eignen, verwenden Sie „Optionen“ oder „Standardeinstellungen“.
- Wird über eine Einstellung direkt das Web anstelle eines Flyouts aufgerufen, informieren Sie den Benutzer mit einem als [Hyperlink](../controls-and-patterns/hyperlinks.md) formatierten visuellen Hinweis darüber, z. B. mit „Hilfe (online)“ oder „Webforen“. Mehrere Weblinks sollten Sie in einem Flyout mit einer einzelnen Einstellung gruppieren. Beispiel: Die Einstellung „Info“ könnte ein Flyout mit Links zu Ihren Nutzungsbedingungen, Datenschutzbestimmungen und App-Support-Infos öffnen.
- Fassen Sie selten verwendete Einstellungen zu einem einzelnen Eintrag zusammen, damit für gängigere Einstellungen jeweils ein eigener Eintrag zur Verfügung steht. Fassen Sie Inhalte oder Links, die nur Informationen enthalten, unter der Einstellung „Info“ zusammen.
- Wiederholen Sie die Funktionen nicht im Berechtigungsbereich. Windows stellt diesen Bereich standardmäßig bereit, und er kann nicht geändert werden.

- Hinzufügen von Einstellungsinhalten zum Einstellungen-Flyout
- Stellen Sie Inhalte von oben nach unten in einer einzelnen, ggf. bildlauffähigen Spalte dar. Beschränken Sie den Bildlauf auf die doppelte Bildschirmhöhe.
- Verwenden Sie die folgenden Steuerelemente für App-Einstellungen:

    - [Umschalter](../controls-and-patterns/toggles.md): Benutzer können Werte aktivieren oder deaktivieren.
    - [Optionsfelder](../controls-and-patterns/radio-button.md): Benutzer können aus bis zu fünf zusammenhängenden Optionen, die sich gegenseitig ausschließen, eine auswählen.
    - [Texteingabefeld](../controls-and-patterns/text-block.md): Benutzer können hier Text eingeben. Wählen Sie die Art des Texteingabefelds passend zum Typ des vom Benutzer erhaltenen Texts aus, beispielsweise für E-Mail-Adressen oder Kennwörter.
    - [Hyperlinks](../controls-and-patterns/hyperlinks.md): Benutzer werden zu einer anderen Seite innerhalb der App oder zu einer externen Website weitergeleitet. Wenn ein Benutzer auf einen Hyperlink klickt, wird das Einstellungen-Flyout geschlossen.
    - [Schaltflächen](../controls-and-patterns/buttons.md): Benutzer können eine Aktion sofort ausführen, ohne das aktuell geöffnete Einstellungenflyout zu schließen.
- Wenn eines der Steuerelemente deaktiviert ist, fügen Sie eine beschreibende Meldung hinzu. Platzieren Sie diese Meldung über dem deaktivierten Steuerelement.
- Zeigen Sie Inhalte und Steuerelemente als einzelnen Block per Animation an, nachdem das Einstellungen-Flyout und die Überschrift eingeblendet wurden. Animieren Sie Inhalte mit der Animation [**enterPage**](https://docs.microsoft.com/previous-versions/windows/apps/br212672(v=win.10)) oder [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) mit einem Offset links von 100 Pixel.
- Verwenden Sie Abschnittsüberschriften, Absätze und Bezeichnungen, um Inhalte ggf. zu organisieren und zu erläutern.
- Verwenden Sie zum Wiederholen von Einstellungen eine zusätzliche UI-Ebene oder ein Model zum Erweitern und Reduzieren. Beschränken Sie Hierarchien jedoch auf maximal zwei Ebenen. So könnte zum Beispiel in einer Wetter-App, deren Einstellungen sich auf die jeweilige Stadt beziehen, eine Liste mit Städten angezeigt werden. Der Benutzer muss dann nur auf die gewünschte Stadt zu tippen, um ein neues Flyout zu öffnen oder zu erweitern, um die Einstellungsoptionen anzuzeigen.
- Wenn das Laden von Steuerelementen oder Webinhalten längere Zeit in Anspruch nimmt, informieren Sie den Benutzer mithilfe eines unbestimmten Statussteuerelements darüber, dass Informationen geladen werden. Weitere Informationen finden Sie unter [Richtlinien für Statussteuerelemente](https://docs.microsoft.com/windows/uwp/controls-and-patterns/progress-controls).
- Verwenden Sie keine Schaltflächen für die Navigation oder zum Übernehmen von Änderungen. Verwenden Sie Hyperlinks, um zu anderen Seiten zu navigieren, und speichern Sie Änderungen an App-Einstellungen automatisch, wenn ein Benutzer das Einstellungen-Flyout schließt, anstatt eine Schaltfläche für das Übernehmen von Änderungen zu verwenden.



## <a name="related-articles"></a>Verwandte Artikel

* [Befehlsdesigngrundlagen](https://docs.microsoft.com/windows/uwp/layout/commanding-basics)
* [Richtlinien für Statussteuerelemente](https://docs.microsoft.com/windows/uwp/controls-and-patterns/progress-controls)
* [Speichern und Abrufen von App-Daten](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition)
