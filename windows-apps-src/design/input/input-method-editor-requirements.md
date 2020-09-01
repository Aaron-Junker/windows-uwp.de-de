---
Description: Entwickeln Sie einen benutzerdefinierten Eingabemethoden-Editor (IME), um einem Benutzer die Eingabe von Text in einer Sprache zu erleichtern, die auf einer Standardtastatur der QWERTY nicht leicht dargestellt werden kann.
title: Anforderungen an den Eingabemethoden-Editor (IME)
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: IME, Eingabemethoden-Editor, Eingabe, Interaktion
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5a34c15826bff757b7c4277b87cc5fed53a6f109
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160004"
---
# <a name="custom-input-method-editor-ime-requirements"></a>Anforderungen an den benutzerdefinierten Eingabemethoden-Editor (IME)

Diese Richtlinien und Anforderungen können Sie bei der Entwicklung eines benutzerdefinierten Eingabemethoden-Editors (IME) unterstützen, mit dem Benutzereingaben in einer Sprache durchführen können, die auf einer standardmäßigen QWERTY-Tastatur nicht leicht dargestellt werden kann.

Eine Übersicht über die IMEs finden Sie unter [Eingabemethoden-Editor (IME)](input-method-editors.md).

## <a name="default-ime"></a>Standard-IME

Benutzer können eine ihrer aktiven IMEs (**Settings-> Time & Language-> Language-> bevorzugte Sprachen-> Language Pack-Optionen**) als Standard-IME für Ihre bevorzugte Sprache auswählen.

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="Bevorzugte Spracheinstellung":::

Wählen Sie auf dem Bildschirm Sprachoptionen Einstellungen die Standardtastatur für die bevorzugte Sprache aus.

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="Tastatur für bevorzugte Sprache":::

> [!Important]
> Es wird nicht empfohlen, direkt in die Registrierung zu schreiben, um die Standardtastatur für Ihren benutzerdefinierten IME festzulegen.

## <a name="compatibility-requirements"></a>Kompatibilitätsanforderungen

Im folgenden sind die grundlegenden Kompatibilitätsanforderungen für eine benutzerdefinierte IME aufgeführt.

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME muss mit Windows-Apps kompatibel sein.

Verwenden Sie das [Text Services-Framework (TSF)](/windows/win32/tsf/text-services-framework) zum Implementieren von IMEs. Zuvor haben Sie die Möglichkeit, den [Eingabemethoden-Manager (imm32)](/windows/win32/intl/input-method-manager) für Eingabe Dienste zu verwenden. Nun blockiert das System die mit Input Method Manager (imm32) implementierten IMEs.

Wenn eine APP gestartet wird, lädt TSF die IME-DLL für den IME, der derzeit vom Benutzer ausgewählt wird. Wenn eine IME geladen wird, unterliegt Sie denselben App-Container Einschränkungen wie die app. So kann z. b. ein IME nicht auf das Internet zugreifen, wenn eine APP im Manifest keinen Internet Zugriff angefordert hat. Durch dieses Verhalten wird sichergestellt, dass IMEs keine Sicherheits Verträge verletzen kann.

TSF ist der Vermittler zwischen der APP und Ihrem IME. TSF kommuniziert Eingabeereignisse an den IME und empfängt Eingabezeichen aus dem IME, nachdem der Benutzer ein Zeichen ausgewählt hat.

Dieses Verhalten ist mit früheren Versionen von Windows identisch, aber das Laden in eine Windows-App wirkt sich auf die möglichen Funktionen eines IME aus.

Wenn Ihr IME unterschiedliche Funktionen oder Benutzeroberflächen zwischen Windows-apps und Desktop-Apps bereitstellen muss, müssen Sie sicherstellen, dass die von TSF geladene DLL überprüft, in welche Art von APP Sie geladen wird. Nennen Sie die [itfthreadmgrex:: getactiveflags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) -Methode in Ihrem IME, und überprüfen Sie das TF_TMF_IMMERSIVEMODE-Flag, sodass Ihr IME abhängig vom Ergebnis eine andere Anwendungslogik auslöst.

Windows-apps unterstützen keine TTS (Table Text Service)-IMEs.

