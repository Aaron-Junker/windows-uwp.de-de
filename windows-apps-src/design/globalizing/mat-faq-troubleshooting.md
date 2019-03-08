---
Description: Dieses Thema enthält Antworten auf häufig gestellte Fragen und Probleme in Bezug auf das Multilingual App Toolkit (MAT) 4.0.
title: Multilingual App Toolkit – Fragen und Antworten sowie Problembehandlung
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 2e27256fbf19ed31a7b087e94dea9e5514db516f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648265"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>Multilingual App Toolkit 4.0 – Häufig gestellte Fragen und Problembehandlung

Dieses Thema enthält Antworten auf häufig gestellte Fragen und Probleme in Bezug auf das Multilingual App Toolkit (MAT) 4.0.

Weitere Informationen finden Sie unter [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

**Hinweis:** Das Toolkit unterstützt sowohl RESW-Dateien (XAML) als auch .resjson-Dateien (JavaScript). Aber in diesem Thema werden nur RESW-Dateien behandelt. Eine RESW-Datei wird Ressourcendatei genannt. Sie enthält Zeichenfolgen, entweder in der Standardsprache oder in eine anderen Sprache übersetzt. Der Ordner mit einer RESW-Datei wird in der Regel nach dem Wert eines Sprachtags benannt.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>Benötige ich RESW-Dateien in mehreren Sprachen?

Nein. Ein großer Vorteil des Toolkits besteht darin, dass RESW-Dateien nicht in mehreren Sprachen erforderlich sind. Die Verwaltung und Synchronisierung der Ressourcen Ihrer App durch das Toolkit erfolgt mithilfe von XLF-Dateien. Dadurch entfallen die Probleme, die sich ergeben können, wenn die Inhalte mehrerer RESW-Dateien synchronisiert werden müssen.

Bei Projekten mit übereinstimmenden .resw- und XLF-Dateien werden die Übersetzungen aus der XLF-Datei ignoriert. In diesem Fall wird beim Erstellen eine Warnung mit dem Hinweis angezeigt, dass die XLF-Übersetzungen nicht in die finale App einbezogen werden. Eine .resw- und eine XLF-Datei stimmen überein, wenn sie eine Zielsprache mit demselben Sprachcode enthalten. Ein Beispiel für ein übereinstimmendes Paar wären eine `Strings\de-DE\Resources.resw`-Datei und ein `<project-name>.de-DE.xlf`-Datei (mit `target-language="de-DE"`).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>Kann ich RESW-Dateien in mehreren Sprachen verwenden?

Dies ist möglich, wird jedoch nicht empfohlen. Wenn Sie Ihrem Projekt mehrsprachige RESW-Dateien hinzufügen und das Toolkit verwenden möchten, müssen Sie darauf achten, dass keine übereinstimmenden .resw- und XLF-Dateien vorhanden sind.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>Im Menü „Extras” ist keine Option zum Aktivieren des Multilingual App Toolkit verfügbar.

Führen Sie folgende Schritte aus.

- Stellen Sie sicher, dass Sie den Projektknoten und nicht den Projektmappenknoten auswählen, bevor Sie das Menü **Tools** öffnen.
- Vergewissern Sie sich, dass die Toolkiterweiterung mithilfe des Erweiterungs-Managers von Visual Studio installiert wurde.
- Stellen Sie sicher, dass Ihr Projekt ein UWP-Projekt ist.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Beim Erstellen meines Projekts wird keine Meldung angezeigt, dass ein Multilingual App Toolkit-Build gestartet wurde.

Stellen Sie sicher, dass Sie MAT für Ihr Projekt aktiviert haben. Wählen Sie im Menü **Extras****Multilingual App Toolkit** > **Auswahl aktivieren**. Wenn Ihr Projekt mit einer früheren Version aktiviert wurde, müssen Sie das MAT im Menü **Extras** zunächst deaktivieren und anschließend erneut aktivieren. Dadurch wird das Projekt aktualisiert, sodass es mit der neuen Version des Toolkits verwendet werden kann.

Vergewissern Sie sich, dass die Komponente „Build Task for all Visual Studio editions” installiert wurde. Diese Buildkomponente wird zusammen mit der Erweiterung installiert, kann jedoch bei der Installation manuell deaktiviert werden. Diese Komponente wird benötigt, um die XLF-Dateien zu aktualisieren und die Übersetzung in die PRI-Datei einzufügen. Bei einer ordnungsgemäßen Installation und Funktion der Komponente werden die folgenden Build-Meldungen angezeigt.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>Das Toolkit meldet, dass bei der Erstellung keine XLIFF-Sprachdateien gefunden wurden.

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Diese Meldung wird angezeigt, wenn das Toolkit keine Dateien mit der Erweiterung XLF im Projekt gefunden hat. Das Toolkit generiert und behält diese Dateien standardmäßig im Ordner `MultilingualResources`. Sie können diese Dateien verschieben. Es wird jedoch empfohlen, die Dateien in diesem Ordner zu belassen, da der Multilingual Editor so auf die zugehörigen Metadatendateien zugreifen kann.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Meine XLF-Datei ist nicht in der Liste der Dateien enthalten, die vom Toolkit Rahmen der Erstellung verarbeitet wurden.

Bei XLF-Dateien, die zunächst ausgeschlossen und anschließend wieder eingeschlossen werden, wird das Dateitypelement möglicherweise nicht ordnungsgemäß festgelegt. Wählen Sie die Datei in Visual Studio, und überprüfen Sie das Eigenschaftenfenster. Die Buildaktion der Datei sollte auf „XliffResource” und „In Ausgabeverzeichnis kopieren” sollte auf „Nicht kopieren” festgelegt sein. Die Referenz in Ihrer Projektdatei sollte wie folgt aussehen.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>Ich habe mehrere XLF-basierte Sprachen hinzugefügt. Wo sind meine Zeichenfolgen?

Die Ressourcendatei (.resw) für die Standardsprache ist Ihr kanonischen „Schema” der Zeichenfolgen, die Ihre App verwendet. Übersetzte Ressourcendateien können alle oder nur eine Teilmenge dieser Zeichenfolgen enthalten.

Wenn Sie Ihr Projekt erstellen, werden Ihre Ressourcendateien und die XLF-Dateien synchronisiert.

- Die XLF-Dateien werden aktualisiert, um alle hinzugefügten oder entfernten Zeichenfolgen oder hinzugefügte oder entfernte Ressourcendateien zu berücksichtigen.
- Die Ressourcendateien werden so aktualisiert, dass sie alle übersetzten Zeichenfolgen in den XLF-Dateien berücksichtigen.

Dies wird ausführlich erläutert in [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Bei der Erstellung meines Projekts bleiben die XLF-Dateien leer.

Bevor Sie MAT effektiv nutzen können, muss Ihre App lokalisierbar sein. Dies wird ausführlich erläutert in [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>Was ist Microsoft Translator?

Microsoft Translator ist ein Cloud-Dienst für Maschinenübersetzungen. Die Maschinenübersetzung bietet eine gute Möglichkeit, um fremdsprachige Texte zu erhalten, wenn der Aufwand für eine professionelle Übersetzung durch Menschen unverhältnismäßig hoch erscheint. Weitere Informationen erhalten Sie unter [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220).

Das Toolkit nutzt den Microsoft Translator-Dienst, um Ihnen Übersetzungsvorschläge anzuzeigen. Wenn das Microsoft Translator-Symbol im Fenster für Übersetzungssprachen angezeigt wird, können Sie sehen, welche Sprachen von Microsoft Translator unterstützt werden.

Das Übersetzen Ihrer App mit Microsoft Translator ist ganz einfach. Wählen Sie im Mehrsprachen-Editor eine Zeichenfolge aus, und klicken Sie auf die Schaltfläche **Übersetzen**.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>Was ist Pseudosprache und was bedeutet Pseudo-Ressourcenüberwachung?

Pseudosprache ist eine künstliche Modifikation des Softwareprodukts, das eine echte Sprachlokalisierung simulieren soll. Sie finden weitere Informationen zu Pseudosprache und Pseudo-Ressourcenüberwachung in [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Wie richte ich Pseudosprache als meine bevorzugte Sprache ein, sodass ich meine in Pseudosprache lokalisierten Zeichenfolgen testen kann?

Dies wird ausführlich erläutert in [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>Welche Arten von Lokalisierungsproblemen kann ich mithilfe der Pseudosprache erkennen?

Dies wird ausführlich erläutert in [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Wenn ich meine App starte, werden keine Übersetzungen angezeigt, oder meine App ist nur teilweise übersetzt.

Öffnen Sie die XLF-Datei im Mehrsprachen-Editor, um zu überprüfen, ob die Übersetzungen vorhanden sind. Wenn Zeichenfolgen in der RESW-Standardsprachendatei explizit geändert werden, werden alle entsprechenden Übersetzungen aus den XLF-Dateien entfernt. Dadurch wird sichergestellt, dass eine Übersetzung ihren Quellzeichenfolgen entspricht. Übersetzen Sie die Zeichenfolgen in den XLF-Dateien, und erstellen Sie sie neu, um die RESW-Dateien für die Nichtstandardsprachen zu aktualisieren.

Wenn Ihre Zeichenfolgen in den XLF-Dateien übersetzt sind, aber nicht in Ihrer App angezeigt werden, erstellen Sie Ihr Projekt neu, um die RESW-Dateien für die Nichtstandardsprachen zu aktualisieren. Visual Studio optimiert den Build-Befehl, sodass nur die Dateien aktualisiert werden, die seit dem letzten Build geändert wurden.

Überprüfen Sie die Reihenfolge Ihrer Sprachpräferenzen. Vergewissern Sie sich unter **Einstellungen**, dass die Sprache, die Sie gerade testen, in den Sprachpräferenzen an erster Stelle steht.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>Das Toolkit meldet den Fehler 0x80004004 in der Buildausgabe.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Diese Nachricht kann angezeigt werden, wenn ein Konflikt zwischen dem regionalen Format und dem Buildvorgang im Toolkit auftritt. Sie können das Problem jedoch umgehen, indem Sie unter **Einstellungen** für den Buildvorgang „en-US” festlegen.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>Das Toolkit meldet den Fehler 0x80004005 in der Buildausgabe.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Diese Nachricht kann angezeigt werden, wenn die XLF-Datei eine nicht unterstützte Zielsprache enthält. Beispielsweise ist „zh-cht” falsch (muss durch „zh-hant” ersetzt werden), und auch „zh-chs” ist falsch (muss durch „zh-hans” ersetzt werden).

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>Gibt es eine Möglichkeit, weitere Informationen zu den angezeigten Fehler zu erhalten?

Ja. Aktivieren Sie die ausführliche Protokollierung in Visual Studio. Klicken Sie auf **Tools** > **Optionen** > **Projekte und Projektmappen** > **Erstellen und ausführen**. Ändern Sie die Einstellung **Ausführlichkeit der MSBuild-Projektbuildausgabe** von „Minimal” in „Normal oder höher”.

Auch durch Ausführung von „MSBuild” über die Befehlszeile können weitere Meldungen angezeigt werden.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Fehler beim Importieren der Übersetzung

Beim Importvorgang wird vor dem eigentlichen Import eine grundlegende Überprüfung durchgeführt. Dadurch wird sichergestellt, dass die Zielkultur in den importierten Dateien mit denen in den vorhandenen XLF-Dateien übereinstimmt. Öffnen Sie die XLF-Dateien im Mehrsprachen-Editor, und vergewissern Sie sich, dass die Kulturinformationen übereinstimmen.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>Was passiert, wenn mein Übersetzer eines der Produkte Windows 10, Visual Studio und Multilingual App Toolkit nicht installiert hat?

Bei der Auswahl **Ausgabe: E-Mail-Empfänger** Ressourcen Zeichenfolge "Export" im Dialogfeld die e-Mail enthält einen Link zum Herunterladen und installieren das Multilingual App Toolkit (MAT) 4.0. Ihre Übersetzer kann den eigenständigen Mehrsprachen-Editor aus MAT 4.0 auch ohne Windows 10 oder Visual Studio installieren.

Weitere Informationen finden Sie unter [Verwenden des Multilingual App Toolkit 4.0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>Was wurde aus den Dateien `MarkupRules.xml` und `ResourcesLocks.xml`?

Das Multilingual App Toolkit 4.0 verwendet keine proprietären Ressourcensperrdateien. Stattdessen wird das XLIFF 1.2-Tag `<mrk>` direkt der XLF-Datei hinzugefügt, um Zeichenfolgen zu kennzeichnen, die nicht von der maschinellen Übersetzung geändert werden. Dies ermöglicht eigenständige XLIFF-Dateien sowie die dateibasierte Sperrung von Ressourcen.

Diese zusätzliche Unterstützungsdateien werden nicht mehr benötigt und können problemlos gelöscht werden (falls vorhanden)

## <a name="what-happened-to-the-tpx-file"></a>Was wurde aus der TPX-Datei?

Mithilfe der TPX-Datei konnten die Dateien `MarkupRules.xml` und `ResourcesLocks.xml` beim Senden der XLF-Datei zum Übersetzen auf einfache Weise eingefügt werden. Diese Funktionalität ist nicht mehr erforderlich.

Wenn Sie Dateien aus einer TPX-Datei benötigen, können Sie einfach die Erweiterung ändern und aus der TPX-Datei eine ZIP-Datei machen. So können Sie die Inhalte mit dem Datei-Explorer oder einem anderen ZIP-kompatiblen Tool öffnen und extrahieren.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Ich habe alle Anweisungen befolgt, aber es funktioniert immer noch nicht.

Führen Sie folgende Schritte aus.

1. Fügen Sie die Übersetzungen mit einer der bereits beschriebenen Methoden hinzu.
2. Lesen Sie die PRI-Datei aus (s. dazu [Befehlszeilenoptionen für MakePri.exe](../../app-resources/makepri-exe-command-options.md)), um zu überprüfen, ob die Übersetzungen darin enthalten sind. Übersetzungen werden wie folgt mit Sprachcode und übersetzten Wert angezeigt.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Beim Erstellen über eine Eingabeaufforderung enthält der resultierende Fehler möglicherweise mehr Details als in der Buildausgabe gemeldet.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Meine App hat keine Zertifizierung für den Microsoft Store erhalten.

Vor dem Start des Zertifizierungsprozesses für den Microsoft Store müssen Sie die Datei `<project-name>.qps-ploc.xlf` aus dem Projekt entfernen. Zum Ermitteln potenzieller Lokalisierungsprobleme oder -fehler wird Pseudosprache verwendet. Diese ist jedoch keine gültige Sprache für den Microsoft Store. Falls sie nicht entfernt wird, tritt beim Zertifizierungsprozess für den Microsoft Store ein Fehler auf.

## <a name="related-topics"></a>Verwandte Themen

* [Mit dem Multilingual App Toolkit 4.0](use-mat.md)
* [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)
* [MakePri.exe-Befehlszeilenoptionen](../../app-resources/makepri-exe-command-options.md)
