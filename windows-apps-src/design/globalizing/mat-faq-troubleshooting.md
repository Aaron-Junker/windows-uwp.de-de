---
description: Dieses Thema enthält Antworten auf häufig gestellte Fragen und Probleme im Zusammenhang mit dem mehrsprachigen App Toolkit (Mat) 4,0.
title: FAQ zum mehrsprachigen App-Toolkit & Problembehandlung
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 86c7805f92adf3551729783e2359c85103a0c13e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030753"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>Multilingual App Toolkit 4.0: häufig gestellte Fragen und Problembehandlung

Dieses Thema enthält Antworten auf häufig gestellte Fragen und Probleme im Zusammenhang mit dem mehrsprachigen App Toolkit (Mat) 4,0.

Siehe auch [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md).

**Hinweis** Das Toolkit unterstützt sowohl resw-(XAML) als auch resjson-Dateien (JavaScript). In diesem Thema wird jedoch nur auf resw-Dateien verwiesen. Eine resw-Datei wird als Ressourcen Datei bezeichnet. Sie enthält Zeichen folgen, entweder in der Standardsprache oder in eine andere Sprache übersetzt. Der Ordner, der eine resw-Datei enthält, wird in der Regel für den Wert eines sprach Tags benannt.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>Benötige ich resw-Dateien in mehreren Sprachen?

Nein. Einer der Hauptvorteile des-Toolkits besteht darin, dass resw-Dateien in mehreren Sprachen nicht erforderlich sind. Das Toolkit verwaltet und synchronisiert die Ressourcen ihrer App mithilfe von XLF-Dateien. Dadurch werden die Herausforderungen beseitigt, die bei der Synchronisierung von Inhalten über mehrere resw-Dateien hinweg auftreten.

Projekte, die übereinstimmende resw-und XLF-Dateien enthalten, bewirken, dass die Übersetzungen aus der XLF-Datei ignoriert werden. In diesem Fall wird während des Builds eine Warnung angezeigt, um Sie darüber zu informieren, dass die XLF-Übersetzungen nicht in der endgültigen App enthalten sind. Eine resw-Datei und eine XLF-Datei stimmen überein, wenn Sie über eine Zielsprache mit dem gleichen Sprachcode verfügen. Ein Beispiel für ein entsprechendes paar wäre `Strings\de-DE\Resources.resw` und eine- `<project-name>.de-DE.xlf` Datei (mit `target-language="de-DE"` ).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>Kann ich resw-Dateien in mehreren Sprachen haben?

Dies ist zwar möglich, wird aber nicht empfohlen. Wenn Sie resw-Dateien in mehrere Sprachen in Ihr Projekt einschließen und das Toolkit verwenden möchten, stellen Sie sicher, dass keine übereinstimmenden resw-und XLF-Dateien vorhanden sind.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>Im Menü Extras wird keine Option angezeigt, um das mehrsprachige App-Toolkit zu aktivieren.

Führen Sie die folgenden Schritte aus.

- Stellen Sie sicher, dass Sie den Projekt Knoten und nicht den Projektmappenknoten auswählen, bevor **Sie das Menü** Extras öffnen.
- Vergewissern Sie sich, dass die Toolkit-Erweiterung mit dem Erweiterungs-Manager von Visual Studio installiert wurde.
- Vergewissern Sie sich, dass Ihr Projekt ein UWP-Projekt ist.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Beim Erstellen des Projekts wird keine Meldung angezeigt, die besagt, dass ein mehrsprachiges App-Toolkit-Build gestartet wurde.

Vergewissern Sie sich, dass Sie die Matte für Ihr Projekt aktiviert haben. Wählen Sie **im Menü Extras** die Option **mehrsprachiges App-Toolkit**  >  **Auswahl aktivieren** aus. Wenn das Projekt mit einer früheren Version aktiviert wurde, deaktivieren Sie die Matte **mithilfe des Menüs** Extras, und aktivieren Sie Sie dann erneut. Dadurch wird das Projekt so aktualisiert, dass es mit der neuen Version des Toolkits funktioniert.

