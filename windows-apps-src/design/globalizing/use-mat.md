---
Description: Das Multilingual App Toolkit (MAT) 4.0 integriert sich in Microsoft Visual Studio 2017, um Übersetzungsunterstützung, Übersetzungsdateiverwaltung und Editortools für UWP-Apps bereitzustellen.
title: Verwenden des Multilingual App Toolkit
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: c6dc07ff35cdd90deaddff06f89aa585aa63156a
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335028"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Verwenden des Multilingual App Toolkit 4.0

Das Multilingual App Toolkit (MAT) 4.0 integriert sich in Microsoft Visual Studio 2017, um Übersetzungsunterstützung, Übersetzungsdateiverwaltung und Editortools für UWP-Apps bereitzustellen. Hier sind einige der Vorteile des Toolkits.

- Unterstützt Sie beim Verwalten von Ressourcenänderungen und des Übersetzungsstatus während der Entwicklung.
- Stellt eine UI zum Auswählen von Sprachen anhand von konfigurierten Übersetzungsanbietern bereit.
- Unterstützt das Standarddateiformat für die Lokalisierung, XLIFF.
- Stellt eine Engine für eine Pseudosprache bereit, die das Erkennen von Übersetzungsproblemen bei der Entwicklung unterstützt.
- Stellt eine Verbindung mit dem Microsoft-Sprachportal her, damit schnell auf übersetzte Zeichenfolgen und Terminologie zugegriffen werden kann.
- Stellt für schnelle Übersetzungsvorschläge eine Verbindung mit Microsoft Translator her.

## <a name="how-to-use-the-toolkit"></a>So verwenden Sie das Toolkit

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Schritt 1 Entwerfen Sie Ihre App im Hinblick auf Globalisierung und Lokalisierung.

Bevor Sie MAT effektiv nutzen können, muss Ihre App lokalisierbar sein. Insbesondere sollten zu Ihrem Projekt eine oder mehrere Ressourcendateien (.resw) gehören, die die Zeichenfolgen Ihrer App in der Standardsprache enthalten. Details dazu finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im Paketmanifest der App](../../app-resources/localize-strings-ui-manifest.md). Sobald Sie damit fertig sind, können mit dem Toolkit zusätzliche Sprachen schnell und einfach hinzugefügt werden.

Die Vorteile von Globalisierung und Lokalisierung sowie die Begriffe **Globalisierung**, **Lokalisierbarkeit**, und **Lokalisierung** werden unter [Globalisierung und Lokalisierung](globalizing-portal.md) erläutert.

