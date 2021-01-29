---
title: Cortana-Entwurfs Richtlinien-Cortana UWP-Entwurf und-Entwicklung
description: Diese Richtlinien und Empfehlungen beschreiben, wie Ihre APP Cortana für die Interaktion mit dem Benutzer verwenden kann.
ms.assetid: 332ccb95-0e56-410e-ab63-cc028fce4192
label: Cortana
template: detail.hbs
ms.date: 01/27/2021
ms.topic: article
keywords: Cortana, Entwurf
ms.localizationpriority: medium
ms.openlocfilehash: 008a2612b808f1ca9dac5d55fd4472f23d39cef3
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "99041553"
---
# <a name="cortana-design-guidelines"></a>Cortana-Entwurfsrichtlinien

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.

Diese Richtlinien und Empfehlungen beschreiben, wie Ihre App bei der Benutzerinteraktion am besten von **Cortana** profitieren kann, um den Benutzer beim Ausführen einer Aufgabe zu unterstützen und ihm dabei deutlich zu vermitteln, was passiert.

Mithilfe von **Cortana** können im Hintergrund ausgeführte Anwendungen den Benutzer zur Bestätigung oder Klärung auffordern. Im Gegenzug erhält der Benutzer Feedback zum Status des Sprachbefehls. Dies ist ein einfacher, schneller Prozess. Der Benutzer muss dafür weder die **Cortana**-Oberfläche verlassen noch den Kontext für die Anwendung wechseln.

Dem Benutzer soll das Gefühl vermittelt werden, dass **Cortana** den Prozess möglichst komfortabel und einfach macht. Gleichzeitig soll **Cortana** aber auch deutlich machen, dass die Aufgabe von Ihrer App ausgeführt wird.

Wir verwenden eine in die **Cortana**-UI integrierte Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**, um viele der hier vorgestellten Konzepte und Features zu veranschaulichen.

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Screenshot der Cortana-Canvas":::

## <a name="conversational-writing"></a>Unkomplizierte Formulierungen

Für erfolgreiche **Cortana**-Interaktionen müssen Sie einige grundlegenden Prinzipien beim Erstellen von Zeichenfolgen für Text-zu-Sprache (TTS) und GUI befolgen.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Prinzip</th>
<th align="left">Schlechtes Beispiel</th>
<th align="left">Gutes Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>Effizienter</dt>
<dd><p>Fassen Sie sich möglichst kurz, und platzieren Sie die wichtigsten Informationen am Anfang.</p>
</dd>
</dl></td>
<td align="left"><p>Natürlich. Welchen Film möchten Sie heute suchen? Wir besitzen eine große Sammlung.</p></td>
<td align="left"><p>Klar, welchen Film suchen Sie?</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>Dienliche</dt>
<dd><p>Stellen Sie Informationen bereit, die nur für die Aufgabe, die Inhalte und den Kontext relevant sind.</p>
</dd>
</dl></td>
<td align="left"><p>Ich habe das Element Ihrer Wiedergabeliste hinzugefügt. Der Akku ist übrigens bald leer.</p></td>
<td align="left"><p>Ich habe das Element Ihrer Wiedergabeliste hinzugefügt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>Klartext</dt>
<dd><p>Vermeiden Sie Mehrdeutigkeit. Verwenden Sie einfache Formulierungen anstelle von Fachjargon.</p>
</dd>
</dl></td>
<td align="left"><p>Keine Ergebnisse für die Abfrage &quot;Reisen nach Las Vegas&quot;.</p></td>
<td align="left"><p>Ich habe keine Reisen nach Las Vegas gefunden.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>Zuverlässigen </dt>
<dd><p>Formulieren Sie so präzise wie möglich. Erklären Sie, was im Hintergrund geschieht. Falls eine Aufgabe noch nicht beendet ist, behaupten Sie nicht, sie wäre schon abgeschlossen. Respektieren Sie den Datenschutz. Lesen Sie persönliche Informationen nicht laut vor.</p>
</dd>
</dl></td>
<td align="left"><p>Ich habe diesen Film nicht gefunden. Er wurde wohl noch nicht veröffentlicht.</p></td>
<td align="left"><p>Ich habe diesen Film nicht in unserem Katalog gefunden.</p></td>
</tr>
</tbody>
</table>

