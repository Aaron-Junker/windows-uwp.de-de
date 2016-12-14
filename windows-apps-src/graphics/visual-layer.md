---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Visuelle Ebene
description: "Die Windows.UI.Composition-API ermöglicht den Zugriff auf die Kompositionsebene zwischen der Frameworkebene (XAML) und der Grafikebene (DirectX)."
translationtype: Human Translation
ms.sourcegitcommit: 9ea05f7ba76c7813b200a4c8cd021613f980355d
ms.openlocfilehash: de6fe0688bec196fc90433ab9274f2e4c4fd9b90

---
# <a name="visual-layer"></a>Visuelle Ebene

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In Windows 10 wurde viel Arbeit in die Entwicklung eines neuen einheitlichen Kompositors und eines Renderingmoduls für alle Windows-Anwendungen (Desktopanwendungen und mobile Anwendungen) investiert. Daraus resultierte die einheitliche WinRT-Composition-API „Windows.UI.Composition“, die Zugriff auf neue einfache Composition-Objekte und neue kompositorgesteuerte Animationen und Effekte bietet.

„Windows.UI.Composition“ ist eine deklarative [Retained Mode](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx)-API, die aus jeder App der universellen Windows-Plattform (UWP) aufgerufen werden kann, um Kompositionsobjekte, Animationen und Effekte direkt in der Anwendung zu erstellen. Bei der API handelt es sich um eine leistungsstarke Ergänzung zu vorhandenen Frameworks wie beispielsweise XAML, die Entwicklern von UWP-Anwendungen eine vertraute C#-Oberfläche zur Erweiterung ihrer Anwendung bietet. Diese APIs können auch dazu verwendet werden, Anwendungen im DX-Stil ohne Frameworks zu erstellen.

Ein XAML-Entwickler kann sich auf die Kompositionsebene in C# begeben, um mit WinRT benutzerdefinierte Änderungen auf der Kompositionsebene vorzunehmen und damit zu vermeiden, auf der Grafikebene mithilfe von DirectX und C++ benutzerdefinierte Änderungen an der Benutzeroberfläche vornehmen zu müssen. Diese Technik kann verwendet werden, um ein vorhandenes Element mittels Composition-APIs zu animieren oder eine Benutzeroberfläche durch die Erstellung einer „visuellen Insel“ von Windows.UI.Composition-Inhalten innerhalb der XAML-Elementstruktur zu ergänzen.

![Anordnung des UI-Framework: Die Frameworkebene (Windows.UI.XAML) basiert auf der visuellen Ebene (Windows.UI.Composition), die wiederum auf der Grafikebene (DirectX) basiert.](images/layers-win-ui-composition.png)
## <a name="span-idcompositionobjectsandthecompositorspanspan-idcompositionobjectsandthecompositorspanspan-idcompositionobjectsandthecompositorspancomposition-objects-and-the-compositor"></a><span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Composition-Objekte und der Ersteller