> [!NOTE]
> Einige Tools zum Erstellen von TTS-IMEs sind IMEs, die von Windows als Malware gekennzeichnet sind.

### <a name="ime-must-be-compatible-with-the-system-tray"></a>Der IME muss mit der Taskleiste kompatibel sein.

Es ist keine sprach Leiste zum Hosten von IME-Symbolen vorhanden. Stattdessen zeigt ein Eingabe Indikator auf der Taskleiste an, die die aktuelle Eingabeoption angibt. Der Eingabe Indikator zeigt nur das IME-Branding-Symbol an, um den derzeit laufenden IME anzugeben. Außerdem gibt es ein IME-Modus-Symbol, das auf der linken Seite des IME-brandingsymbols angezeigt wird, damit Benutzer den am häufigsten verwendeten IME-Modusschalter ausführen können, wie z. b. das Aktivieren oder Deaktivieren des IME.

Der Eingabe Indikator zeigt das IME-Branding-Symbol und das Mode-Symbol nur für kompatible IMEs an. Bei nicht kompatiblen nicht kompatiblen Symbolen sind das Branding-Symbol und das Mode-Symbol in der Taskleiste nicht angezeigt. Stattdessen zeigt der Eingabe Indikator die sprach Abkürzung anstelle des IME-Branding-Symbols an.

Speichern Sie die IME-Symbole in einer DLL-oder exe-Datei anstelle einer eigenständigen ICO-Datei. Der Entwurf von IME-Symbolen muss den Richtlinien folgen, die im folgenden Abschnitt Richtlinien zum Entwerfen von Benutzeroberflächen beschrieben werden

### <a name="ime-branding-icon"></a>Symbol für IME-Branding

Der Eingabe Indikator Ruft das IME-Branding-Symbol von der IME-DLL mithilfe der Ressourcen-ID ab, die durch den IME definiert wurde, als er im System registriert wurde.

### <a name="ime-mode-icon"></a>Symbol für IME-Modus

Einige IMEs müssen sich möglicherweise auf den Eingabe Indikator stützen, der auf der Taskleiste angezeigt wird, um das Symbol für den IME-Modus anzuzeigen. In diesem Fall übergibt der IME das IME-Modus-Symbol mithilfe von GUID_LBI_INPUTMODE an den Eingabe Indikator.

Wenn Sie die Symbole des IME-Modus an den Eingabe Indikator in der Taskleiste übergeben, beträgt die Standardgröße des IME-Modus-Symbols 16x16 Pixel. Die Skalierung der Benutzeroberfläche folgt dpi.

Wenn das Symbol für den IME-Modus an den Eingabe Indikator in der Benutzerkontensteuerung (Benutzerkontensteuerung in Secure Desktop) übergeben wird, beträgt die Standardgröße des IME-Modus-Symbols 20 x 20 Pixel. Das Symbol für die Benutzeroberflächen Skalierung für das IME-Modus auf der Benutzerkontensteuerung folgt

## <a name="ime-must-work-in-app-container"></a>IME muss im App-Container arbeiten

Einige IME-Funktionen sind in einem App-Container betroffen.

- **Wörterbuchdateien** : häufig verfügen IMEs über schreibgeschützte Wörterbuchdateien, um Benutzereingaben bestimmten Zeichen zuzuordnen. Um auf diese Dateien aus einem App-Container zugreifen zu können, muss Ihr IME diese Dateien unter den Programmen "Programme" oder "Windows" platzieren. Standardmäßig können diese Verzeichnisse aus einem App-Container gelesen werden, sodass Sie auf Wörterbuchdateien zugreifen können, die an diesen Speicherorten gespeichert sind. Wenn Ihr IME die Wörterbuchdatei an einem anderen Speicherort speichern muss, muss Sie die [Access Control Listen (ACL)](/windows/win32/secauthz/access-control-lists) der Wörterbuchdateien explizit manipulieren, um den Zugriff von App-Containern zu ermöglichen.
- **Internet Aktualisierung** : Wenn Ihr IME die Wörterbücher mithilfe von Daten aus dem Internet aktualisieren muss, kann dies innerhalb eines App-Containers nicht zuverlässig durchzuführen sein, da der Internetzugang nicht immer zulässig ist. Stattdessen sollte Ihr IME einen separaten Desktop Prozess ausführen, der für das Aktualisieren der Wörterbuchdateien mit Daten aus dem Internet zuständig ist.
- **Das dynamische lernen** : Wenn ein IME in einem App-Container ausgeführt wird, der über Internet Zugriff verfügt, gibt es keine Einschränkung für die Endpunkte, mit denen der IME kommunizieren kann. In diesem Fall kann ein IME einen Cloud-Server verwenden, um dynamische Lerndienste bereitzustellen. Einige IMEs laden Benutzereingaben herunter und laden Sie dynamisch hoch, während der Benutzer eine Eingabe vornimmt. Da der Internet Zugriff in einem App-Container nicht garantiert wird, ist dies möglicherweise nicht immer zulässig.
- **Freigabe von Informationen zwischen Prozessen** : möglicherweise müssen Sie Daten über die Eingabeeinstellungen des Benutzers zwischen apps in verschiedenen App-Containern freigeben. Verwenden Sie einen Webdienst, um Daten zwischen apps freizugeben.