Stellen Sie sicher, dass die Komponente "BuildTask für alle Visual Studio-Editionen" installiert wurde. Diese Buildkomponente wird mit der Erweiterung installiert, kann aber während der Installation manuell deaktiviert werden. Diese Komponente ist erforderlich, um die XLF-Dateien zu aktualisieren und die Übersetzung in die PRI-Datei einzufügen. Wenn diese Komponente installiert ist und ordnungsgemäß funktioniert, werden diese Buildmeldungen angezeigt.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>Das Toolkit meldet, dass während des Builds keine XLIFF-Sprachdateien gefunden wurden.

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Diese Meldung wird angezeigt, wenn das Toolkit keine Dateien im Projekt mit der Erweiterung. XLF findet. Das Toolkit generiert und speichert diese Dateien `MultilingualResources` standardmäßig im Ordner. Sie können verschoben werden. Es ist jedoch am besten, Sie in diesem Ordner zu belassen, da dadurch der mehrsprachige Editor die zugehörigen Metadatendateien finden kann.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Meine XLF-Datei ist nicht in der Liste der Dateien enthalten, die vom Toolkit während des Builds verarbeitet werden.

Wenn Sie eine XLF-Datei manuell aus dem Projekt ausschließen und Sie dann erneut einschließen, ist das Dateityp Element möglicherweise nicht ordnungsgemäß festgelegt. Wählen Sie in Visual Studio die Datei aus, und überprüfen Sie die Eigenschaftenfenster. Die Buildaktion der Datei sollte auf xliffresource festgelegt sein, und in das Ausgabeverzeichnis kopieren sollte auf nicht kopieren festgelegt sein. Auf diese Weise sollte der Verweis in der Projektdatei aussehen.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>Ich habe XLF-basierte Sprachen hinzugefügt. Wo sind meine Zeichen folgen?

Die Ressourcen Datei für die Standardsprache (. resw) ist das kanonische Schema der Zeichen folgen, die Ihre APP verwendet. Übersetzte Ressourcen Dateien können alle oder eine Teilmenge dieser Zeichen folgen enthalten.

Wenn Sie Ihr Projekt erstellen, werden die Ressourcen Dateien und die XLF-Dateien synchronisiert.

- Die XLF-Dateien werden so aktualisiert, dass Sie alle hinzugefügten oder entfernten Zeichen folgen oder Ressourcen Dateien hinzugefügt oder entfernt wurden.
- Die Ressourcen Dateien werden so aktualisiert, dass alle übersetzten Zeichen folgen in den XLF-Dateien reflektiert werden.

Dies wird im Abschnitt [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md)ausführlich erläutert.

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Beim Erstellen des Projekts bleiben die XLF-Dateien leer.

Bevor Sie die Matte effektiv verwenden können, muss Ihre APP lokalisierbar sein. Dies wird im Abschnitt [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md)ausführlich erläutert.

## <a name="what-is-microsoft-translator"></a>Was ist Microsoft Translator?

