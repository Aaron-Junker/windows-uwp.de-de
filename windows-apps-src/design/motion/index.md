---
Description: Gut gestaltete Bewegungen lassen Apps lebhaft und realistisch erscheinen. Helfen Sie Benutzern dabei, Kontextänderungen zu verstehen, und verbinden Sie Interaktionen mit visuellen Übergängen.
title: Bewegung und Animation in UWP-Apps
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31cf2134fb8f77809b75a5abf3e6980443452059
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867404"
---
# <a name="motion-for-uwp-apps"></a>Bewegung für UWP-Apps

![Bewegungssymbol](../images/motion-2x.png)

In einer App sind fließende Bewegungen wichtig. Sie geben intelligentes Feedback basierend auf dem Verhalten des Benutzers, lassen die Benutzeroberfläche lebendig wirken und unterstützen den Benutzer bei der Navigation in der App. Durch fließende Bewegungen entsteht eine gewisse emotionale Verbindung zwischen einem Benutzer und der digitalen Benutzeroberfläche. Wir gehen von grundlegenden natürlichen Bewegungen aus, die der Benutzer bereits aus der physischen Welt kennt, und erweitern unser System darauf aufbauend.

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/Motion">die App zu öffnen und Bewegung in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-motion-principles"></a>Prinzipien fließender Bewegungen

### <a name="physical"></a>Physikalisch

Bewegte Objekte verhalten sich wie Objekte in der realen Welt. Fließende, dynamische Bewegungen ermöglichen eine natürliche Verwendung der Benutzeroberfläche und verleihen ihr eine persönliche Note, durch die eine gewisse emotionale Verbindung entsteht.

![UI-Beispiel für physische Bewegung](images/Physical.gif)
> Wenn Sie per Toucheingabe mit der UI interagieren, entspricht die Bewegung auf der Benutzeroberfläche der Geschwindigkeit der Interaktion. Die Toucheingabe ist eine direkte Eingabe. Daher beeinflusst das Objekt, mit dem Sie interagieren, die umgebenden Objekte.

### <a name="functional"></a>Funktionell

Bewegung dient einem Zweck und muss überzeugend sein. Sie unterstützt den Benutzer in komplexen Umgebungen und hilft beim Festlegen der Hierarchie. Bewegung vermittelt den Eindruck einer besseren Leistung und optimiert das Benutzererlebnis, da keine Latenz wahrgenommen wird.

![UI-Beispiel für funktionelle Bewegung](images/functional.gif)
> Seitenübergänge sind zweckorientiert gestaltet. Sie geben Hinweise darauf, wie Seiten zusammenhängen. Sie erfolgen auf eine Weise, die selbst dann als schnell empfunden wird, wenn die Leistung nicht optimal ist.

### <a name="continuous"></a>Kontinuierlich

Durch fließende Bewegungen wird die Aufmerksamkeit des Benutzers geschickt auf bestimmte Punkte gelenkt. Dadurch werden die einzelnen Schritte einer Aufgabe elegant miteinander verknüpft, um den Benutzer bei der Ausführung zu unterstützen.

![UI-Beispiel für kontinuierliche Bewegung](images/continuous3.gif)
> Objekte können sich von Szene zu Szene bewegen oder in einer Szene die Gestalt ändern, um Kontinuität zu schaffen und den Kontext für Benutzer aufrechtzuerhalten.

### <a name="contextual"></a>Kontextbezogen

Dank intelligenter Bewegung erhält der Benutzer eine Rückmeldung, die sich an seiner Interaktion mit der Benutzeroberfläche orientiert. Die Interaktion ist auf den Benutzer ausgerichtet. Die Bewegung muss dem Formfaktor angemessen und dem Szenario entsprechend gestaltet sein. Sie sollte jedem Benutzer vertraut sein.

![UI-Beispiel für kontextuelle Bewegung](images/Contextual.gif)
> Eine Animation sollte mit der Benutzerinteraktion verknüpft sein. Ein Kontextmenü wird dort bereitgestellt, wo es vom Benutzer aktiviert wurde.

## <a name="motion-articles"></a>Artikel zu Bewegungen

:::row:::
    :::column:::
### <a name="timing-and-easingtiming-and-easingmd"></a>[Timing und Beschleunigung](timing-and-easing.md)
Timing und Beschleunigung sind wichtige Elemente, um die Bewegung von Objekten natürlich erscheinen zu lassen, die in die Benutzeroberfläche hineinkommen, sie verlassen oder sich darin bewegen.
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravitydirectionality-and-gravitymd"></a>[Richtung und Schwerkraft](directionality-and-gravity.md)
Richtungssignale festigen das mentale Modell der Erlebnisse, die ein Benutzer auf einer Oberfläche macht. Eine gerichtete Bewegung unterliegt Kräften – z.B. der Schwerkraft –, die die Natürlichkeit der Bewegung verstärken.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitionspage-transitionsmd"></a>[Seitenübergänge](page-transitions.md)
Seitenübergänge dienen der Navigation, wenn Benutzer zwischen Seiten in einer App wechseln, und liefern Feedback über die Beziehung zwischen Seiten. Mithilfe von Seitenübergängen wissen Benutzer immer, an welcher Stelle der Navigationshierarchie sie sich gerade befinden.
    :::column-end:::
    :::column:::
### <a name="connected-animationconnected-animationmd"></a>[Verbundene Animationen](connected-animation.md)
Mit verbundenen Animationen können Sie dynamische und ansprechende Navigationsfunktionen erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren.
    :::column-end:::
:::row-end:::