> [!Important]
> Wenn Sie versuchen, App-Container-Sicherheitsregeln zu umgehen, wird Ihr IME möglicherweise als Schadsoftware behandelt und blockiert.

## <a name="ime-and-touch-keyboard"></a>IME-und Touchscreen-Tastatur

Ihr IME muss sicherstellen, dass die Benutzeroberfläche des Kandidaten Bereichs und andere Elemente der Benutzeroberfläche nicht unterhalb der Fingereingabe Tastatur gezeichnet werden. Die Fingereingabe Tastatur wird in einem höheren z-Reihenfolge als alle apps angezeigt, und die IME-Benutzeroberfläche wird in derselben z-Reihenfolge angezeigt wie die APP, in der Sie aktiv ist. Folglich kann sich die Fingereingabe Tastatur überlappen und die IME-Benutzeroberfläche ausblenden. In den meisten Fällen sollte die APP die Größe des Fensters ändern, um die Touchscreen-Tastatur zu berücksichtigen. Wenn die Größe einer APP nicht geändert wird, kann der IME weiterhin die [inputpane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) -API verwenden, um die Position der Touchscreen-Tastatur zu erhalten. Der IME fragt die [Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) -Eigenschaft ab oder registriert einen Handler für das Show-und Hide-Ereignis der Touchscreen-Tastatur. Das Show-Ereignis wird jedes Mal ausgelöst, wenn der Benutzer auf ein Bearbeitungsfeld tippt, auch wenn die Fingereingabe Tastatur derzeit angezeigt wird. Der IME kann diese API verwenden, um den von der touchtastatur verwendeten Bildschirmbereich zu erhalten, bevor die IME-Benutzeroberfläche (oder eine andere) Benutzeroberfläche erstellt wird, und um die IMEs-Benutzeroberfläche zu übertragen, um das Zeichnen unterhalb der touchtastatur zu vermeiden

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>Angeben des bevorzugten Berührungs Tastaturlayouts

Der IME kann angeben, welches Touchscreen-Tastaturlayout verwendet werden soll, und der IME ist für die Arbeit mit Touchscreen-optimierten Layouts aktiviert. Diese Funktionalität ist auf IMEs für die Sprachen Koreanisch, Japanisch, Chinesisch (vereinfacht) und Chinesisch (traditionell) beschränkt.

Es werden sieben Layouts von der Touchscreen-Tastatur unterstützt, von denen drei klassische Layouts und vier für Berührungs optimierte Layouts sind. Die klassischen Layouts Aussehen und Verhalten sich wie eine physische Tastatur.

Alle drei klassischen Layouts dienen zum Einfügen von traditionellem Chinesisch in verschiedenen Formularen:

- Phonetische basierte Eingabe
- ChangJie-Eingabe
- DaYi-Eingabe

Zusätzlich zu den klassischen Layouts gibt es für jede der Sprachen für Koreanisch, Japanisch, Chinesisch (vereinfacht) und Chinesisch (traditionell) ein Fingerabdruck optimiertes Layout.

Um diese Funktion verwenden zu können, muss Ihr IME die [itffngetpreferredtouchkeyboardlayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) -Schnittstelle implementieren, die durch den IME mithilfe der [ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) -API des Text Services-Frameworks exportiert wird.

