---
description: Stellt einen Wert für ein beliebiges XAML-Attribut bereit, indem ein Verweis auf eine Ressource aus einer benutzerdefinierten Ressourcennachschlage-Implementierung untersucht wird. Das Nachschlagen der Ressource erfolgt mithilfe einer Implementierung der CustomXamlResourceLoader-Klasse.
title: CustomResource-Markuperweiterung
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d59a8662d430c527aa2f83aea945ee5bcf48642e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366435"
---
# <a name="customresource-markup-extension"></a>{CustomResource}-Markuperweiterung


Stellt einen Wert für ein beliebiges XAML-Attribut bereit, indem ein Verweis auf eine Ressource aus einer benutzerdefinierten Ressourcennachschlage-Implementierung untersucht wird. Das Nachschlagen der Ressource erfolgt mithilfe einer Implementierung der [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)-Klasse.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| key | Der Schlüssel für die angeforderte Ressource. Die ursprüngliche Zuweisung des Schlüssels ist abhängig von der Implementierung der [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)-Klasse, die aktuell zur Verwendung registriert wurde. |

## <a name="remarks"></a>Hinweise

**CustomResource** ist eine Methode zum Abrufen von Werten, die an anderer Stelle in einem benutzerdefinierten Ressourcenrepository definiert sind. Diese Technik ist relativ komplex und wird in den meisten Szenarien für Windows-Runtime-App-Szenarien nicht verwendet.

Die Auflösung einer **CustomResource** in ein Ressourcenwörterbuch wird in diesem Thema nicht beschrieben, da diese sehr unterschiedlich sein kann, abhängig von der Implementierung von [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

Die [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)-Methode der Implementierung von [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) wird vom XAML-Parser der Windows-Runtime aufgerufen, wenn eine `{CustomResource}` im Markup gefunden wird. Die *resourceId*, die an **GetResource** übergeben wird, stammt aus dem *key*-Argument. Die anderen Eingabeparameter sind kontextabhängig und richten sich beispielsweise nach der Eigenschaft, auf die sich die Verwendung bezieht.

Eine `{CustomResource}`-Syntax funktioniert standardmäßig nicht (die Basisimplementierung von [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) ist unvollständig). Für einen gültigen `{CustomResource}`-Verweis müssen Sie die folgenden Schritte ausführen:

1.  Leiten Sie eine benutzerdefinierte Klasse von [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) ab, und überschreiben Sie die [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)-Methode. Rufen Sie in der Implementierung nicht die Methode der Basisklasse auf.
2.  Legen Sie [**CustomXamlResourceLoader.Current**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) fest, um auf Ihre Klasse in der Initialisierungslogik zu verweisen. Dies muss erfolgen, bevor XAML-Code auf Seitenebene geladen wird, der die `{CustomResource}`-Erweiterungssyntax enthält. Eine Stelle, an der **CustomXamlResourceLoader.Current** festgelegt werden kann, ist im [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application)-Unterklassenkonstruktor, der in den App.xaml-CodeBehind-Vorlagen für Sie generiert wird.
3.  Jetzt können Sie `{CustomResource}`-Erweiterungen in dem XAML-Code, den Ihre App als Seiten lädt, oder in XAML-Ressourcenverzeichnissen verwenden.

**CustomResource** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn für Attributwerte Escapezeichen verwendet werden müssen, damit sie keine Literalwerte oder Handlernamen darstellen, und es nicht ausreicht, Typkonverter für bestimmte Typen oder Eigenschaften zu verwenden. Alle Markuperweiterungen in XAML verwenden die "\{"und"\}" Zeichen in der Attributsyntax, dies ist die Konvention mit dem ein XAML-Prozessor erkannt wird, dass das Attribut von eine Markuperweiterung verarbeitet werden muss.

## <a name="related-topics"></a>Verwandte Themen

* [ResourceDictionary und XAML Ressourcenverweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
* [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)

