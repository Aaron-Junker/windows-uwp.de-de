---
description: Tipps zum Erstellen von konsistenten, benutzerfreundlichen Apps, die auch Originalität und Kreativität zum Ausdruck bringen
title: Stil und Konsistenz im Gleichgewicht (Windows-App-Design)
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ecfa771d8976b3c9e8cce8c8e9c45a5db5fb9b0a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220273"
---
# <a name="balancing-style-and-consistency"></a>Stil und Konsistenz im Gleichgewicht

 

> Hinweis: Dieser Artikel ist ein früher Entwurf für Windows 10 RS2. Featurenamen, Terminologie und Funktionen sind nicht endgültig.

Wenn Sie ein Produkt entwerfen, tun Sie das für die Kunden. Wir sind alle bemüht, das Design zu entwickeln, das unseren Absichten am besten entspricht. In diesem Artikel wird das Verhältnis zwischen dem Einhalten von Konventionen zum Erstellen einer konsistenten Benutzeroberfläche und dem Erstellen von einzigartigen Features und Funktionen untersucht, die Ihre App von anderen abhebt. 

 
## <a name="the-importance-of-consistency"></a>Die Bedeutung der Konsistenz
Warum ist Konsistenz wichtig? Konsistenz kann eine App benutzerfreundlicher machen. Ein wichtiger Bestandteil eines guten Designs besteht in der leichten Erlernbarkeit. Ein App-übergreifend konsistentes Design sorgt dafür, dass der Endbenutzer die Bedienung der App nicht immer wieder „neu erlernen” muss. Sehen Sie sich Beispiele aus dem wahren Leben an: Ein Gaspedal ist immer auf der rechten Seite, ein Türknauf muss zum Öffnen der Tür immer gegen den Uhrzeigersinn gedreht werden, ein Stoppschild ist immer rot. 

Eine App konsistent zu gestalten bedeutet jedoch nicht, dass alle Apps gleich aussehen und sich gleich verhalten müssen. Um noch einmal zu Beispielen aus dem wahren Leben zurückzukehren: Es gibt eine beinahe unendliche Anzahl an Stuhldesigns, aber nur bei sehr wenigen davon müssen Sie neu erlernen, wie Sie darauf sitzen können. Dies liegt daran, dass die wichtigen Elemente – die Sitzfläche – konsistent genug sind, dass der Benutzer sie versteht. 

Eine der Herausforderungen beim Erstellen eines guten App-Designs besteht darin, zu verstehen, in welchen Punkten es wichtig ist, konsistent zu sein, und in welchen Punkten es wichtig ist, einzigartig zu sein. 

## <a name="the-consistency-spectrum"></a>Das Konsistenzspektrum
 Es ist hilfreich, sich Konsistenz als ein Spektrum mit zwei entgegengesetzten Enden vorzustellen:


![Das Konsistenzspektrum](images/consistency/consistency-spectrum.png)

Zu den Elementen der vertrauen Umgebung gehören:
-   Bewährte UI-Paradigmen (das Verhalten bei einem Mausklick, das Drücken und Halten eines Elements, ähnlich aussehende Symbole)
-   Elemente Ihrer Marke, die Sie produktübergreifend anwenden möchten (Typografie, Farben)

Zu den Unterscheidungsmerkmalen gehören:
-   Elemente, die die einzigartige „Seele” des Produkts ausmachen
-   Elemente, mit denen Sie die Umgebung an den gewünschten Formfaktor anpassen können

Erstellen wir ein Designmodell anhand dieses Spektrums, und wenden wir es auf die wichtigen Elemente einer App an. 

![Das Designkonsistenz-Modell](images/consistency/design-consistency-model.png)

In diesem Modell stellen die Basisebenen eine bewährte Grundlage für die Konsistenz dar, während der Fokus bei den oberen Schichten auf Flexibilität und Anpassung liegt.  

1. Die App-Grundlagen bilden die erste Ebene unseres Modells: Layoutraster, [Farbpalette](color.md), [Typografiekonventionen](typography.md), und [Symbolstile](icons.md). Diese grundlegenden Features sollten konsistent angewendet werden. 