Wenn Ihr IME die itffngetpreferredtouchkeyboardlayout-Schnittstelle nicht unterstützt, führt die Verwendung von IME zum klassischen Standardlayout für die Sprache, die von der touchtastatur angezeigt wird.

Wenn Ihr IME eines der klassischen Layouts als bevorzugtes Layout festlegen muss, ist auf der IME-Seite keine zusätzliche Arbeit erforderlich, über die die Schnittstellen itffngetpreferredtouchkeyboardlayout und ITfFunctionProvider hinaus unterstützt werden. Im IME sind jedoch zusätzliche Arbeitsschritte erforderlich, um mit den Touchscreen-optimierten Layouts arbeiten zu können. Dies wird im nächsten Abschnitt beschrieben.

### <a name="touch-optimized-layout"></a>Berührungs optimiertes Layout

Die touchoptimierten Tastaturen für die Eingabe Sprachen Koreanisch, Japanisch, Chinesisch (vereinfacht) und Chinesisch (traditionell) zeigen ein anderes Layout für den Konvertierungsmodus IME on und IME aus. Es gibt einen Schlüssel auf der Fingereingabe Tastatur, um den IME-Konvertierungsmodus auf ein oder aus festzulegen. der IME-Modus der Tastatur kann sich jedoch auch als Fokus Änderungen zwischen Bearbeitungs Steuerelementen ändern.

Die Fingerabdruck optimierten Tastatur für die Eingabe Sprachen Japanisch, Chinesisch (vereinfacht) und Chinesisch (traditionell) enthalten einen Schlüssel oder Schlüssel, den der IME verwendet, um durch Kandidaten Seiten zu navigieren. Für Japanisch und Chinesisch (vereinfacht) wird der Schlüssel der Kandidaten Seite im Fingerabdruck optimierten Layout angezeigt. Für Chinesisch (traditionell) gibt es separate Schlüssel für die vorherigen und nächsten Kandidaten Seiten.

Wenn diese Tasten gedrückt werden, ruft die Touchscreen-Tastatur die [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) -Funktion auf, um die folgenden Unicode-Zeichen für den privaten Anwendungsbereich an die fokussierte Anwendung zu senden, die der IME abfangen und verarbeiten kann:

- **Nächste Seite (0xF 003)** : wird gesendet, wenn der Schlüssel der Kandidaten Seite auf der Fingereingabe optimierten Tastatur für Japanisch und vereinfachtes Chinesisch gedrückt wird oder wenn der nächste Seiten Schlüssel auf der Touchscreen-Tastatur für traditionelles Chinesisch gedrückt wird.
- **Vorherige Seite (0xF 004)** : wird gesendet, wenn entweder der Schlüssel der Kandidaten Seite zur gleichen Zeit wie die UMSCHALTTASTE auf der Touchscreen-Tastatur für Japanisch und vereinfachtes Chinesisch gedrückt wird oder wenn der vorherige Seiten Schlüssel auf der Touchscreen-optimierten Tastatur für traditionelles Chinesisch gedrückt wird.

Diese Zeichen werden als Unicode-Eingabe gesendet. Im nächsten Abschnitt wird erläutert, wie die Zeichen Informationen in den Schlüsseln der Schlüsselereignis Senke extrahiert werden, die das Framework für den Text Dienst Framework empfängt. Diese Zeichen Werte werden nicht in einer Header Datei definiert, daher müssen Sie Sie in Ihrem Code definieren.

Um Tastatureingaben abzufangen, muss sich der IME als Schlüsselereignis Senke registrieren. Bei Unicode-Eingaben, die mithilfe der SendInput-Funktion generiert werden, enthält der wParam-Parameter der [ITF keyeventsink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) -Rückrufe (OnKeyDown, OnKeyUp, ontestkeydown, ontestkeyup) immer den virtuellen Schlüssel VK_PACKET und identifiziert das Zeichen nicht direkt.

Implementieren Sie die folgende-Rückruf Sequenz, um auf das Zeichen zuzugreifen:

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>Integration der IME-Suche

Stellen Sie Benutzern Suchfunktionen über den Such Vertrag und die Integration in den Suchbereich bereit.

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="Suchbereich und IME-Vorschläge":::<br/>
*Suchbereich und IME-Vorschläge*