Orientieren Sie sich bei Formulierungen an einer natürlichen Sprechweise. Stellen Sie grammatische Feinheiten nicht über Natürlichkeit. So können beispielsweise gängige Verkürzungen wie „fürs“ oder „ins“ von TTS ausgegeben werden.

Verwenden Sie die implizierte erste Person, falls dies möglich ist und nicht gekünstelt wirkt. So impliziert beispielsweise „Suche nach Ihrer nächsten Adventure Works-Reise“, dass jemand sucht, ohne dass explizit das Wort „ich“ verwendet wird.

Verwenden Sie Varianten, um die Natürlichkeit Ihrer App zu steigern. Stellen Sie unterschiedliche Versionen Ihrer TTS- und GUI-Zeichenfolgen bereit, die grundsätzlich die gleiche Aussage haben. Beispiel: „Welchen Film möchten Sie sehen?“ könnten Sie als Alternative beispielsweise „Welchen Kinofilm würden Sie gerne anschauen?“ angeben. Niemand sagt immer genau das gleiche. Achten Sie jedoch darauf, dass die TTS-Version mit der GUI-Version übereinstimmt.

Verwenden Sie Wendungen wie „OK“ und „In Ordnung“ in Ihren Antworten mit Bedacht. Sie dienen zwar der Bestätigung und machen deutlich, dass sich etwas tut, können bei zu häufiger Verwendung aber auch störend wirken.

> [!NOTE]
> Verwenden Sie nur Bestätigungs Ausdrücke in "TTS". Aufgrund des begrenzten Platzangebots der **Cortana**-Canvas empfiehlt es sich, Bestätigungen in den entsprechenden GUI-Zeichenfolgen wegzulassen.

Verwenden Sie Verkürzungen, um eine natürlichere Interaktion zu ermöglichen und zusätzlich Platz auf der **Cortana**-Canvas zu sparen. Beispiel: „Ich finde diesen Film nicht“ anstatt „Ich konnte diesen Film nicht finden“. Formulieren Sie fürs Ohr, nicht fürs Auge.

Verwenden Sie die Sprache, die das System versteht. Benutzer neigen dazu, die Begriffe zu wiederholen, die Ihnen angezeigt werden. Stimmen Sie die Formulierung auf den angezeigten Inhalt ab.

Variieren Sie Ihre Antworten, indem Sie durch eine Reihe von verschiedenen Antworten wechseln oder zufällig Antworten aus diesen verschiedenen Antworten auswählen. Beispiel: „Welchen Film möchten Sie sehen?“ und „Welchen Film würden Sie gerne anschauen?“. Dadurch klingt Ihre App natürlicher und individueller.

## <a name="localization"></a>Lokalisierung

Ihre App muss Sprachbefehle in der Sprache erkennen, die der Benutzer auf dem Gerät ausgewählt hat („Einstellungen“ &gt; „System“ &gt; „Sprache“ &gt; „Spracherkennungssprache“), um eine Aktion per Sprachbefehl initiieren zu können.

Lokalisieren Sie die Sprachbefehle, auf die Ihre App reagiert, sowie alle TTS- und GUI-Zeichenfolgen.

Vermeiden Sie lange GUI-Zeichenfolgen. Die **Cortana**-Canvas bietet drei Zeilen für Antworten. Längere Zeichenfolgen werden abgeschnitten.

Weitere Informationen finden Sie im [Abschnitt Globalisierung und Lokalisierung](/windows/uwp/design/globalizing/guidelines-and-checklist-for-globalizing-your-app).

## <a name="image-resources-and-scaling"></a>Bildressourcen und Skalierung

Universelle Windows-Plattform (UWP)-Apps können automatisch das am besten geeignete App-Logobild basierend auf bestimmte Einstellungen und Gerätefunktionen (hoher Kontrast, effektive Pixel, Gebietsschema und so weiter) auswählen. Sie müssen lediglich die Bilder bereitstellen und sicherstellen, dass Sie die entsprechende Benennungskonvention und Ordnerstruktur innerhalb des App-Projekts für die verschiedenen Ressourcenversionen verwenden. Wenn Sie nicht die empfohlenen Ressourcenversionen bereitstellen können, werden unter Umständen je nach Voreinstellungen, Fähigkeiten, Gerätetyp und Standort des Benutzers die Eingabehilfen, Lokalisierung und Bildqualität beeinträchtigt.