Composition-Objekte werden mit dem [**Ersteller**](https://msdn.microsoft.com/library/windows/apps/Dn706789) erstellt, der als Factory für Composition-Objekte dient. Der Kompositor kann [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekte erstellen, die wiederum die Erstellung einer visuellen Struktur ermöglichen, die die Grundlage für alle anderen Features und Composition-Objekte in der API bildet und von diesen verwendet wird.

Die API ermöglicht es Entwicklern, einzelne oder viele [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.

Visuelle Elemente können Container für andere visuelle Elemente sein oder Inhalte visueller Elemente hosten. Die API sorgt für mehr Benutzerfreundlichkeit, indem sie eine eindeutige Gruppe von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekten für bestimmte Aufgaben bereitstellt, die in einer Hierarchie vorhanden sind:

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – das Basisobjekt. Die meisten Eigenschaften befinden sich hier und werden von den anderen visuellen Objekten geerbt.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – wird von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) abgeleitet und bietet die Möglichkeit, untergeordnete visuelle Elemente einzusetzen.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – wird von [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) abgeleitet und enthält Inhalte in Form von Bildern, Effekten und Swapchains.
-   [**LayerVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.layervisual.aspx) – ein ContainerVisual-Element, dessen untergeordnete Elemente in einer einzelnen Ebene vereinfacht werden.  
-   [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – die Objektfactory, die die Beziehung zwischen einer Anwendung und dem Kompositorprozess des Systems verwaltet.

Der Kompositor ist auch eine Factory für zahlreiche andere Kompositionsobjekte, die dazu verwendet werden, um visuelle Elemente in der Struktur sowie eine Reihe von Animationen und Effekten zu beschneiden und umzuwandeln.

## <a name="span-ideffectssystemspanspan-ideffectssystemspanspan-ideffectssystemspaneffects-system"></a><span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Effektsystem

„Windows.UI.Composition“ unterstützt Echtzeiteffekte, die animiert, angepasst und verkettet werden können. Zu den Effekten zählen 2D-affine Transformationen, arithmetische Kompositionen, Mischungen, Farbquelle, Zusammensetzung, Kontrast, Belichtung, Graustufen, Gammakorrektur, Farbtondrehung, Invertierung, Sättigung, Sepia, Temperatur und Farbton.

Weitere Informationen finden Sie in der Übersicht [Kompositionseffekte](composition-effects.md).

## <a name="span-idanimationsystemspanspan-idanimationsystemspanspan-idanimationsystemspananimation-system"></a><span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Animationssystem

„Windows.UI.Composition“ enthält ein ausdrucksstarkes, Framework-agnostisches Animationssystem, das Ihnen die Einrichtung von zwei Arten von Animationen ermöglicht: Keyframeanimationen und Ausdrucksanimationen. Diese werden dazu verwendet, visuelle Objekte zu verschieben, umzuwandeln oder zuzuschneiden bzw. einen Effekt zu animieren. Durch die direkte Ausführung im Kompositorprozess wird Gleichmäßigkeit und Skalierung erreicht, sodass Sie eine ganze Reihe eindeutiger Animationen gleichzeitig ausführen können.

Weitere Informationen finden Sie in der Übersicht [Kompositionsanimationen](composition-animation.md).

## <a name="span-idxamlinteroperationspanspan-idxamlinteroperationspanspan-idxamlinteroperationspanxaml-interoperation"></a><span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>XAML-Interoperabilität

Die Composition-API kann eine visuelle Struktur von Grund auf neu erstellen und ist zusätzlich für die Interoperabilität mit einer bereits vorhandenen XAML-UI entworfen, wobei die [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976)-Klasse in [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) verwendet wird.

- [**ElementCompositionPreview.GetElementVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual): ruft das zugrundeliegende visuelle Objekt eines Elements auf, um es mittels Composition-APIs zu animieren.
- [**ElementCompositionPreview.SetChildVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual): fügt eine „visuelle Insel“ von Composition-Inhalten zu einer XAML-Struktur hinzu.
- [**ElementCompositionPreview.GetScrollViewerManipulationPropertySet()**](https://msdn.microsoft.com/library/windows/apps/mt608980.aspx): verwendet die Manipulation eines [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) als Eingabe für eine Compositionsanimation.


**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen hierzu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <a name="span-idadditionalresourcesspanspan-idadditionalresourcesspanspan-idadditionalresourcesspanadditional-resources"></a><span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Weitere Ressourcen:

-   Lesen Sie den MSDN-Artikel von Kenny Kerr zu dieser API: [Grafiken und Animationen – Windows Composition wird 10](https://msdn.microsoft.com/magazine/mt590968)
-   Erweiterte Beispiele für Benutzeroberfläche und Komposition finden Sie im [WindowsUIDevLabs-GitHub](https://github.com/microsoft/windowsuidevlabs).
-   [**Vollständige Referenzdokumentation für die API**](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   [Bekannte Probleme](http://go.microsoft.com/fwlink/?LinkId=823237).

 

 







<!--HONumber=Dec16_HO1-->