2. Für die zweite Ebene stellt UWP einen Satz von grundlegenden [allgemeinen Steuerelementen](../controls-and-patterns/index.md) bereit, die für ein Gleichgewicht zwischen Effizienz und Flexibilität sorgen. Wir haben darüber hinaus Richtlinien für eine konsistente Ausdrucksweise und einen konsistenten Tonfall entwickelt. Dabei werden zum Beschreiben die gleichen Worte verwendet wie dafür, Personen durch App-Funktionen zu führen. Wir haben eine Reihe von Mustern erstellt, die für App-Designs verwendet werden können. Dadurch ist sichergestellt, dass unsere Designs über verschiedene Geräteformate und Eingaben hinweg skalierbar sind. 
3. Ebene Drei ist der Bereich, in dem Sie die Navigation an verschiedene Geräte und unterschiedlichen Kontext anpassen. Beispielsweise ist die Navigation über Toucheingaben auf einem Telefon wahrscheinlich anders als die Navigation auf einem 32-Zoll-Monitor mit Tastatur und Maus oder auf einer HoloLens mit Gesten und 100 Berührungspunkten auf einer 84-Zoll-Oberfläche.
4. Ebene Vier ist der Bereich, in dem Sie Ihre Markenpersönlichkeit definieren. Welche Signaturdesignelemente stärken Ihre Marke und heben sie von der Konkurrenz ab? Dort können Sie Ihre App auch für verschiedene Endbenutzer anpassen. Ist Ihre App für Spieler, für Information-Worker oder Schüler/Lehrkräfte (1. bis 12. Klasse) bestimmt? Was sind die individuellen Anforderungen für diese verschiedenen Kunden, und können wir das Design weiter für sie optimieren? Machen Sie sie nicht für alle gleich, sondern versuchen Sie weiterhin, Mehrwert für Ihre verschiedenen Kunden zu schaffen.  


## <a name="design-principles"></a>Designprinzipien
Um dieses Modell effektiv nutzen zu können, benötigen wir eine Reihe von Designprinzipien, die uns dabei helfen, die richtigen Entscheidungen zu treffen. Dies sind unsere Arbeitsprinzipien zum Erreichen von Designkonsistenz:

**Wenn es gleich aussieht, soll es sich auch gleich verhalten**
-   Wenn Benutzer ein Textfeld, oder das Hamburger-Steuerelement, sehen, erwarten sie wahrscheinlich, dass dieses sich auf verschiedenen Geräten auf die gleiche Weise verhält. Wenn gute Gründe dafür bestehen, von einer etablierten Verhaltensweise abzuweichen, sollte auch ein Unterschied erkennbar sein, damit die Erwartungen der Benutzer entsprechend angepasst werden.

**Wenn ein Element eine große Ähnlichkeit mit einem vorhandenen Element oder einer bestehenden Konvention aufweist, sollten diese auch gleich aussehen**
-   Sie benötigen ein Symbol für „neues Dokument”. Warum ein neues entwerfen, das sich nur wenig unterscheidet, wenn Sie eines verwenden können, das die Benutzer bereits kennen?

**Benutzerfreundlichkeit hat Vorrang gegenüber Konsistenz**
-   Es ist besser, benutzerfreundlich als konsistent zu sein. In einigen Fällen müssen Sie möglicherweise neue Steuerelemente oder Verhaltensweisen entwickeln, um die Benutzerfreundlichkeit zu fördern. Das Benutzen eines Telefons mit nur einer Hand bringt besondere Herausforderungen mit sich. Dasselbe gilt für das Arbeiten mit einem 80-Zoll-Bildschirm. Ein gutes Design gibt dem Benutzer das Gefühl, ein Experte zu sein. 

**Engagement ist wichtig**
-   Es sollte nicht langweilig sein. Wenn alles flach ist und nur aus einer Farbe mit Quadraten besteht: Werden unsere Kunden dies annehmen und verwenden? Wecken Sie Begeisterung. Führen Sie neue Elemente ein, die Überraschungen beinhalten, ohne die Lernförderlichkeit einzuschränken. 

**Verhaltensweisen ändern sich**
-   Dies ist der schwierige Teil: Wenn sich die Branche ändert, entstehen neue Verhaltensweisen. Es besteht die Möglichkeit, dass aktuelle Verhaltensweisen verschwinden und unsere konsistenten Verhaltensweisen neue Standards annehmen müssen. Sehen Sie sich das Zoomen durch Zusammenführen der Finger an. Früher galt die Erwartung, dass das Vergrößern/Verkleinern von Bereichen über die UI +/- ausgeführt wird. In der modernen UI gilt jedoch die Erwartung, dass das Zoomen durch Zusammenführen der Finger ausgeführt wird. Sehen Sie sich neue Oberflächenparadigmen an, und nehmen Sie Weiterentwicklungen vor. 
