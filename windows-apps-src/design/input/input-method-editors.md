---
Description: Ein Eingabemethoden-Editor (Input Method Editor, IME) ist eine Softwarekomponente, die es Benutzern ermöglicht, Text in einer Sprache einzugeben, die auf einer Standardtastatur der QWERTY nicht leicht dargestellt werden kann.
title: Eingabemethoden-Editoren (IME)
label: Input Method Editors (IME)
template: detail.hbs
keywords: IME, Eingabemethoden-Editor, Eingabe, Interaktion
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 438c53a0f3fbec1fdac0206bde3584c738759de4
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840021"
---
# <a name="input-method-editors-ime"></a>Eingabemethoden-Editoren (IME)

Ein Eingabemethoden-Editor (Input Method Editor, IME) ist eine Softwarekomponente, die es Benutzern ermöglicht, Text in einer Sprache einzugeben, die auf einer Standardtastatur der QWERTY nicht leicht dargestellt werden kann. Dies ist in der Regel auf die Anzahl der Zeichen in der Sprache des Benutzers zurückzuführen, wie z. b. die verschiedenen ostasiatischen Sprachen.

Anstatt jedes einzelne Zeichen, das in einer einzelnen Tastatur angezeigt wird, gibt ein Benutzer Tastenkombinationen ein, die vom IME interpretiert werden. Der IME generiert entweder das Zeichen, das mit dem Satz von Tastenkombinationen übereinstimmt, oder eine Liste von Kandidaten Zeichen, die ausgewählt werden sollen. Das ausgewählte Zeichen wird dann in das Bearbeitungs Steuerelement eingefügt, mit dem der Benutzer interagiert.

> [!NOTE]
> IMEs können sowohl Hardware-Tastaturen als auch Bildschirm-oder touchtastaturen unterstützen.

Ihre APP muss nicht direkt mit dem IME interagieren. Der IME ist in das System integriert, ebenso wie die Touchscreen-Tastatur. Wenn Ihre APP Texteingaben hat und Sie Texteingaben in Sprachen unterstützen möchten, die eine IME erfordern, sollten Sie die End-to-End-Kundenfreundlichkeit für den Text Eintrag testen. Auf diese Weise können Sie Probleme beheben, z. b. das Anpassen der Benutzeroberfläche, sodass Sie nicht durch das Fingereingabe Tastatur-oder IME Candidate-Fenster verdeckt werden.

## <a name="creating-an-ime"></a>Erstellen eines IME

Um eine großartige Eingabe für alle Benutzer zu ermöglichen, erzeugt Microsoft IMEs für eine Vielzahl von Sprachen, die im Lieferumfang enthalten sind.

Zusätzlich zu den in-Box-Anwendungen können Sie eigene benutzerdefinierte IMEs erstellen, die Benutzer wie eine in-Box-IME installieren und verwenden können.

Alle IMEs werden im Windows-System ausgeführt, das zum verhindern bösartiger IMEs und zum Verbessern der Sicherheit und des Benutzer Erlebnisses aller IMEs gehört.

Benutzerdefinierte IMEs können mit der Standard tastentastatur verknüpft werden und das Layout verwenden, damit Endbenutzer Ihren IME mit der Touchscreen-Tastatur verwenden können. Sie können jedoch keine eigene unabhängige Fingereingabe Tastatur bereitstellen, und bestimmte Funktionen von in-Box-IMEs für Berührungs Tastatur sind nicht für benutzerdefinierte IMEs verfügbar.

## <a name="requirements-for-imes"></a>Anforderungen für IMEs

Ein Drittanbieter-IME muss die folgenden Anforderungen erfüllen:

- Muss digital signiert werden
- Muss das [Text Dienste-Framework (TSF)](/windows/win32/tsf/text-services-framework) unterstützen, wobei geeignete IME-Flags ordnungsgemäß festgelegt sind.
- Muss den in den Anforderungen für den [Eingabemethoden-Editor (IME)](input-method-editor-requirements.md) beschriebenen Richtlinien und dem [Entwerfen und Codieren von Windows-apps](/windows/uwp/design/)

Die Ausführung eines Drittanbieter-IME, der diese Anforderungen nicht erfüllt, wird blockiert.

> [!NOTE]
> Ältere benutzerdefinierte IMEs können in Desktop-Apps ausgeführt werden, werden jedoch in Windows-apps blockiert.

Außerdem entfernt Windows Defender schädliche IMEs aus dem System. Aus diesem Grund ist es wichtig, dass Sie sich mit den IME-Codierungs Anforderungen vertraut machen. Weitere Informationen finden Sie unter [Eingabemethoden-Editor (IME)-Anforderungen](input-method-editor-requirements.md).

## <a name="design-guidelines-for-imes"></a>Entwurfs Richtlinien für IMEs

Weitere Informationen zu bewährten Methoden und Entwurfs Richtlinien für IMEs finden Sie in den [Anforderungen des Eingabemethoden-Editors (IME)](input-method-editor-requirements.md) . Im Allgemeinen benötigen alle IME-UIs Folgendes:

- Befolgen Sie die UX-Richtlinien für Windows-Runtime-apps.
- Vermeiden Sie modale Erfahrungen, und zeigen Sie bei Bedarf nur das IME-Fenster
- Symbole nur schwarz und weiß einschließen

## <a name="related-topics"></a>Verwandte Themen

- [Anforderungen an den Eingabemethoden-Editor (IME)](input-method-editor-requirements.md)
- [Itffngetpreferredtouchkeyboardlayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITF-menteventsink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [Itfthreadmgrex:: getactiveflags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITF contextview:: getwnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
