---
description: Das mehrsprachige App Toolkit (Mat) 4,0 ist in Microsoft Visual Studio 2019 integriert, um Windows-apps Übersetzungsunterstützung, Übersetzungsdatei Verwaltung und Editor-Tools bereitzustellen.
title: Verwenden des Multilingual App Toolkit
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 912a92e0eaabbefb4bf3e6f1ac3596d6a2919234
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034333"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Verwenden des Multilingual App Toolkit 4.0

Das mehrsprachige App Toolkit (Mat) 4,0 ist in Microsoft Visual Studio 2019 integriert, um Windows-apps Übersetzungsunterstützung, Übersetzungsdatei Verwaltung und Editor-Tools bereitzustellen. Im folgenden finden Sie einige der Wert Angebote des Toolkits.

- Hilft Ihnen bei der Verwaltung von Ressourcen Änderungen und dem Übersetzungsstatus während der Entwicklung.
- Stellt eine Benutzeroberfläche zum Auswählen von Sprachen basierend auf konfigurierten Übersetzungs Anbietern bereit.
- Unterstützt das standardmäßige XLIFF-Dateiformat der Lokalisierung.
- Stellt eine Pseudo Sprach-Engine zur Verfügung, mit der Übersetzungsprobleme während der Entwicklung identifiziert werden können.
- Stellt eine Verbindung zum Microsoft-Sprach Portal her, um problemlos auf übersetzte Zeichen folgen und Terminologie zuzugreifen
- Stellt eine Verbindung mit dem Microsoft Translator für schnelle Übersetzungs Vorschläge her.

## <a name="how-to-use-the-toolkit"></a>Verwenden des Toolkits

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Schritt 1: Entwerfen Ihrer APP für Globalisierung und Lokalisierung

Bevor Sie die Matte effektiv verwenden können, muss Ihre APP lokalisierbar sein. Insbesondere sollte Ihr Projekt mindestens eine Ressourcen Datei (. resw) enthalten, die die Zeichen folgen ihrer app in der Standardsprache enthält. Weitere Informationen finden Sie unter Lokalisieren von Zeichen folgen [in der Benutzeroberfläche und im App-Paket Manifest](../../app-resources/localize-strings-ui-manifest.md). Nachdem Sie dies getan haben, wird das Hinzufügen zusätzlicher Sprachen durch das Toolkit schnell und einfach.

Informationen zu den Werten der Globalisierung und Lokalisierung &mdash; sowie Definitionen der Begriffe **Globalisierung** , **Lokalisier barkeit** und **Lokalisierung** finden Sie unter &mdash; [Globalisierung und Lokalisierung](globalizing-portal.md).