Weitere Informationen zu Bild Ressourcen für hohe Kontraste und Skalierungsfaktoren finden Sie unter [Richtlinien für Kachel-und Symbol Objekte](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast).

Sie benennen Ressourcen mithilfe von Qualifizierern. Ressourcenqualifizierer sind Ordner- und Dateinamenmodifikatoren, die den Kontext angeben, in dem eine bestimmte Version einer Ressource verwendet werden soll.

Die standardmäßige Benennungs Konvention ist "FolderName/qualifiername-Value \[ \_ qualifiername-Value \] /filename.qualifiername-Value \[ \_ qualifiername-Value \] . ext". Beispiel: Images/Logo. Scale und 100 an \_contrast-white.png wird einfach in Code mithilfe des Stamm Ordners und Dateiname: Images/logo.png bezeichnet. Weitere Informationen finden Sie unter [Verwalten von Sprache und Region](/windows/uwp/design/globalizing/manage-language-and-region) und [Benennen von Ressourcen mit Qualifizierern](/previous-versions/windows/apps/hh965324(v=win.10)).

Es wird empfohlen, dass Sie die Standardsprache für Zeichen folgen-Ressourcen Dateien (z. b. "en-US \\ Resources. resw") und den Standard Skalierungsfaktor für Bilder (z. b. "logo.scale-100.png") markieren, auch wenn Sie derzeit nicht planen, lokalisierte oder mehrere Auflösungs Ressourcen bereitzustellen. Es wird jedoch empfohlen, dass Sie mindestens Ressourcen für die Skalierungsfaktoren 100, 200 und 400 bereitstellen.

> [!IMPORTANT]
> Das im Titelbereich der Cortana-Canvas verwendete App-Symbol ist das in der Datei „Package.appxmanifest“ angegebene Symbol „Square44x44Logo“.

Sie können auch ein Symbol für jede Ergebniskachel einer Benutzerabfrage angeben. Gültige Bildgrößen für Ergebnissymbole sind:

- 68 (B) x 68 (H)
- 68 (B) x 92 (H)
- 280 (B) x 140 (H)

## <a name="result-tile-templates"></a>Vorlagen für Ergebniskacheln

Für die Ergebniskacheln, die auf der Cortana-Canvas angezeigt werden, werden eine Reihe von Vorlagen bereitgestellt. Verwenden Sie diese Vorlagen, um den Titel der Kachel anzugeben und ob die Kachel Text und ein Ergebnissymbolbild enthält. Jede Kachel kann bis zu drei Zeilen Text und ein Bild enthalten, abhängig von der angegebenen Vorlage.

Folgende Vorlagen werden unterstützt (mit Beispielen):

| Name | Beispiel |
| --- | --- |
| Nur Titel  | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titleonly-small.png" alt-text="Screenshot der Cortana-Canvas mit nur Titel"::: |
| Titel mit Text | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewithtext-small.png" alt-text="Screenshot der Cortana-Canvas mit Titel mit Text"::: |
| Titel mit 68x68-Symbol | Kein Bild |
| Titel mit 68x68-Symbol und Text | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x68iconandtext-small.png" alt-text="Screenshot des Cortana-Zeichen Bereichs mit dem Titel mit dem 68x68-Symbol und Text"::: |
| Titel mit 68x92-Symbol | Kein Bild |
| Titel mit 68x92-Symbol und Text | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x92iconandtext-small.png" alt-text="Screenshot des Cortana-Zeichen Bereichs mit dem Titel mit dem 68x92-Symbol und Text"::: |
| Titel mit 280x140-Symbol | Kein Bild |
| Titel mit 280x140-Symbol und Text | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith280x140iconandtext-small.png" alt-text="Screenshot der Cortana-Canvas mit Titel mit dem Symbol &quot;280x140&quot; und Text"::: |

Weitere Informationen zu Cortana-Vorlagen finden Sie unter [VoiceCommandContentTileType](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType).

## <a name="example"></a>Beispiel

Dieses Beispiel veranschaulicht einen vollständigen Aufgabenablauf für eine Hintergrund-App in **Cortana**. Wir verwenden die **Adventure Works**-App, um eine Reise nach Las Vegas zu stornieren. In diesem Beispiel wird die Vorlage „Titel mit 68x68-Symbol und Text“ verwendet.