Der Suchbereich ist ein zentraler Ort, an dem Benutzer Suchvorgänge für alle Ihre apps durchführen können. Für IME-Benutzer bietet Windows eine einzigartige Suchfunktion, mit der kompatible IMEs in Windows integriert werden kann, um eine höhere Effizienz und Benutzerfreundlichkeit zu erzielen.

Benutzer, die mit einem IME eingeben, der mit der Suche kompatibel ist, erhalten zwei wichtige Vorteile:

- Nahtlose Interaktion zwischen dem IME und der Suchfunktion. IME-Kandidaten werden Inline im Suchfeld angezeigt, ohne Suchvorschläge zu verwerfen. Der Benutzer kann die Tastatur verwenden, um zwischen dem Suchfeld, den IME-Konvertierungs Kandidaten und den Such Vorschlägen nahtlos zu navigieren.
- Schneller Zugriff auf relevante Ergebnisse und Vorschläge, die von Anwendungen bereitgestellt werden. Die APP verfügt über Zugriff auf alle aktuellen Konvertierungs Kandidaten, um relevantere Vorschläge bereitzustellen. Um Suchvorschläge besser priorisieren zu können, werden die-Konvertierungen in der Reihenfolge ihrer Relevanz an Apps übergeben. Benutzer suchen und wählen das gewünschte Ergebnis, ohne Sie zu überschreiben, indem Sie einfach phonetisch eingeben.

Ein IME ist mit der integrierten Suchfunktion kompatibel, wenn die folgenden Kriterien erfüllt sind:

- Kompatibel mit der Windows-stilshell.
- Implementieren Sie die TSF-uiless-Modus-APIs. Weitere Informationen finden Sie unter [Übersicht über den uiless-Modus](/windows/win32/tsf/uiless-mode-overview).
- Implementieren Sie die TSF Search Integration-APIs, [itffnsearchcandidateprovider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider) und [itfintegratablecandidatelistuielement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement).

Wenn Sie im Suchbereich aktiviert ist, wird ein kompatibles IME im uiless-Modus abgelegt und kann die Benutzeroberfläche nicht anzeigen. Stattdessen sendet Sie Konvertierungs Kandidaten an Windows, das Sie im Inline Kandidatenlisten-Steuerelement anzeigt, wie im vorherigen Screenshot dargestellt.

Außerdem sendet der IME Kandidaten, die zum Ausführen der aktuellen Suche verwendet werden sollen. Diese Kandidaten können mit den Konvertierungs Kandidaten identisch sein, oder Sie können auf die Suche zugeschnitten werden.

Gute Such Kandidaten erfüllen die folgenden Kriterien:

- Kein Präfix überlappend. Beispielsweise sind 北京大学 and北京 redundant, da eins ein Präfix der anderen ist.
- Keine redundanten Kandidaten. Jeder redundante Kandidat ist für die Suche nicht nützlich, da er nicht zum Filtern von Ergebnissen beiträgt. Beispielsweise entspricht jedes Ergebnis, das 北京大学 entspricht, auch 北京.
- Kein Vorhersage Kandidat, nur Konvertierung. Wenn der Benutzer z. b. "be" eingibt, kann der IME 北 als Kandidat, aber nicht als 北京大学 zurückgeben. In der Regel sind Vorhersage Kandidaten zu restriktiv.

IMEs, die die Kriterien nicht erfüllen, sind nicht auf die gleiche Weise wie andere Steuerelemente mit der Suchanzeige kompatibel und können die Benutzeroberflächen Integration und Such Kandidaten nicht nutzen. Apps empfangen nur Abfragen, nachdem die Erstellung des Benutzers abgeschlossen ist.

Wenn eine APP, die den Such Vertrag unterstützt, eine Abfrage empfängt, enthält das Abfrage Ereignis ein "querytextalternatives"-Array, das alle bekannten Alternativen enthält, die von der relevantesten (wahrscheinlichsten) bis zum am wenigsten relevanten (unwahrscheinlich) geordnet sind.