Siehe auch [Richtlinien für Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md) und [App lokalisierbar machen](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Schritt 2 Laden Sie das Multilingual App Toolkit 4.0 herunter, und installieren Sie es.

Das Multilingual App Toolkit 4.0 (MAT) besteht aus zwei Teilen. Zu jedem Teil gehört ein eigenes Installationsprogramm.

- [Multilingual App Toolkit 4.0-Erweiterung für Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Enthält die MAT 4.0-Erweiterung für Visual Studio 2017 in Form eines VSIX-Installationsprogramms.
- [Multilingual App Toolkit 4.0 Editor](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit). Enthält das eigenständige Multilingual Editor-Tool aus MAT 4.0 in Form eines MSI-Installationsprogramms. Zudem ist die MAT 4.0-Erweiterung für Visual Studio 2015 und für Visual Studio 2013 enthalten

Wenn Sie Visual Studio 2017 verwenden, sollten Sie nacheinander beide Installationsprogramme herunterladen und ausführen. Wenn Sie Visual Studio 2015 oder Visual Studio 2013 verwenden, laden Sie das MSI-Installationsprogramm herunter und führen es aus.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Schritt 3 Aktivieren Sie das Multilingual App Toolkit für Ihr Projekt

Bevor Sie mit der Lokalisierung der App beginnen können, muss das MAT für Ihr Projekt aktiviert werden. So aktivieren Sie das Toolkit:

- Öffnen Sie die Projektmappe in Visual Studio.
- Wählen Sie im Projektmappen-Explorer das gewünschte Projekt aus.
- Wählen Sie im Menü **Extras****Multilingual App Toolkit** > **Auswahl aktivieren**. 

Warten Sie, bis im Ausgabefenster (das die Ausgabe des Multilingual App Toolkit enthält) die Nachricht `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>` angezeigt wird. Sobald diese Meldung erscheint, kann MAT verwendet werden.

### <a name="step-4-add-languages-to-your-project"></a>Schritt 4 Fügen Sie Ihrem Projekt Sprachen hinzu.

Führen Sie die folgenden Schritte aus, um Ihrem Projekt Sprachen hinzuzufügen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten.
2. Klicken Sie auf **Multilingual App Toolkit** > **Übersetzungssprachen hinzufügen...**.
3. Wählen Sie im Dialogfeld „Übersetzungssprachen” die Sprachen aus, die sie unterstützen möchten, und klicken Sie dann auf OK.

Das Toolkit führt daraufhin Folgendes aus:

- Für jede Sprache, die Sie hinzugefügt haben, wird ein neuer Ordner erstellt, benannt nach dem [BCP-47-Sprachtag](https://go.microsoft.com/fwlink/p/?linkid=227302) der Sprache. In diesem Ordner werden neue Ressourcendateien (.resw) erstellt, die mit denen übereinstimmen, in denen die Zeichenfolgen der Standardsprache enthalten sind.
- Wenn Sie das erste Mal eine Sprache hinzufügen, wird dem Projekt ein neuer Ordner namens `MultilingualResources` hinzugefügt. Dieser Ordner enthält für jede Sprache eine XLF-Datei. Die XLF-Dateien enthalten eine Übersetzungseinheit für jede Zeichenfolge aus jeder Ressourcendatei (.resw) Ihres Projekts.
- Im Ausgabefenster wird das Hinzufügen der Sprachen bestätigt.

Wenn Sie eine Ressourcendatei für Standardsprachen (.resx) hinzufügen/entfernen oder eine Zeichenfolge in einer Ressourcendatei für Standardsprachen (.resx) hinzufügen/entfernen, müssen Sie das Projekt neu erstellen, um die XLF-Dateien erneut zu synchronisieren. Dadurch wird sichergestellt, dass die XLF-Dateien die Gesamtmenge der Zeichenfolgen in der Standardsprache enthalten.

Installierte Übersetzungsanbieter wie das [Microsoft-Sprachenportal](https://go.microsoft.com/fwlink/p/?LinkId=330295) oder [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220) können zum Übersetzen der Ressourcen Ihrer App verwendet werden. Wenn ein Anbieter eine bestimmte Sprache unterstützt, wird das Symbol des Anbieters im Dialogfeld „Übersetzungssprachen” neben dem Namen der Sprache angezeigt.

Im Dialogfeld "Übersetzungssprachen" wird für alle vorhandenen XLF-basierten Sprachen, die vom Toolkit erkannt werden, das Auswahlkontrollkästchen aktiviert, um anzuzeigen, dass die Sprache bereits im Projekt enthalten ist.

Sobald eine Sprache zum Projekt hinzugefügt wurde, kann sie nicht einfach dadurch entfernt werden, dass Sie das Kontrollkästchen im Dialogfeld „Übersetzungssprachen” deaktivieren. Klicken Sie zum Entfernen einer Sprache mit der rechten Maustaste auf die sprachspezifische XLF-Datei, und wählen Sie **Löschen**. Nach Ihrer Bestätigung wird auch die zugehörige Ressourcendatei (.resw) gelöscht.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Schritt 5 Testen Sie Ihre App mit Pseudosprache.

Pseudosprache ist eine künstliche Modifikation des Softwareprodukts, die eine echte Sprachlokalisierung simulieren soll, aber für Muttersprachler lesbar bleibt. Bei einer Pseudoübersetzung werden Zeichen ersetzt und die Länge der Ressourcenzeichenfolge erweitert, um mögliche Lokalisierungsprobleme oder Fehler früh im Projektzyklus und vor Beginn der Lokalisierung zu erkennen.

Führe Sie die folgenden Schritte aus, um eine Pseudolokalisierung vorzunehmen und das Projekt zu testen.

1. Verwenden Sie das Dialogfeld „Übersetzungssprachen”, um Pseudosprache (Pseudo) [qps-ploc] zu Ihrem Projekt hinzuzufügen.
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Datei `<project-name>.qps-ploc.xlf` und auf **Multilingual App Toolkit** > **Computerübersetzungen generieren**.
3. Klicken Sie unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Sprachen** auf **Sprache hinzufügen**.
5. Geben Sie `qps-ploc` im Suchfeld ein.
6. Klicken Sie zum Hinzufügen auf `English (qps-ploc)`.
7. Wählen Sie `English (qps-ploc)` aus der Sprachenliste, und klicken Sie auf **Als Standard festlegen**.
8. Testen Sie Ihre pseudolokalisierte App. Suchen Sie beispielsweise in der Benutzeroberfläche nach Zeichenfolgen, die nicht vollständig angezeigt werden (die Zeichenfolge ist abgeschnitten) oder nach Zeichenfolgen, die nicht übersetzt sind (weil hartcodiert)

Neben der Ersetzung und Erweiterung von Zeichen stellt das Pseudo-Modul einen eindeutigen Überwachungsbezeichner für jede Ressource bereit. Dieser Bezeichner wird jeder Zeichenfolge vorangestellt und ist in Klammern eingeschlossen `[xxxxx]`. Die Tracker können bei der optischen Überprüfung der Benutzeroberfläche verwendet werden. Sie können dabei helfen, spezifische Ressourcen im Produkt zu finden. Dies gilt insbesondere für mehrfach vorhandene Ressourcen mit ähnlichem oder gleichem Text.

Im folgenden Textbeispiel „Hello World” wird die Pseudoübersetzung erweitert und nimmt 30 % mehr Platz auf dem Bildschirm ein. Anschließend wird die Ressourcenüberwachung angewendet.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Schritt 6 Übersetzen Sie Ihre App in die ausgewählten Sprachen.

Das Multilingual App Toolkit ist in den Erstellungsprozess integriert. Bei einer Erstellung werden der XLF-Datei für die betreffende Sprache automatisch aktualisierte Zeichenfolgen hinzugefügt.
Nachdem Sie Ihre App mit der Pseudosprache getestet haben, gibt es drei Optionen, Ihre App für die Veröffentlichung in andere Sprachen zu übersetzen.

#### <a name="option-1-translate-the-strings-yourself"></a>Option 1: Übersetzen Sie die Zeichenfolgen selbst.

Sie können den Multilingual Editor verwenden, um Zeichenfolgen einzeln zu übersetzen. Wie bereits erwähnt, ist er im [MSI-Installationsprogramm](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit) enthalten.

- Klicken Sie mit der rechten Maustaste auf die XLF-Datei, die Sie übersetzen möchten.
- Klicken Sie auf **Öffnen mit...**, und wählen Sie den Multilingual Editor. Sie können auch auf **Als Standard festlegen** klicken.
- Für jede Zeichenfolge wird unter **Quelle** die ursprüngliche Zeichenfolge in der Standardsprache angezeigt. Unter **Übersetzung** geben Sie die Übersetzung der Zeichenfolge in der entsprechenden Sprache für die XLF-Datei ein, die Sie bearbeiten.
- Wenn Sie fertig sind, speichern und schließen Sie die Datei.

Erstellen Sie Ihr Projekt neu, um zu bewirken, dass die übersetzten Zeichenfolgen in die Ressourcendatei (.resw) kopiert werden, die der gerade bearbeiteten XLF-Datei entspricht.

Sie können auch den Multilingual Editor auch wie folgt starten. Wechseln Sie zum Startmenü, zeigen Sie alle Apps an, öffnen Sie den Ordner mit dem Multilingual App Toolkit, und klicken Sie auf den Multilingual Editor, um ihn zu starten.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Option 2: Senden Sie die XLF-Dateien zur Übersetzung an einen Drittanbieter.

Um die Übersetzungs- und Bearbeitungsaufgaben an Lokalisierer zu übergeben, wählen Sie die gewünschten XLF-Dateien im Projektmappen-Explorer aus. Klicken Sie dann mit der rechten Maustaste darauf und anschließend auf **Multilingual App Toolkit** > **Übersetzungen exportieren...**.

Wählen Sie **Ausgabe: E-Mail-Empfänger** in das Dialogfeld "Exportieren Zeichenfolge Ressourcen", und auf OK, und Ihre Dateien komprimiert und an eine neue e-Mail angefügt. Wählen Sie **Ausgabe: Speicherort**Browser wählen Sie für einen Ordner und klicken Sie auf OK, optional für die Dateien komprimiert werden, und klicken Sie auf OK erneut, Ihre Dateien werden (ZIP und) an der Position, die Sie ausgewählt haben, in einen neuen Ordner mit dem Namen für Ihr Projekt gespeichert.

Nachdem Ihre Lokalisierer die Übersetzungsarbeiten abgeschlossen und Ihnen die übersetzten XLF-Dateien übermittelt haben, können Sie sie in Ihr Projekt importieren. Wählen Sie die gewünschten XLF-Dateien im Projektmappen-Explorer mit der rechten Maustaste sie und klicken Sie auf **Multilingual App Toolkit** > **Import-/Wiederverwendung Übersetzungen...** . Klicken Sie auf **hinzufügen**, navigieren Sie zu der XLF oder ZIP-Dateien, und klicken Sie auf **Import**.

**Hinweis** Beim Importvorgang wird vor dem eigentlichen Import eine grundlegende Überprüfung durchgeführt. Dadurch wird sichergestellt, dass die Zielkultur in den importierten Dateien mit denen in den vorhandenen XLF-Dateien übereinstimmt.

Erstellen Sie Ihr Projekt neu, um zu bewirken, dass die übersetzten Zeichenfolgen in die Ressourcendatei (.resw) kopiert werden, die der gerade importierten XLF-Datei entspricht.

Folgende Drittanbieter bieten Lokalisierungsdienste an und können Ihnen möglicherweise behilflich sein:

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> Die obige Liste dient nur zu Informationszwecken und stellt keine Empfehlung dar. Microsoft übernimmt keine Verantwortung oder Garantie bezüglich diesen Anbietern oder ihren Dienstleistungen. Microsoft schließt jegliche Haftung für die Verwendung dieser Anbieter oder Dienste aus. Alle Fragen, Beschwerden oder Ansprüche in Bezug auf solche Anbieter oder deren Dienste müssen an den entsprechenden Anbieter gerichtet werden.

#### <a name="option-3-use-the-integrated-translation-services"></a>Option 3. Verwenden Sie die integrierten Übersetzungsdienste.

Die Übersetzungsdienste sind in die Entwicklungsumgebung (IDE) von Visual Studio und in den Multilingual Editor integriert. So haben Sie einfachen Zugriff auf die Übersetzungsdienste, während Sie das Produkt entwickeln und die Ressourcen lokalisieren. Für diesen Dienst benötigen Sie ein Azure-Kontoabonnement, wie unter [Microsoft Translator wurde in das Azure-Portal verschoben](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal) beschrieben.

Um auf die Übersetzungsdienste in Visual Studio zuzugreifen, wählen Sie im Projektmappen-Explorer mit der rechten Maustaste die gewünschten XLF-Dateien aus, und klicken Sie auf **Computerübersetzungen generieren**.

Mit dem Multilingual Editor erhalten Sie die gleiche Übersetzungsunterstützung sowie interaktive Übersetzungsvorschläge, sodass Sie jeweils die Übersetzung wählen können, die am besten zu Ihren Ressourcenzeichenfolgen passt. Anhand der Übersetzungsvorschläge können Sie die Zeichenfolge Ihrem Übersetzungsstil entsprechend überarbeiten.

Das Multilingual App Toolkit verfügt über zwei Anbieter.

- Der Anbieter [Microsoft-Sprachportal](https://go.microsoft.com/fwlink/p/?LinkId=330295) ermöglicht die Wiederverwendung von Übersetzungen sowie Terminologieanpassungen. Grundlage hierfür sind Übersetzungen der Benutzeroberflächentexte von Produkten und Diensten von Microsoft.
- Der Anbieter [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220) bietet maschinelle Übersetzungen nach Bedarf.

Sie und Ihre Übersetzer können den Status von Übersetzungen im Mehrsprachen-Editor verwalten, um unsichere Übersetzungen später zu überarbeiten. Sie können festlegen, den Status jeder Zeichenfolge in die **Eigenschaften** Registerkarte. Statuswerte sind: **Neue**, **Needs Review**, **übersetzte**, **endgültige**, und **abgemeldet**. Der Status wird durch eine Markierung links von der Zeile angezeigt. Wenn für alle Zeilen im Multilingual Editor Grün angezeigt wird, ist die Übersetzung beendet.

Erstellen Sie Ihr Projekt neu, um zu bewirken, dass die übersetzten Zeichenfolgen in die Ressourcendatei (.resw) kopiert werden, die der gerade bearbeiteten XLF-Datei entspricht.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Schritt 7 Laden Sie Ihre App in den Microsoft Store hoch.

Vor dem Start des Zertifizierungsprozesses für den Microsoft Store müssen Sie die Datei `<project-name>.qps-ploc.xlf` aus dem Projekt entfernen. Zum Ermitteln potenzieller Lokalisierungsprobleme oder -fehler wird Pseudosprache verwendet. Diese ist jedoch keine gültige Sprache für den Microsoft Store. Falls sie nicht entfernt wird, tritt beim Zertifizierungsprozess für den Microsoft Store ein Fehler auf.

## <a name="related-topics"></a>Verwandte Themen

* [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
* [Globalisierung und Lokalisierung](globalizing-portal.md)
* [Richtlinien für die Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md)
* [Stellen Sie Ihre app lokalisierbar](prepare-your-app-for-localization.md)
* [BCP-47-Sprachtag.](https://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>Downloads

* [Multilingual App Toolkit 4.0 VSIX-installer](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Multilingual App Toolkit 4.0 MSI-Installationsprogramm](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Übersetzungsdienste

* [Microsoft-Sprachenportal](https://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)