:::image type="content" source="images/cortana/e2e-canceltrip.png" alt-text="Screenshot der Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow":::

Im Anschluss finden Sie die einzelnen Schritte aus der Abbildung:

1. Der Benutzer tippt auf das Mikrofon, um **Cortana** zu initiieren.
2. Der Benutzer sagt „Meine Adventure Works-Reise nach Vegas stornieren“, um die **Adventure Works**-App im Hintergrund zu starten. Die App interagiert sowohl über die Sprachfunktion als auch über die Canvas von **Cortana** mit dem Benutzer.
3. Neben einem Übergabebildschirm, der dem Benutzer den Vorgang bestätigt („Das gebe ich gleich an Adventure Works weiter.“), zeigt **Cortana** eine Statusleiste und eine Schaltfläche zum Abbrechen an.
4. In diesem Fall sind für den Benutzer mehrere Reisen vorhanden, die der Anfrage entsprechen. Daher zeigt die App einen Mehrdeutigkeitsvermeidungsbildschirm mit allen passenden Ergebnissen an und fragt, welche Reise der Benutzer stornieren möchte.
5. Der Benutzer gibt das Element „Vegas Tech Conference“ an.
6. Da die Stornierung nicht rückgängig gemacht werden kann, zeigt die App einen Bestätigungsbildschirm an, in dem der Benutzer gebeten wird, den Vorgang zu bestätigen.
7. Der Benutzer sagt „Ja“.
8. Daraufhin zeigt die App einen Abschlussbildschirm mit dem Ergebnis des Vorgangs an.

Im Anschluss gehen wir ausführlicher auf die einzelnen Schritte ein.

### <a name="handoff"></a>Übergabe

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Screenshot der Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow unter Verwendung von AdventureWorks-Abbruch Fahrten ohne Übergabe von":::*AdventureWorks "Abbrechen von Fahrten" ohne Übergabe Bildschirm*

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow unter Verwendung von AdventureWorks Abbrechen von Fahrten mit der Übergabe von":::*AdventureWorks "Abbrechen von Fahrten" mit Übergabe Bildschirm*

Aufgaben, auf die Ihre App in unter 500 ms reagieren kann und für die keine zusätzlichen Informationen vom Benutzer benötigt werden, können abgesehen von der Anzeige des Abschlussbildschirms ohne weitere Beteiligung von **Cortana** durchgeführt werden.

Wenn die Anwendung mehr als 500 ms benötigt, um zu reagieren, stellt **Cortana** einen Übergabebildschirm bereit. Das App-Symbol und der App-Name werden angezeigt, und Sie müssen sowohl eine GUI- als auch eine TTS-Übergabezeichenfolge bereitstellen, um den Benutzer darüber zu informieren, dass der Sprachbefehl korrekt erkannt wurde. Der Übergabebildschirm wird bis zu fünf Sekunden lang angezeigt. Reagiert die App nicht innerhalb dieses Zeitraums, zeigt **Cortana** einen allgemeinen Fehlerbildschirm an.

### <a name="gui-and-tts-guidelines-for-handoff-screens"></a>GUI- und TTS-Richtlinien für Übergabebildschirme

Machen Sie deutlich, dass die Aufgabe bearbeitet wird.

Verwenden Sie Präsens.

Verwenden Sie ein Tätigkeitsverb, um die initiierte Aufgabe zu bestätigen, und geben Sie die spezifische Entität an.

Verwenden Sie ein allgemeines Verb, das sich nicht auf die angeforderte, noch nicht abgeschlossene Aktion bezieht. (beispielsweise „Ihre Reise wird gesucht“ anstelle von „Ihre Reise wird storniert“). Falls keine Ergebnisse zurückgegeben werden, vermeiden Sie dadurch Ausgaben wie „Ihre Reise nach Las Vegas wird storniert... Keine Reise nach Las Vegas gefunden.“

Machen Sie deutlich, dass die Aufgabe noch nicht ausgeführt wurde, falls die App noch die angeforderte Entität auflösen muss. Beachten Sie beispielsweise, dass wir nicht „Ihre Reise wird storniert“ verwenden, sondern „Ihre Reise wird gesucht“, da das Ergebnis noch nicht bekannt ist und die Möglichkeit besteht, dass null oder mehrere Reisen gefunden werden.

