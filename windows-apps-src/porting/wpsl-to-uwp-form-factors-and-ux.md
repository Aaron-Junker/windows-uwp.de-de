---
description: Windows-Apps weisen auf PCs, Mobilgeräten und vielen anderen Arten von Geräten ein einheitliches Erscheinungsbild auf. Die Benutzeroberfläche, Eingabe und Interaktionsmuster sind fast identisch, und Benutzer profitieren von einer einheitlichen Umgebung auf allen Geräten.
title: Portieren von Windows Phone Silverlight zu UWP für Formfaktor und Benutzerfreundlichkeit
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e4471d7cfb682906b4f340fdbdc294723116855f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164354"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>Portieren von Windows Phone Silverlight zu UWP für Formfaktor und Benutzerfreundlichkeit


Das vorherige Thema war [Portieren der Geschäfts- und Datenebene](wpsl-to-uwp-business-and-data.md).

Windows-Apps weisen auf PCs, Mobilgeräten und vielen anderen Arten von Geräten ein einheitliches Erscheinungsbild auf. Die Benutzeroberfläche, Eingabe und Interaktionsmuster sind fast identisch, und Benutzer profitieren von einer einheitlichen Umgebung auf allen Geräten. Unterschiede zwischen Geräten, z. B. physische Größe, Standardausrichtung und effektive Pixelauflösung, wirken sich auf die Darstellung einer UWP (Universelle Windows-Plattform)-App unter Windows 10 aus. Die gute Nachricht ist, dass viele schwierige Aufgaben für Sie vom System übernommen werden, indem intelligente Konzepte wie effektive Pixel verwendet werden.

## <a name="different-form-factors-and-user-experience"></a>Verschiedene Formfaktoren und Benutzerfreundlichkeit

Unterschiedliche Geräte weisen viele verschiedene Hoch- und Querformatauflösungen sowie unterschiedliche Seitenverhältnisse auf. Wie werden die visuellen Aspekte wie Oberfläche, Text und Ressourcen von Ihrer UWP-App skaliert? Wie unterstützen Sie Toucheingabe sowie Maus- und Tastatureingaben? Wie kann ein Steuerelement bei einer App, die die Toucheingabe auf verschieden großen Geräten mit unterschiedlichen Bildschirmabständen unterstützt, ein korrekt angepasstes Touchelement in verschiedenen Pixeldichten sein *und* aus unterschiedlichen Entfernungen lesbare Inhalte bieten? In den folgenden Abschnitten erhalten Sie sämtliche wichtige Informationen.

## <a name="what-is-the-size-of-a-screen-really"></a>Wie groß ist ein Bildschirm wirklich?

Kurz gesagt, ist das subjektiv. Das hängt nicht nur von der objektiven Anzeigegröße, sondern auch von der Entfernung dazu ab. Für diese Subjektivität müssen Entwickler guter Apps stets den Blickwinkel des Benutzers einnehmen.

Objektiv gesehen wird ein Bildschirm in der Einheit Zoll und physischen Pixeln („raw pixel“) gemessen. Mit diesen beiden Werten können Sie ermitteln, wie viele Pixel in ein Zoll passen. Dies ist die Pixeldichte, auch bekannt als DPI-Wert (dots per inch, Punkte pro Zoll), auch bekannt als PPI (pixels per inch, Pixel pro Zoll). Und der Kehrwert des DPI-Werts ist die physische Größe der Pixel als Bruchteil eines Zolls. Pixeldichte ist auch als *Auflösung* bekannt, obwohl mit diesem Begriff häufig die Pixelanzahl gemeint ist.

