---
title: Bewährte Methoden für Xbox
description: Erfahren Sie, wie Sie Ihre universelle Windows-Plattform-Anwendung (UWP) für Xbox One optimieren, indem Sie diese bewährten Methoden für die Xbox-Entwicklung befolgen.
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8aaf8759b59c8ccbb5b09ba969675096700ce9e8
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094457"
---
# <a name="xbox-best-practices"></a>Bewährte Methoden für Xbox

Standardmäßig können UWP-Apps auf Xbox One ohne zusätzlichen Aufwand von Ihrer Seite ausgeführt werden. Wenn Sie Ihre Kunden allerdings mit Ihrer App beeindrucken möchten und mit den besten Apps auf Xbox konkurrieren möchten, sollten Sie die folgenden bewährten Methoden anwenden.
  > [!NOTE]
  > Bevor Sie beginnen, sehen Sie sich die Entwurfsrichtlinien in [Entwerfen für Xbox und Fernsehgeräte](../design/devices/designing-for-tv.md) an.   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>So erzielen Sie optimale Ergebnisse für Xbox One

### <a name="do-turn-off-mouse-mode"></a>*Empfohlen:* Deaktivieren des Mausmodus

Xbox-Benutzer schätzen Ihre Controller. Um die Controller Eingaben zu optimieren, deaktivieren Sie den [Maus Modus](how-to-disable-mouse-mode.md) und aktivieren die direktionale Navigation (auch als " [XY-Fokus Navigation und Interaktion](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)" bezeichnet). Achten Sie darauf, dass Fokus-Traps und die Benutzeroberfläche nicht zugänglich

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*Empfohlen:* Ziehen Sie ein Fokusrechteck, das für einen Abstand von 3 m geeignet ist

Die meisten Xbox-Nutzer sitzen im Wohnzimmer am Fernseher. Denken Sie also daran, dass das Standard-Fokusrechteck aus 3 m Entfernung schwer zu erkennen ist. Um sicherzustellen, dass das UI-Element mit dem Eingabefokus jederzeit deutlich für die Nutzer sichtbar ist, befolgen Sie die Richtlinien zur [Fokusanzeige](../design/input/gamepad-and-remote-interactions.md#focus-visual). In XAML erhalten Sie dieses Verhalten automatisch, wenn Ihre App auf Xbox ausgeführt wird, aber für HTML-Apps ist eine benutzerdefinierte CSS-Formatvorlage erforderlich.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*Empfohlen:* Integration in die SystemMediaTransportControls-Klasse

Xbox-Benutzer möchten Medien-Apps mit der Xbox-Medienfernbedienung Cortana (insbesondere mit den Sprachbefehlen „Wiedergabe“ und „Anhalten“) und mit Xbox SmartGlass steuern. Diese Features erhalten Sie automatisch, wenn in Ihren Apps die [SystemMediaTransportControls](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols)-Klasse verwendet wird, die automatisch in den Xbox-Mediensteuerelementen enthalten ist. Wenn in Ihrer App benutzerdefinierte Mediensteuerelemente verwendet werden, stellen Sie sicher, dass diese in die **SystemMediaTransportControls**-Klasse integriert werden, damit diese Features für Ihre Nutzer bereitstehen. Wenn Sie eine App mit Hintergrundmusik erstellen, integrieren Sie die **SystemMediaTransportControls**-Klasse, um sicherzustellen, dass die Hintergrundmusik-Steuerelemente in der Xbox-Multitasking-Registerkarte richtig funktionieren.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*Beachten:* Zeichnen bis zum Bildschirmrand

Viele Fernseher schneiden das Bild an den Bildschirmrändern ab. Daher sollte der wichtige Inhalt Ihrer App im [fernsehsicheren Bereich](../design/devices/designing-for-tv.md#tv-safe-area) angezeigt werden. UWP hält den Inhalt mit *Overscan* im fernsehsicheren Bereich, aber bei diesem Standardverhalten könnte ein sichtbarer Rahmen um die App gezeichnet werden. Sie erzielen optimale Ergebnisse, wenn Sie das Standardverhalten deaktivieren und die Anweisungen auf [So wird's gemacht - Zeichnen der Benutzeroberfläche bis zum Bildschirmrand](turn-off-overscan.md) befolgen.
> [!IMPORTANT]
  > Wenn Sie Overscan deaktivieren, müssen Sie selbst sicherstellen, dass interaktive Elemente und Text innerhalb des fernsehsicheren Bereichs bleiben. 

### <a name="consider-use-tv-safe-colors"></a>*Beachten:* Verwenden von fernsehsicheren Farben

Fernsehgeräte gehen mit hohen Farbintensitäten nicht so gut wie Computermonitore um. Vermeiden Sie in Ihren Apps Farben mit hoher Intensität, damit die Benutzer keine merkwürdigen Bandeffekte und keine verwaschenen Bilder sehen. Beachten Sie auch, dass Unterschiede zwischen Fernsehern dazu führen können, dass Farben, die auf *Ihrem* Fernseher gut aussehen, für Ihre Benutzer völlig anders aussehen können. Lesen Sie die [Farben](../design/devices/designing-for-tv.md#colors) , um zu verstehen, wie Sie Ihre APP für alle Benutzer hervorragend gestalten!

### <a name="remember-you-can-disable-scaling"></a>*Denken Sie daran:* Die Skalierung kann deaktiviert werden.

UWP-Apps werden automatisch skaliert, um sicherzustellen, dass Elemente der Benutzeroberfläche wie Steuerelemente und Schriftarten auf allen Geräten lesbar sind. XAML-Apps werden auf 200 %, skaliert, während HTML-Apps auf 150 % skaliert werden. Wenn Sie eine bessere Kontrolle über das Aussehen Ihrer App auf der Xbox möchten, deaktivieren Sie den Standardskalierungsfaktor und verwenden Sie die tatsächliche Pixelanzahl eines HDTV-Geräts (1920 x 1080). Sehen Sie sich [So wird's gemacht: Deaktivieren der Skalierung](disable-scaling.md) und [Effektive Pixel und Skalierung](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling) an, um Informationen zum Anpassen Ihrer App für die Xbox zu erhalten.

Wenn Sie einen Überblick über diese Vorgehensweisen erhalten möchten, die auf eine UWP-App angewendet werden, sehen Sie sich dieses Video an.
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

Die folgenden Gespräche auf [Channel 9](https://channel9.msdn.com/) sind eine hervorragende Informationsquelle zum Entwickeln von tollen apps auf Xbox:

- [Erstellen von großartigen UWP-Apps (Universelle Windows-Plattform) für Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
- [Passen Sie Ihre App für die Xbox One und den Fernseher an](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP-Entwicklung 1: Erstellen einer adaptiven Benutzeroberfläche](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web-Apps über den Browser hinaus: plattformübergreifende und geräteübergreifende Entwicklung](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>App-Entwicklung auf Xbox

Das **App dev on Xbox** -Ereignis ist ein idealer Ausgangspunkt für Entwickler, die noch nicht mit der Entwicklung von apps auf Xbox vertraut sind.

* [Ansehen der aufgezeichneten Sitzungen](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [Blogbeiträge lesen](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>Weitere Informationen

- [UWP auf Xbox One](index.md)
- [Entwerfen für Xbox und Fernsehgeräte](../design/devices/designing-for-tv.md)
- [Progressive Web-Apps für Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)