Wenn Alternativen bereitgestellt werden, sollte die APP jede Alternative als Abfrage behandeln und alle Ergebnisse zurückgeben, die einer der alternativen entsprechen. Die APP sollte sich so Verhalten, als ob der Benutzer mehrere Abfragen gleichzeitig ausgegeben hätte, um im Grunde eine "oder"-Abfrage an den Dienst auszugeben, der die Ergebnisse liefert. Überlegungen zur Leistung beschränken apps häufig die Übereinstimmung auf zwischen 5 und 20 der relevantesten Alternativen.

## <a name="ui-design-guidelines"></a>Entwurfs Richtlinien für Benutzeroberflächen

Alle IMEs müssen den in [Entwerfen und Codieren von Windows-apps](../index.md)beschriebenen Richtlinien für die Benutzer Leistung entsprechen.

### <a name="dont-use-sticky-windows"></a>Keine kurzfenster verwenden

Ihre IME-Fenster sollten nur bei Bedarf angezeigt werden, und Sie sollten nicht ständig sichtbar sein. Wenn Benutzer nicht eingeben müssen, sollte IME Windows nicht angezeigt werden. Das IME-Fenster darf kein voll Bildfenster sein. IME-Fenster sollten einander nicht überlappen. Die Fenster sollten in einem Windows-Stil entworfen werden und die Benutzeroberflächen Skalierung befolgen.

### <a name="ime-icons"></a>IME-Symbole

Es gibt zwei Arten von IME-Symbolen, brandingsymbolen und Mode-Symbolen. Alle IME-Symbole müssen nur mit schwarz-und weißem Farben entworfen werden. Die neuen IME-Symbole werden aus dem Glyphen Aussehen der Task leisten Symbole ausgeliehen. Dieser Stil wurde erstellt, sodass er von allen Sprachen dazu verwendet werden kann, die familiäre Darstellung zu ergänzen, während gleichzeitig voneinander unterschieden wird.

Das Dateiformat für IME-Symbole ist ico. Sie müssen die folgenden Symbolgrößen angeben.

- 16x16 Pixel
- 20 x 20 Pixel
- 24 x 24 Pixel
- 32 x 32 Pixel
- 40 x 40 Pixel
- 48 x 48 Pixel

Stellen Sie sicher, dass in allen Auflösungen 32-Bit-Symbole mit Alphakanal bereitgestellt werden.

IME-Marken Symbole werden durch ein weißes Feld definiert, in dem ein typografisches Symbol, das in einer modernen Schriftart gerendert wird, platziert wird. Jedes zu definierende Symbol wird von jedem Sprachteam ausgewählt. Das Symbol ist schwarz. Das Feld enthält einen äußeren Strich von 1 Pixel in schwarz mit einer Deckkraft von 50%. "Neue" Versionen werden durch eine abgerundete Ecke in der oberen linken Ecke des Felds definiert.

Die Symbole des IME-Modus werden durch ein weißes typografisches Symbol in einer modernen Schriftart definiert, das einen äußeren Strich von 1 Pixel in schwarz mit einer Deckkraft von 50% enthält.

| Symbol | BESCHREIBUNG |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="Beispiel für das IME-Marken Symbol für traditionelles Chinesisch (changejie)."::: | Beispiel für das IME-Marken Symbol für traditionelles Chinesisch (changejie). |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="Beispiel für ein IME-Marken Symbol für traditionelles Chinesisch (New changejie)."::: | Beispiel für das IME-Marken Symbol für traditionelles Chinesisch (changejie). |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="Symbol für den chinesischen Modus"::: | Beispiel Symbol für den IME-Modus. |

### <a name="owned-window"></a>Eigenes Fenster

Um die Benutzeroberfläche des Kandidaten anzuzeigen, muss ein IME sein Fenster als Besitzer des Fensters festlegen, damit es über der derzeit ausgelaufenden App angezeigt werden kann. Verwenden Sie die [itfcontextview:: getwnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd) -Methode, um das Fenster abzurufen, das im Besitz von ist. Wenn getwnd einen Fehler oder NULL-HWND zurückgibt, müssen Sie die [GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) -Funktion aufrufen.

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>Interaktion des IME-Kandidaten Fensters mit hellen Oberflächen