Bei zunehmendem Abstand *erscheinen* all diese objektiven Metriken kleiner und werden in der *effektiven Größe* und der entsprechenden *effektiven Auflösung* des Bildschirms angezeigt. Mit dem geringsten Abstand zu Ihrem Auge betrachten Sie in der Regel Ihr Smartphone, gefolgt von Ihrem Tablet und dem PC-Bildschirm. Bei [Surface Hub](https://www.microsoft.com/surface/devices/surface-hub)-Geräten und Fernsehern ist der Abstand am größten. Um dies auszugleichen, werden Geräte mit zunehmendem Abstand vom Bildschirm objektiv größer. Wenn Sie Größe für Ihre UI-Elemente festlegen, verwenden Sie dabei die Einheit der so genannten „effektiven Pixel“ (Epx). Unter Windows 10 werden außerdem der DPI-Wert und der normale Betrachtungsabstand von einem Gerät berücksichtigt, um die beste Größe für Ihre UI-Elemente in physischen Pixeln und somit die bestmögliche Anzeige zu erzielen. Weitere Informationen finden Sie unter [Anzeigepixel/Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren](wpsl-to-uwp-porting-xaml-and-ui.md).

Trotzdem ist es ratsam, Ihre App mit vielen verschiedenen Geräten zu testen, damit Sie die Benutzerfreundlichkeit jeweils selbst überprüfen können.

## <a name="touch-resolution-and-viewing-resolution"></a>Touchauflösung und Anzeigeauflösung

Touchelemente (UI-Widgets) müssen die richtige Größe aufweisen. Daher muss für Touchelemente auf verschiedenen Geräten mit verschiedenen Pixeldichten die physische Größe mehr oder weniger beibehalten werden. Auch hierfür sind effektive Pixel hilfreich: Sie werden auf unterschiedlichen Geräten – unter Berücksichtigung der Pixeldichte – skaliert, um eine nahezu konstante physische Größe zu erreichen, die sich ideal für Touchelemente eignet.

Text muss die richtige Größe zum Lesen haben (Text mit 12 Punkten ist bei einem Abstand von 50 cm eine gute Faustregel), und Bilder müssen die richtige Größe und effektive Auflösung für den Abstand aufweisen. Auf unterschiedlichen Geräten sorgt diese einheitliche Skalierung der effektiven Pixel dafür, dass UI-Elemente die richtige Größe aufweisen und lesbar sind. Text und andere vektorbasierten Grafiken werden automatisch skaliert, und das sehr gut. Raster (Bitmap)-basierte Grafiken werden auch automatisch skaliert, wenn der Entwickler eine Ressource in einer einzelnen, großen Größe bereitstellt. Vorzugsweise stellt der Entwickler jedoch alle Ressourcen in einer Vielzahl von Größen bereit, damit die passende Option für den Skalierungsfaktor des Zielgeräts automatisch geladen werden kann. Weitere Informationen hierzu finden Sie unter [Anzeigepixel/Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="layout-and-adaptive-visual-state-manager"></a>Layout und adaptiver Visual State-Manager

Wir haben die Faktoren beschrieben, die für das Verständnis der Bildschirmgröße wichtig sind. Als Nächstes sehen wir uns das Layout Ihrer App an und überlegen, wie wir bei Verfügbarkeit zusätzliche Bildschirmfläche nutzen können. Betrachten Sie diese Seite aus einer einfachen App, die auf ein schmales Mobilgerät ausgelegt ist. Wie sollte diese Seite auf einem größeren Bildschirm aussehen?

![Die portierte Windows Phone Store-App](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

Die Mobilgeräteversion ist auf die Ausrichtung im Hochformat beschränkt, da dieses das beste Seitenverhältnis für die Bücherliste bietet. Für eine Seite mit Text, der auf Mobilgeräten am besten in einer einzelnen Spalte gehalten wird, würden wir ebenso vorgehen. Aber PC- und Tabletbildschirme sind in jeder Ausrichtung groß, und die Einschränkung der mobilen Geräte erscheint für größere Geräte unnötig.

Durch das optische Zoomen der App wirkt diese wie die Mobilgeräteversion, nur größer. Das Gerät und sein zusätzlicher Platz werden nicht voll ausgeschöpft, was für den Benutzer nicht von Vorteil ist. Wir sollten mehr Inhalt anzeigen, anstatt nur den gleichen Inhalt größer anzuzeigen. Sogar auf einem Phablet könnten weitere Zeilen mit Inhalt angezeigt werden. Wir könnten den zusätzlichen Platz nutzen, um andere Inhalte wie Werbung anzuzeigen, oder wir könnten das Listenfeld in eine Listenansicht ändern und, wo möglich, Elemente in mehrere Spalten aufteilen. Siehe [Richtlinien für Listen- und Rasteransichts-Steuerelemente](../design/controls-and-patterns/lists.md).

Zusätzlich zu den neuen Steuerelementen, z. B. Listen- und Rasteransicht, verfügen die meisten etablierten Layouttypen aus Windows Phone Silverlight über Entsprechungen in der universellen Windows-Plattform (UWP). Beispiele: [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) und [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel). Das Portieren des Großteils der Benutzeroberfläche, die diese Typen verwendet, sollte unkompliziert ablaufen. Suchen Sie jedoch immer nach Möglichkeiten, die dynamischen Layoutfunktionen dieser Layoutpanel zu nutzen und diese auf Geräten mit verschiedenen Größen automatisch anzupassen und neu zu gestalten.

Über das dynamische Layout hinaus, das in Systemsteuerelemente und Layoutpanel integriert ist, können wir ein neues Windows 10-Feature mit dem Namen [Adaptiver Visual State-Manager](wpsl-to-uwp-porting-xaml-and-ui.md) verwenden.

## <a name="input-modalities"></a>Eingabemodalitäten

Eine Windows Phone Silverlight-Oberfläche ist für die Toucheingabe konzipiert. Die Benutzeroberflächen Ihrer portierten Apps sollten natürlich auch die Toucheingabe unterstützen, Sie können jedoch auch andere Eingabemodalitäten wie Maus und Tastatur zulassen. In der UWP sind Maus-, Stift- und Toucheingabe als *Zeigereingaben* zusammengefasst. Weitere Informationen finden Sie unter [Behandeln von Zeigereingaben](../design/input/handle-pointer-input.md) und [Tastaturinteraktionen](../design/input/keyboard-interactions.md).

## <a name="maximizing-markup-and-code-re-use"></a>Maximieren der Wiederverwendung von Markup und Code

In der Liste [Maximieren der Wiederverwendung von Markup und Code](wpsl-to-uwp-porting-to-a-uwp-project.md) finden Sie Techniken für die Freigabe Ihrer Benutzeroberfläche für Zielgeräte mit unterschiedlichen Formfaktoren.

## <a name="more-info-and-design-guidelines"></a>Weitere Informationen und Designrichtlinien

-   [Entwerfen von UWP-Apps](https://developer.microsoft.com/windows/apps/design)
-   [Richtlinien für Schriftarten](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)
-   [Planen für verschiedene Formfaktoren](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)

## <a name="related-topics"></a>Zugehörige Themen

* [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md)