Die GUI- und TTS-Zeichenfolgen können identisch sein, dies ist aber nicht zwingend erforderlich. Halten Sie die GUI-Zeichenfolge möglichst kurz, um zu vermeiden, dass die Zeichenfolge abgeschnitten wird oder andere visuelle Objekte dupliziert werden.

| TTS | GUI |
| --- | --- |
| Ihre nächste Adventure Works-Reise wird gesucht... | Ihre nächste Reise wird gesucht... |
| Ihre Adventure Works-Reise nach Falls City wird gesucht... | Reise nach Falls City wird gesucht... |

### <a name="progress"></a>Fortschritt

:::image type="content" source="images/cortana/e2e-canceltrip-progress.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow unter Verwendung von AdventureWorks Abbrechen Trip Progress":::*AdventureWorks "Abbrechen von Fahrten" Fortschritt*

Wenn eine Aufgabe zwischen den einzelnen Schritten etwas mehr Zeit benötigt, muss Ihre App den Benutzer mithilfe eines Statusbildschirms entsprechend informieren. Das App-Symbol wird angezeigt, und Sie müssen sowohl eine GUI- als auch eine TTS-Statuszeichenfolge bereitstellen, die deutlich macht, dass die Aufgabe ausgeführt wird.

Es empfiehlt sich, einen Link mit Startparametern bereitzustellen, über den sich Ihre App im entsprechenden Zustand starten lässt. Dadurch kann der Benutzer die Aufgabe bei Bedarf selbst anzeigen oder ausführen. **Cortana** stellt den Linktext bereit (beispielsweise „Zu Adventure Works“).

Statusbildschirme werden jeweils fünf Sekunden lang angezeigt. Danach muss ein weiterer Bildschirm folgen. Andernfalls tritt für die Aufgabe ein Timeout auf.

Nach einem Statusbildschirm können folgende Bildschirme angezeigt werden:

- Fortschritt
- Bestätigung (explizit; Beschreibung folgt weiter unten)
- Mehrdeutigkeitsvermeidung
- Completion

### <a name="gui-and-tts-guidelines-for-progress-screens"></a>GUI- und TTS-Richtlinien für Statusbildschirme

Verwenden Sie Präsens.

Verwenden Sie ein Tätigkeitsverb, um anzugeben, dass die Aufgabe ausgeführt wird.

**GUI**: Wenn die Entität angezeigt wird, verweisen Sie darauf („Diese Reise wird storniert...“). Wird keine Entität angezeigt, geben Sie die Entität ausdrücklich an („'Vegas Tech Conference' wird storniert...“).

**TTS**: Geben Sie nur für den ersten Statusbildschirm eine TTS-Zeichenfolge an. Wenn weitere Status Bildschirme erforderlich sind, senden Sie eine leere Zeichenfolge, {} als ihre TTS-Zeichenfolge, und geben Sie nur eine GUI-Zeichenfolge an.

| Bedingungen  | TTS | GUI |
| --- | --- | --- |
| ENTITÄT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT AUF DEM DISPLAY ANGEZEIGT     | Diese Reise wird storniert...          | Diese Reise wird storniert...          |
| ENTITÄT NICHT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT AUF DEM DISPLAY ANGEZEIGT | Ihre Reise nach Vegas wird storniert... | Diese Reise wird storniert...          |
| ENTITÄT NICHT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT NICHT ANGEZEIGT        | Ihre Reise nach Vegas wird storniert... | Ihre Reise nach Vegas wird storniert... |

### <a name="confirmation"></a>Bestätigung

:::image type="content" source="images/cortana/e2e-canceltrip-confirmation.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow mithilfe von AdventureWorks Abbrechen der Fahrten Bestätigung":::*AdventureWorks "Abbruch Fahrt" Bestätigung*

Einige Aufgaben können aufgrund der Art des Benutzerbefehls implizit bestätigt werden. Andere sind unter Umständen etwas kniffliger und erfordern eine explizite Bestätigung. Im Anschluss finden Sie einige Richtlinien zur Verwendung expliziter oder impliziter Bestätigungen.

Sowohl die GUI- als auch die TTS-Zeichenfolge für den Bestätigungsbildschirm stammt von Ihrer App. Anstelle des **Cortana**-Avatars wird das App-Symbol angezeigt (sofern vorhanden).