Das Kündigungs Modell für Popup Fenster wird als "Light-verwerfen" bezeichnet, da es für einen Benutzer einfach ist, solche Fenster zu schließen. Damit IMEs im Windows-Interaktionsmodell ordnungsgemäß funktioniert, muss das IME-Fenster am Modell für das Licht verwerfen teilnehmen.

Um an dem Modell für das einfache verwerfen teilzunehmen, muss Ihr IME mithilfe der [NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent) -Funktion oder einer ähnlichen Funktion drei neue Windows-Ereignisse aufwecken. Diese neuen Ereignisse lauten:

- **EVENT_OBJECT_IME_SHOW** -dieses Ereignis aus, wenn das IME sichtbar wird.
- **EVENT_OBJECT_IME_HIDE** -dieses Ereignis aus, wenn das IME ausgeblendet ist.
- **EVENT_OBJECT_IME_CHANGE** : Dieses Ereignis wird beim Verschieben oder Ändern der Größe durch den IME erhöht.

### <a name="declaring-compatibility"></a>Deklarieren der Kompatibilität

IMEs deklarieren, dass Sie kompatibel sind, indem Sie die kategorieGUID_TFCAT_TIPCAP_IMMERSIVESUPPORT für Ihren IME mithilfe von [ITF categorymgr:: registercategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)registrieren.

### <a name="set-the-default-ime-mode-to-on"></a>Legen Sie den Standard-IME-Modus auf ein fest.

Wir bieten eine bessere Benutzeroberfläche für IMEs.

## <a name="dpi-scaling-support-for-desktop-applications"></a>Unterstützung der DPI-Skalierung für Desktop Anwendungen

Die verbesserte Unterstützung für die DPI-Skalierung ermöglicht das Abfragen des deklarierten dpi-sesegradegradegradegradegradegradegradevorgangs In einem Szenario mit mehreren Monitoren skaliert Windows die Benutzeroberfläche entsprechend den verschiedenen DPI-Einstellungen auf den einzelnen Monitoren.

Da Ihr IME im Kontext der einzelnen Anwendungsprozesse ausgeführt wird, sollten Sie keinen dpi-Kompatibilitäts Grad für Ihren IME deklarieren. Dadurch wird sichergestellt, dass Ihr IME auf der dpi-Ebene des aktuellen Prozesses ausgeführt wird.

Um sicherzustellen, dass alle IME-Benutzeroberflächen Elemente eine Skalierungs Parität mit den Benutzeroberflächen Elementen des Prozesses aufweisen, in dem Sie ausführen, müssen Sie entsprechend auf verschiedene dpi-Werte reagieren.

> [!NOTE]
> Um die Parität mit neuen Desktop Anwendungen sicherzustellen, sollte Ihr IME pro Monitor – dpi-Erkennung unterstützen, sollte jedoch nicht eine Ebene von Bewusstsein selbst deklarieren. Das System bestimmt die entsprechenden Skalierungs Anforderungen in jedem Szenario.

Ausführliche Informationen zu den Anforderungen für die DPI-Skalierung für Desktop Anwendungen finden Sie unter [High dpi](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows).

## <a name="ime-installation"></a>IME-Installation

Wenn Sie Ihre IME mithilfe von Microsoft Visual Studio erstellen, erstellen Sie eine Installations Benutzerfunktion für Ihren IME, indem Sie ein Drittanbieter-Installationsprogramm wie InstallShield von Flexera Software verwenden.

Die folgenden Schritte zeigen, wie Sie InstallShield verwenden, um ein Setup-Projekt für Ihre IME-DLL zu erstellen.