Beachten Sie auch die [Richtlinien für die Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md) , und [machen Sie Ihre APP lokalisiert](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Schritt 2: Herunterladen und Installieren des mehrsprachigen App-Toolkits 4,0

Es gibt zwei Teile des mehrsprachigen App-Toolkits 4,0 (Matt 4,0), die jeweils über einen eigenen Installer verfügen.

- [Mehrsprachige App-Toolkit 4,0-Erweiterung für Visual Studio 2017 und](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)höher. Diese enthält die Matte 4,0-Erweiterung für Visual Studio 2019 in Form eines VSIX-Installers.
- [Mehrsprachiges App-Toolkit 4,0-Editor](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit). Diese enthält das eigenständige Tool für die mehrsprachige Version von Mat 4,0 in Form eines MSI-Installers. Außerdem enthält Sie die Erweiterung "Mat 4,0" für Visual Studio 2015 und für Visual Studio 2013.

Wenn Sie Visual Studio 2017 oder Visual Studio 2019 verwenden, laden Sie beide Installer herunter, und führen Sie Sie nacheinander aus. Wenn Sie Visual Studio 2015 oder Visual Studio 2013 verwenden, laden Sie das MSI-Installationsprogramm herunter, und führen Sie es aus.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Schritt 3: Aktivieren Sie das mehrsprachige App-Toolkit für Ihr Projekt.

Die Matte muss für Ihr Projekt aktiviert werden, bevor Sie mit dem Lokalisieren der APP beginnen können. So aktivieren Sie das Toolkit.

- Öffnen Sie die Projekt Mappe in Visual Studio.
- Wählen Sie das gewünschte Projekt in Projektmappen-Explorer aus.
- Wählen Sie **im Menü Extras** die Option **mehrsprachiges App-Toolkit**  >  **Auswahl aktivieren** aus. 

Achten Sie auf die Meldung im Ausgabefenster (Ausgabe des mehrsprachigen App-Toolkits wird angezeigt) `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>` . Wenn diese Meldung angezeigt wird, kann die Matte verwendet werden.

### <a name="step-4-add-languages-to-your-project"></a>Schritt 4: Hinzufügen von Sprachen zu Ihrem Projekt

Führen Sie die folgenden Schritte aus, um Ihrem Projekt Sprachen hinzuzufügen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten.
2. Klicken Sie auf **mehrsprachige App Toolkit**  >  **Übersetzungs Sprachen hinzufügen...** .
3. Wählen Sie im Dialogfeld Übersetzungs Sprachen die Sprache (n) aus, die Sie unterstützen möchten, und klicken Sie auf OK.

Das Toolkit führt diese Aktionen als Reaktion aus.

- Für jede hinzugefügte Sprache wird ein neuer Ordner mit dem Namen für das [bcp-47-sprach Kennzeichen](https://tools.ietf.org/html/bcp47) der Sprache erstellt. Innerhalb dieses Ordners werden neue Ressourcen Dateien (. resw) erstellt, die den (n) Dateien entsprechen, die die Standardsprachen Zeichenfolgen enthalten.
- Wenn Sie zum ersten Mal eine Sprache hinzugefügt haben, `MultilingualResources` wird dem Projekt ein neuer Ordner mit dem Namen hinzugefügt. Innerhalb dieses Ordners wird für jede Sprache eine XLF-Datei hinzugefügt. Die XLF-Dateien enthalten eine Übersetzungseinheit für jede Zeichenfolge in jeder Ressourcen Datei (. resw) in Ihrem Projekt.
- Das Fenster Ausgabe bestätigt das Hinzufügen der hinzugefügten Sprache (en).

Wenn Sie eine Standard Sprachressourcen Datei (. resw) hinzufügen oder entfernen oder eine Zeichenfolge in einer Standard Sprachressourcen-Datei (. resw) hinzufügen oder entfernen, erstellen Sie das Projekt neu, um die XLF-Dateien erneut zu synchronisieren. Dadurch wird sichergestellt, dass die XLF-Dateien die Vereinigung der Zeichen folgen in der Standardsprache enthalten.

Installierte Übersetzungsanbieter &mdash; wie das [Microsoft-Sprach Portal](https://www.microsoft.com/Language/) und [Microsoft Translator](https://www.microsofttranslator.com/) &mdash; können verwendet werden, um die Ressourcen Ihrer APP zu übersetzen. Wenn ein Anbieter eine bestimmte Sprache unterstützt, wird das Symbol des Anbieters neben dem Sprachnamen im Dialogfeld Übersetzungs Sprachen angezeigt.

Im Dialogfeld Übersetzungs Sprachen wird für alle vorhandenen XLF-basierten Sprachen, die vom Toolkit ermittelt werden, das ausgewählte Feld ausgewählt, um anzugeben, dass die Sprache bereits im Projekt enthalten ist.

Nachdem dem Projekt eine Sprache hinzugefügt wurde, kann Sie nicht mehr entfernt werden, indem das Kontrollkästchen im Dialogfeld Übersetzungs Sprachen deaktiviert wird. Um eine Sprache zu entfernen, klicken Sie mit der rechten Maustaste auf die sprachspezifische XLF-Datei, und wählen Sie **Löschen** aus. Wenn Sie bestätigen, wird dadurch auch die zugehörige Ressourcen Datei (. resw) gelöscht.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Schritt 5. Testen Ihrer APP mit Pseudo Sprache

Bei der Pseudo Sprache handelt es sich um eine künstliche Änderung des Softwareprodukts, das zum Simulieren der Lokalisierung in der Sprache gedacht ist, aber für Native Referenten lesbar ist. Die Pseudo Übersetzung ersetzt Zeichen und erweitert die Länge der Ressourcen Zeichenfolge, um potenzielle lokalisierbare Probleme oder Fehler zu erkennen, die früh im Projekt Cycle auftreten und bevor die Lokalisierung ernsthaft beginnt.

Führen Sie diese Schritte aus, um das Projekt zu Pseudo lokalisieren und zu testen.

1. Verwenden Sie das Dialogfeld Übersetzungs Sprachen, um dem Projekt Pseudo Sprache (Pseudo) [QPS-ploc] hinzuzufügen.
2. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die `<project-name>.qps-ploc.xlf` Datei, und klicken Sie auf **mehrsprachige App Toolkit**  >  **Computerübersetzung generieren**
3. Klicken Sie unter **Einstellungen**  >  **Zeit & sprach**  >  **Region & sprach**  >  **Sprachen** auf **Sprache hinzufügen** .
5. Geben Sie in das Suchfeld `qps-ploc` ein.
6. Klicken Sie `English (qps-ploc)` , um es hinzuzufügen.
7. Wählen Sie in der Liste Sprache die Option `English (qps-ploc)` **als Standard festlegen** aus, und klicken Sie darauf.
8. Testen Sie Ihre Pseudo lokalisierte app. Suchen Sie z. b. nach Problemen mit dem Benutzeroberflächen Layout, bei denen nicht alle Zeichen folgen angezeigt werden (die Zeichenfolge ist abgeschnitten), oder Zeichen folgen, die nicht übersetzt (aber stattdessen hart codiert) werden.

Zusätzlich zum ersetzen und Erweitern von Zeichen bietet die Pseudo-Engine einen eindeutigen nach Verfolgungs Bezeichner für jede Ressource. Diese Nachverfolgung wird dem Anfang jeder Zeichenfolge vorangestellt und in eckige Klammern eingeschlossen `[xxxxx]` . Sie können diese Tracker während der Prüfung der Tests der visuellen Benutzeroberfläche verwenden. Sie können dabei helfen, bestimmte Ressourcen im Produkt zu ermitteln, insbesondere dann, wenn mehrere Ressourcen über einen ähnlichen oder doppelten Text verfügen.

In dieser "Hello, World!" Textbeispiel: die Pseudo Übersetzung wird um etwa 30 Prozent mehr Bildschirmfläche erweitert und dann die ressourcentracker angewendet.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Schritt 6: Übersetzen der app in ausgewählte Sprachen

Das mehrsprachige App-Toolkit ist in den Buildprozess integriert. Während eines Builds werden jeder Sprache. XLF-Datei automatisch aktualisierte Zeichen folgen hinzugefügt.
Nachdem Sie Ihre APP mit Pseudo Sprache getestet haben, stehen Ihnen drei Optionen zur Verfügung, mit denen Sie Ihre APP für die Veröffentlichung in andere Sprachen übersetzen können.

#### <a name="option-1-translate-the-strings-yourself"></a>Option 1. Zeichen folgen selbst übersetzen

Sie können den mehrsprachigen Editor verwenden, um Zeichen folgen einzeln zu übersetzen. Wie bereits erwähnt, ist dies im [MSI-Installations](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)Programm enthalten.

- Klicken Sie mit der rechten Maustaste auf die XLF-Datei, die Sie übersetzen möchten.
- Klicken Sie auf **Öffnen mit...** , und wählen Sie mehrsprachige Editor Optional können Sie auf **als Standard festlegen** klicken.
- Für jede Zeichenfolge zeigt **Quelle** die ursprüngliche Zeichenfolge in der Standardsprache an. Geben Sie in **Translation** die Zeichenfolge in die entsprechende Sprache für die XLF-Datei ein, die Sie bearbeiten.
- Wenn Sie fertig sind, speichern Sie die Datei, und schließen Sie Sie.

Erstellen Sie das Projekt neu, damit die übersetzten Zeichen folgen in die Ressourcen Datei (. resw) kopiert werden, die der XLF-Datei entspricht, die Sie gerade bearbeitet haben.

Sie können auch den mehrsprachigen Editor wie diesen starten. Wechseln Sie zu Start, alle apps anzeigen, öffnen Sie den Ordner für mehrsprachige App Toolkit, und klicken Sie auf den mehrsprachigen Editor, um ihn zu starten

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Option 2. Senden der XLF-Dateien an einen Drittanbieter zur Übersetzung

Um die Übersetzungs-und Bearbeitungs Arbeit an Lokalisierer auszulagern, wählen Sie die gewünschten XLF-Dateien in Projektmappen-Explorer aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **mehrsprachige App-Toolkit**  >  **Export Übersetzungen exportieren.**

Wählen Sie im Dialogfeld Zeichen folgen Ressourcen exportieren den Eintrag **Ausgabe:** e-Mail-Empfänger aus, und klicken Sie auf OK, und Ihre Dateien werden gezippt und an eine neue e-Mail angehängt. Wählen Sie **Ausgabe: Speicherort des Datei Ordners** , Browser für einen Ordner aus, klicken Sie auf OK, wählen Sie optional für die Dateien aus, die gezippt werden sollen, und klicken Sie erneut auf OK, und Ihre Dateien werden (gezippt und) an dem von Ihnen gewählten Speicherort gespeichert.

Nachdem Ihre Lokalisierer die Übersetzungsarbeit durchgeführt und Ihnen die übersetzten XLF-Dateien gesendet haben, können Sie Sie in Ihr Projekt importieren. Wählen Sie die gewünschten XLF-Dateien in Projektmappen-Explorer aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **mehrsprachige App-Toolkit**  >  **Import/Papier Übersetzung...** . Klicken Sie auf **Hinzufügen** , navigieren Sie zu den XLF-oder ZIP-Dateien, und klicken Sie auf **importieren** .

**Hinweis** Der Import Vorgang führt vor dem Importieren eine grundlegende Validierung aus. Dadurch wird sichergestellt, dass die Zielkultur Informationen in den importierten Dateien mit der in den vorhandenen XLF-Dateien übereinstimmen.

Erstellen Sie das Projekt neu, damit die übersetzten Zeichen folgen in die Ressourcen Dateien (. resw) kopiert werden, die den soeben importierten XLF-Dateien entsprechen.

Diese Drittanbieter bieten Lokalisierungsdienste an und sind möglicherweise in der Lage, Sie zu unterstützen.

- [Elanex](https://www.strakertranslations.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [SDL](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> Die obige Liste wird nur zu Informationszwecken bereitgestellt und wird nicht unterstützt. Microsoft übernimmt keine Verantwortung oder Garantie bezüglich diesen Anbietern oder ihren Dienstleistungen. Microsoft schließt jegliche Haftung für die Verwendung dieser Anbieter oder Dienste aus. Fragen, Beschwerden oder Ansprüche in Bezug auf solche Anbieter oder deren Dienste müssen an den entsprechenden Anbieter weitergeleitet werden.

#### <a name="option-3-use-the-integrated-translation-services"></a>Option 3. Verwenden der integrierten Übersetzungsdienste

Übersetzungsdienste sind in die Visual Studio-IDE und in den mehrsprachigen Editor integriert. Dies ermöglicht den einfachen Zugriff auf Übersetzungsdienste während der Entwicklung Ihres Produkts und der Lokalisierung Ihrer Ressourcen. Für diesen Dienst benötigen Sie ein Azure-Konto Abonnement, wie unter [Microsoft Translator wechselt zum Azure-Portal](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal)beschrieben.

Um auf die Übersetzungsdienste in Visual Studio zuzugreifen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf eine oder mehrere XLF-Dateien, und klicken Sie dann auf **Computerübersetzung generieren** .

Der mehrsprachige Editor bietet dieselbe Übersetzungsunterstützung und fügt interaktive Übersetzungs Vorschläge hinzu, mit denen Sie die Übersetzung auswählen können, die ihren Ressourcen Zeichenfolgen am besten entspricht. Nachdem der Übersetzungs Vorschlag bereitgestellt wurde, können Sie die Zeichenfolge für den Übersetzungsstil optimieren.

Zwei Anbieter werden mit dem mehrsprachigen App-Toolkit ausgeliefert.

- Der [Microsoft-Sprach Portal](https://www.microsoft.com/Language/) Anbieter ermöglicht die Unterstützung von Übersetzungs-und terminologieübereinstimmungen auf der Grundlage von Übersetzungen des Benutzeroberflächen Texts für Microsoft-Produkte und-Dienste.
- Der [Microsoft Translator](https://www.microsofttranslator.com/) -Anbieter aktiviert Bedarfs gesteuerte maschinelle Übersetzungsdienste.

Sie und ihre Konvertierer können den Status von Übersetzungen im mehrsprachigen Editor verwalten, um später unsichere Übersetzungen zu überprüfen. Sie können den Status der einzelnen Zeichen folgen auf der Registerkarte " **Eigenschaften** " festlegen. Die Status Werte lauten " **neu** ", " **überprüfen** ", " **übersetzt** **", "** abgeschlossen" und " **Abmelden** " Der Indikator links neben der Zeile zeigt den Status an. Wenn alle Zeilen im mehrsprachigen Editor grün angezeigt werden, erfolgt die Übersetzung der Arbeit.

Erstellen Sie das Projekt neu, damit die übersetzten Zeichen folgen in die Ressourcen Dateien (. resw) kopiert werden, die den soeben bearbeiteten XLF-Dateien entsprechen.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Schritt 7. Hochladen der app in den Microsoft Store

Bevor Sie den Microsoft Store Zertifizierungsprozess starten, müssen Sie die `<project-name>.qps-ploc.xlf` Datei aus dem Projekt ausschließen. Die Pseudo Sprache wird verwendet, um mögliche Lokalisier barkeits Probleme oder Fehler zu erkennen, aber es handelt sich nicht um eine gültige Microsoft Store Sprache. Wenn Sie nicht entfernt wird, schlägt Ihre APP während des Microsoft Store Zertifizierungsprozesses fehl.

## <a name="related-topics"></a>Verwandte Themen

* [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
* [Globalisierung und Lokalisierung](globalizing-portal.md)
* [Richtlinien für Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md)
* [App lokalisierbar machen](prepare-your-app-for-localization.md)
* [BCP-47-Sprachtag](https://tools.ietf.org/html/bcp47)

## <a name="downloads"></a>Downloads

* [Mehrsprachiges App-Toolkit 4,0. vsix-Installer](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Mehrsprachiges App-Toolkit 4,0. MSI-Installer](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Übersetzungsdienste

* [Microsoft-Sprach Portal](https://www.microsoft.com/Language/)
* [Microsoft Translator](https://www.microsofttranslator.com/)