Wenn der Kunde auf die Bestätigung reagiert, muss die Anwendung innerhalb von 500 ms den nächsten Bildschirm bereitstellen, um die Anzeige eines Statusbildschirms zu vermeiden.

Situationen für eine explizite Bestätigung

- Der Benutzer sendet etwas (beispielsweise eine SMS, eine E-Mail oder einen Social Media-Beitrag).
- Eine Aktion kann nicht rückgängig gemacht werden (beispielsweise ein Einkauf oder Löschvorgang).
- Das Ergebnis wäre unter Umständen peinlich (beispielsweise ein Anruf bei der falschen Person).
- Eine komplexere Erkennung ist erforderlich (beispielsweise bei einer offenen Umschreibung).

Situationen für eine implizite Bestätigung

- Inhalte werden nur für den Benutzer gespeichert (beispielsweise ein Memo).
- Der Vorgang kann problemlos rückgängig gemacht werden (beispielsweise durch Aktivieren/Deaktivieren eines Alarms).
- Die Aufgabe muss schnell ausgeführt werden (beispielsweise, um eine Idee aufzuschreiben).
- Die Genauigkeit ist hoch (beispielsweise bei einem einfachen Menü).

### <a name="gui-and-tts-guidelines-for-confirmation-screens"></a>GUI- und TTS-Richtlinien für Bestätigungsbildschirme

Verwenden Sie Präsens.

Stellen Sie dem Benutzer eine eindeutige Frage, die er mit „Ja“ oder „Nein“ beantworten kann. Die Frage muss explizit die gewünschte Aktion des Benutzers bestätigen, und es dürfen keine anderen offensichtlichen Optionen übrig bleiben.

Stellen Sie für den Fall, dass der Sprachbefehl beim ersten Mal nicht verstanden wird, eine Variante für eine Nachfrage bereit.

**GUI**: Wenn die Entität angezeigt wird, verweisen Sie darauf. Wird keine Entität angezeigt, geben Sie die Entität ausdrücklich an.

**TTS**: Verweisen Sie immer auf das spezifische Element oder auf die spezifische Entität, es sei denn, das Element oder die Entität wurde bereits im vorherigen Durchgang ausgegeben.

| Bedingungen | TTS | GUI |
| --- | --- | --- |
| ENTITÄT NICHT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT AUF DEM DISPLAY ANGEZEIGT | Möchten Sie „Vegas Tech Conference“ stornieren? | Diese Reise stornieren?                             |
| ENTITÄT NICHT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT NICHT ANGEZEIGT        | Möchten Sie „Vegas Tech Conference“ stornieren? | „Vegas Tech Conference“ stornieren?                 |
| ENTITÄT IN VORHERIGEM DURCHGANG GELESEN/ENTITÄT NICHT ANGEZEIGT            | Möchten Sie diese Reise stornieren?             | Diese Reise stornieren?                             |
| NACHFRAGE BEI ANGEZEIGTER ENTITÄT                              | Wollten Sie diese Reise stornieren?            | Möchten Sie diese Reise wirklich stornieren?             |
| NACHFRAGE OHNE ANGEZEIGTE ENTITÄT                          | Wollten Sie diese Reise stornieren?            | Wollten Sie „Vegas Tech Conference“ stornieren? |

### <a name="disambiguation"></a>Mehrdeutigkeitsvermeidung

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow mithilfe von AdventureWorks Abbrechen Trip disambiguations":::*AdventureWorks "Abbrechen von Fahrten"*

Bei bestimmten Aufgaben muss der Benutzer unter Umständen eine Auswahl in einer Liste mit Entitäten treffen, um die Aufgabe abschließen zu können.

Sowohl die GUI- als auch die TTS-Zeichenfolge für den Mehrdeutigkeitsvermeidungsbildschirm stammt von Ihrer App. Anstelle des **Cortana**-Avatars wird das App-Symbol angezeigt (sofern vorhanden).

Wenn der Kunde auf die Nachfrage reagiert, muss die Anwendung innerhalb von 500 ms den nächsten Bildschirm bereitstellen, um die Anzeige eines Statusbildschirms zu vermeiden.