Microsoft Translator ist ein cloudbasierter Dienst, der eine computerbasierte Übersetzung bereitstellt. Die maschinelle Übersetzung ist ideal, um Zugriff auf die Übersetzung zu erhalten, wenn die Benutzer Übersetzung nicht angemessen ist. Weitere Informationen finden Sie unter [Microsoft Translator](https://www.microsofttranslator.com/).

Das Toolkit verwendet den Microsoft Translator-Dienst, um Ihnen Übersetzungs Vorschläge bereitzustellen. Sie können sehen, welche Sprachen von Microsoft Translator unterstützt werden, wenn das Microsoft Translator-Symbol im Dialogfeld Übersetzungs Sprachen vorhanden ist.

Sie können Ihre APP schnell mithilfe von Microsoft Translator innerhalb des mehrsprachigen Editors übersetzen, indem Sie eine Zeichenfolge auswählen und auf **übersetzen** klicken.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>Was ist Pseudo Sprache und was sind Pseudo Ressourcen-Tracker?

Bei der Pseudo Sprache handelt es sich um eine künstliche Änderung des Softwareprodukts, das zum Simulieren der Lokalisierung realer Sprachen vorgesehen ist Weitere Informationen über Pseudo Sprache und Pseudo Ressourcen-Tracker finden Sie unter [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Gewusst wie meine bevorzugte Sprache auf Pseudo Sprache festgelegt, damit ich meine Pseudo-Loc-Zeichen folgen testen kann?

Dies wird in [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md)erläutert.

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>Welche Art von lokalisierbaren Problemen kann ich mithilfe der Pseudo Sprache finden?

Dies wird in [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md)erläutert.

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Ich sehe keine Übersetzungen, wenn ich meine APP starte, oder meine APP ist nur teilweise übersetzt.

Öffnen Sie im mehrsprachigen Editor die XLF-Datei, um festzustellen, ob die Übersetzungen vorhanden sind. Wenn Zeichen folgen in der Datei "Standardsprache. resw" explizit geändert werden, werden alle entsprechenden Übersetzungen aus XLF-Dateien entfernt. Dadurch wird sichergestellt, dass eine Übersetzung mit der Quell Zeichenfolge übereinstimmt. Übersetzen Sie die Zeichenfolge (e) in die XLF-Datei (en), und erstellen Sie Sie neu, um die nicht standardmäßige sprach-resw-Datei (en) zu aktualisieren.

Wenn Ihre Zeichen folgen in die XLF-Datei (en) übersetzt werden, aber nicht in der App angezeigt werden, erstellen Sie das Projekt neu, um die nicht standardmäßigen resw-Datei (en) zu aktualisieren. Visual Studio optimiert den Build-Befehl, um nur Dateien zu erstellen, die sich seit dem letzten Build geändert haben.

Überprüfen Sie Ihre bevorzugte Reihenfolge der Sprache. Stellen Sie sicher, dass die zu testende Sprache in den **Einstellungen** oben in der Liste mit den bevorzugten Sprachen aufgeführt wird.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>Das Toolkit meldet den Fehler 0x80004004 in der Buildausgabe.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Diese Meldung kann angezeigt werden, wenn das Regions Format mit dem Toolkit-Buildvorgang in Konflikt steht. Die Problem Umgehung besteht darin, die Sprache in den **Einstellungen** beim Aufbau in "en-US" zu ändern.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>Das Toolkit meldet den Fehler 0x80004005 in der Buildausgabe.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Diese Meldung kann angezeigt werden, wenn die XLF-Datei eine nicht unterstützte Zielsprache enthält. Beispielsweise ist "zh-CHT" falsch (ändern Sie es in "zh-Hant"), und "zh-CHS" ist falsch (ändern Sie es in "zh-Hans").

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>Gibt es eine Möglichkeit, weitere Informationen über die Fehler zu finden, die ich sehe?

Ja, Sie können die ausführliche Protokollierung in Visual Studio aktivieren. Klicken **Sie** auf Extras  >  **Optionen**  >  **Projekte und Projekt** Mappen  >  **Erstellen und ausführen** . Ändern Sie die **Ausführlichkeit der MSBuild-Projektbuildausgabe** von minimal in normal oder höher.

Durch das Ausführen von MSBuild über die Befehlszeile können auch zusätzliche Meldungen erzeugt werden.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Fehler bei der Import Übersetzung

Der Import Vorgang führt vor dem Importieren eine grundlegende Validierung aus. Dadurch wird sichergestellt, dass die Zielkultur Informationen in den importierten Dateien mit der in den vorhandenen XLF-Dateien übereinstimmen. Öffnen Sie die XLF-Dateien im mehrsprachigen Editor, und stellen Sie sicher, dass die Kultur Informationen übereinstimmen.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>Was geschieht, wenn mein Translator nicht über Windows 10 und/oder Visual Studio und/oder das mehrsprachige App-Toolkit installiert ist?

Wenn Sie im Dialogfeld Zeichen folgen Ressourcen exportieren den Eintrag **Ausgabe:** e-Mail-Empfänger auswählen, enthält die e-Mail einen Link zum herunterladen und Installieren des mehrsprachigen App-Toolkits (Mat) 4,0. Der Konvertierer kann weiterhin das Tool für eigenständige mehrsprachige Tools von Mat 4,0 auch ohne Windows 10 und Visual Studio installieren.

Weitere Informationen finden Sie unter [Verwenden des mehrsprachigen App-Toolkits 4,0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>Was ist mit den `MarkupRules.xml` Dateien und passiert `ResourcesLocks.xml` ?

Das mehrsprachige App Toolkit 4,0 verwendet keine proprietären Ressourcen Sperrdateien. Stattdessen wird das Tag XLIFF 1,2 `<mrk>` direkt der XLF-Datei hinzugefügt, um Zeichen folgen zu identifizieren, die während der maschinellen Übersetzung nicht geändert werden. Dadurch kann die XLIFF-Datei eigenständig sein, und es wird eine dateibasierte Ressourcen Sperre ermöglicht.

Diese zusätzlichen Unterstützungs Dateien werden nicht mehr benötigt, und Sie können Sie auf sichere Weise löschen.

## <a name="what-happened-to-the-tpx-file"></a>Was ist mit der TPX-Datei passiert?

Die TPX-Datei bietet eine einfache Möglichkeit, die `MarkupRules.xml` -und-Dateien einzuschließen, `ResourcesLocks.xml` als die XLF-Datei für die Übersetzung gesendet wurde. Diese Funktionalität ist nicht mehr erforderlich.

Wenn Sie Übersetzungen in einer TPX-Datei verfügen, die Sie abrufen müssen, benennen Sie einfach die Dateierweiterung. TPX in zip um. Auf diese Weise können Sie den Inhalt mit dem Datei-Explorer oder einem beliebigen Zip-kompatiblen Tool öffnen und extrahieren.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Ich denke, ich habe alles richtig gemacht, funktioniert aber immer noch nicht.

Führen Sie die folgenden Schritte aus.

1. Fügen Sie Übersetzungen mithilfe einer der bereits beschriebenen Methoden hinzu.
2. Sichern Sie die PRI-Datei (siehe [MakePri.exe Befehlszeilenoptionen](../../app-resources/makepri-exe-command-options.md)), um festzustellen, ob sich Ihre Übersetzungen in der PRI-Datei befinden. Übersetzungen werden mit Sprachcode und übersetztem Wert wie dieser angezeigt.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Build über eine Eingabeaufforderung der resultierende Fehler weist möglicherweise mehr Details auf, als in der Buildausgabe gemeldet werden.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Fehler bei der Zertifizierung meiner app für die Microsoft Store

Bevor Sie den Microsoft Store Zertifizierungsprozess starten, müssen Sie die `<project-name>.qps-ploc.xlf` Datei aus dem Projekt ausschließen. Die Pseudo Sprache wird verwendet, um mögliche Lokalisier barkeits Probleme oder Fehler zu erkennen, aber es handelt sich nicht um eine gültige Microsoft Store Sprache. Wenn Sie nicht entfernt wird, schlägt Ihre APP während des Microsoft Store Zertifizierungsprozesses fehl.

## <a name="related-topics"></a>Verwandte Themen

* [Verwenden des Multilingual App Toolkit 4.0](use-mat.md)
* [Microsoft Translator](https://www.microsofttranslator.com/)
* [Befehlszeilenoptionen für „MakePri.exe“](../../app-resources/makepri-exe-command-options.md)