- Installieren von Visual Studio.
- Starten Sie Visual Studio.
- Zeigen Sie im Menü **Datei** auf **neu** , und wählen Sie **Projekt**aus. Das Dialogfeld " **Neues Projekt** " wird geöffnet.
- Navigieren Sie im linken Bereich zu **Vorlagen > anderen Projekttypen > Setup und Bereitstellung**, klicken Sie auf **InstallShield Limited Edition aktivieren**, und klicken Sie auf **OK**. Befolgen Sie die Installationsanweisungen.
- Starten Sie Visual Studio neu.
- Öffnen Sie die IME-Projektmappendatei (. sln).
- Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe, zeigen Sie auf **Hinzufügen**, und wählen Sie **Neues Projekt**. Das Dialogfeld **Neues Projekt hinzufügen** wird geöffnet.
- Navigieren Sie im linken Strukturansicht-Steuerelement zu **Vorlagen > anderen Projekttypen > InstallShield Limited Edition**.
- Klicken Sie im mittleren Fenster auf **InstallShield Limited Edition-Projekt**.
- Geben Sie im Textfeld **Name den Namen** "setupime" ein, und klicken Sie auf **OK**.
- Klicken Sie im Dialogfeld **Projekt-Assistent** auf **Anwendungsinformationen**.
- Geben Sie den Namen Ihres Unternehmens und die anderen Felder ein.
- Klicken Sie auf **Anwendungs Dateien**.
- Klicken Sie im linken Bereich mit der rechten Maustaste auf den Ordner **[INSTALLDIR]** , und wählen Sie **neuer Ordner**aus. Benennen Sie den Ordner "Plug-ins".
- Klicken Sie auf **Dateien hinzufügen**. Navigieren Sie zu ihrer IME-DLL, und fügen Sie Sie **dem Ordner Plug** -in hinzu. Wiederholen Sie diesen Schritt für das IME-Wörterbuch.
- Klicken Sie mit der rechten Maustaste auf die IME-DLL und wählen Sie **Eigenschaften** Das Dialogfeld **Eigenschaften** wird geöffnet.
- Klicken Sie im Dialogfeld **Eigenschaften** auf die Registerkarte **com & .NET-Einstellungen** .
- Wählen Sie unter **Registrierungstyp**die Option **Selbstregistrierung** aus, und klicken Sie auf **OK**.
- Erstellen Sie die Projektmappe. Die IME-DLL wird erstellt, und InstallShield erstellt eine setup.exe-Datei, die es Benutzern ermöglicht, Ihr IME unter Windows zu installieren.

Um eine eigene Installationsumgebung zu erstellen, rufen Sie die [itfinputprocessorprofilemgr:: registerprofile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) -Methode auf, um den IME während der Installation zu registrieren. Schreiben Sie Registrierungseinträge nicht direkt.

Wenn der IME unmittelbar nach der Installation verwendet werden muss, müssen Sie [installlayoutortip](/windows/win32/tsf/installlayoutortip) aufrufen, um den Benutzer fähigen Eingabemethoden den IME hinzuzufügen. verwenden Sie dabei das folgende Format für den PSZ-Parameter:

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>IME-Barrierefreiheit

Implementieren Sie die folgende Konvention, damit Ihre IMEs den Barrierefreiheits Anforderungen entsprechen und mit der Sprachausgabe arbeiten können. Damit Kandidatenlisten zugänglich gemacht werden können, müssen die IMEs dieser Konvention folgen.

- Die Kandidatenliste muss eine **UIA_AutomationIdPropertyId** gleich "IME_Candidate_Window" für Listen von Konvertierungs Kandidaten oder "IME_Prediction_Window" für Listen von Vorhersage Kandidaten haben.
- Wenn die Kandidatenliste angezeigt und ausgeblendet wird, werden Ereignisse vom Typ " **UIA_MenuOpenedEventId** " und " **UIA_MenuClosedEventId**" ausgelöst.
- Wenn sich der aktuelle ausgewählte Kandidat ändert, löst die Kandidatenliste eine **UIA_SelectionItem_ElementSelectedEventId**aus. Für das ausgewählte Element sollte eine Eigenschaft **UIA_SelectionItemIsSelectedPropertyId** gleich **true**sein.
- Die **UIA_NamePropertyId** für jedes Element in der Kandidatenliste muss der Name des Kandidaten sein. Optional können Sie zusätzliche Informationen bereitstellen, um Kandidaten über **UIA_HelpTextPropertyId**zu unterscheiden.

## <a name="related-topics"></a>Zugehörige Themen

- [Eingabemethoden-Editor (IME)](input-method-editors.md)
- [Itffngetpreferredtouchkeyboardlayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITF-menteventsink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [Itfthreadmgrex:: getactiveflags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITF contextview:: getwnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [Itffnsearchcandidateprovider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [Itfintegratablecandidatelistuielement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [Bedienungshilfen](../accessibility/accessibility.md)