### <a name="gui-and-tts-guidelines-for-disambiguation-screens"></a>GUI- und TTS-Richtlinien für Mehrdeutigkeitsvermeidungsbildschirme

Verwenden Sie Präsens.

Stellen Sie dem Benutzer eine eindeutige Frage, die mit dem Titel oder der Textzeile einer der angezeigten Entitäten beantworten werden kann.

Es können bis zu zehn Entitäten angezeigt werden.

Jede Entität muss einen eindeutigen Titel besitzen.

Stellen Sie für den Fall, dass der Sprachbefehl beim ersten Mal nicht verstanden wird, eine Variante für eine Nachfrage bereit.

**TTS**: Verweisen Sie immer auf das spezifische Element oder auf die spezifische Entität, es sei denn, das Element oder die Entität wurde bereits im vorherigen Durchgang ausgegeben.

**TTS**: Lassen Sie die Entitätenliste nur vorlesen, wenn diese maximal drei (kurze) Entitäten enthält.

| Bedingungen                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| NACHFRAGE – MAXIMAL DREI ELEMENTE  | Welche Vegas-Reise möchten Sie stornieren? „Vegas Tech Conference“ oder „Party in Vegas“? | Welche möchten Sie stornieren? |
| NACHFRAGE – MEHR ALS DREI ELEMENTE | Welche Vegas-Reise möchten Sie stornieren?                                          | Welche möchten Sie stornieren? |
| ERNEUTE NACHFRAGE                   | Welche Vegas-Reise wollten Sie stornieren?                                         | Welche möchten Sie stornieren? |

### <a name="completion"></a>Completion

:::image type="content" source="images/cortana/e2e-canceltrip-completion.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow unter Verwendung von AdventureWorks Abbrechen von Trip":::-Abschluss *AdventureWorks "Abbrechen von Fahrten"*

Nach erfolgreichem Abschluss der Aufgabe muss Ihre App den Benutzer darüber informieren, dass die angeforderte Aufgabe erfolgreich abgeschlossen wurde.

Sowohl die GUI- als auch die TTS-Zeichenfolge für den Abschlussbildschirm stammt von Ihrer App. Anstelle des **Cortana**-Avatars wird das App-Symbol angezeigt (sofern vorhanden).

Es empfiehlt sich, einen Link mit Startparametern bereitzustellen, über den sich Ihre App im entsprechenden Zustand starten lässt. Dadurch kann der Benutzer die Aufgabe bei Bedarf selbst anzeigen oder ausführen. **Cortana** stellt den Linktext bereit (beispielsweise „Zu Adventure Works“).

### <a name="gui-and-tts-guidelines-for-completion-screens"></a>GUI- und TTS-Richtlinien für Abschlussbildschirme

Verwenden Sie die Vergangenheitsform.

Verwenden Sie ein Tätigkeitsverb, um explizit anzugeben, dass die Aufgabe abgeschlossen wurde.

Wenn die Entität angezeigt wird oder im vorherigen Durchgang darauf verwiesen wurde, verweisen Sie lediglich darauf.

| Bedingungen                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| ENTITÄT ANGEZEIGT/ENTITÄT IN VORHERIGEM DURCHGANG GELESEN         | Ich habe diese Reise storniert.                       | Diese Reise wurde storniert.               |
| ENTITÄT NICHT ANGEZEIGT/ENTITÄT IN VORHERIGEM DURCHGANG NICHT GELESEN | Ich habe die Reise „Vegas Tech Conference“ storniert. | „Vegas Tech Conference“ wurde storniert. |

### <a name="error"></a>Fehler

:::image type="content" source="images/cortana/e2e-canceltrip-error.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow mit dem AdventureWorks-Trip-Fehler":::*AdventureWorks "Abbruch Fahrt" Fehler*

Bei Auftreten eines der folgenden Fehler zeigt **Cortana** die gleiche allgemeine Fehlermeldung an.

- Der App-Dienst wird unerwartet beendet.
- **Cortana** kann nicht mit dem App-Dienst kommunizieren.
- Von der App wird kein Bildschirm bereitgestellt, nachdem **Cortana** fünf Sekunden lang einen Übergabe- oder Statusbildschirm angezeigt hat.

## <a name="related-articles"></a>Verwandte Artikel

[Cortana-Interaktionen in Windows-apps](cortana-interactions.md)
