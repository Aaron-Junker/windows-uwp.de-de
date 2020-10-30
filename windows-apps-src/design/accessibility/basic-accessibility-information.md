---
description: Grundlegende Informationen zur Barrierefreiheit werden häufig in die Kategorien Name, Rolle und Wert unterteilt. In diesem Thema wird der Code beschrieben, mit dem Ihre App die grundlegenden Informationen verfügbar machen kann, die für Hilfstechnologien erforderlich sind.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit
label: Expose basic accessibility information
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 09dfb92f53105d7c8718ff12f1a0d5634ba6a75d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032563"
---
# <a name="expose-basic-accessibility-information"></a>Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit  



Grundlegende Informationen zur Barrierefreiheit werden häufig in die Kategorien Name, Rolle und Wert unterteilt. In diesem Thema wird der Code beschrieben, mit dem Ihre App die grundlegenden Informationen verfügbar machen kann, die für Hilfstechnologien erforderlich sind.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>Name zur Verwendung durch Bildschirmleseprogramme  
Der Name zur Verwendung durch Screenreader-Software ist eine kurze, beschreibende Textzeichenfolge, mit der die Sprachausgabe ein UI-Element ansagt. Legen Sie den Namen zur Verwendung durch Screenreader-Software für UI-Elemente fest, deren Bedeutung wichtig für das Verständnis des Inhalts oder die Interaktion mit der Benutzeroberfläche ist. Zu diesen Elementen gehören normalerweise Bilder, Eingabefelder, Schaltflächen, Steuerelemente und Bereiche.

In der folgenden Tabelle wird beschrieben, wie Sie für verschiedene Elementtypen in einer XAML-Benutzeroberfläche einen Namen zur Verwendung durch Screenreader-Software definieren oder abrufen.

| Elementtyp | BESCHREIBUNG |
|--------------|-------------|
| Statischer Text | Für [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) und [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)-Elemente wird automatisch anhand des sichtbaren (inneren) Textes ein Name zur Verwendung durch Screenreader-Software bestimmt. Der gesamte Text im Element wird als Name verwendet. Siehe [Name aus innerem Text](#name_from_inner_text). |
| Bilder | Das XAML-Element [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) verfügt nicht über ein direktes Gegenstück zum **alt** -HTML-Attribut von **img** und ähnlichen Elementen. Verwenden Sie zum Bereitstellen eines Namens die [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)-Eigenschaft oder das Untertitelverfahren. Siehe [Namen zur Verwendung durch Bildschirmleseprogramme für Bilder](#images). |
| Formularelemente | Bei einem Formularelement sollte der Name zur Verwendung durch Screenreader-Software mit der Bezeichnung identisch sein, die für das Element angezeigt wird. Siehe [Bezeichnungen und LabeledBy](#labels). |
| Schaltflächen und Links | Standardmäßig basiert der Name zur Verwendung durch Screenreader-Software einer Schaltfläche oder eines Links auf dem sichtbaren Text, wobei ebenfalls die unter [Name aus innerem Text](#name_from_inner_text) beschriebenen Regeln angewendet werden. Verwenden Sie in Fällen, in denen eine Schaltfläche nur ein Bild enthält, die [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)-Eigenschaft, um eine reine Textentsprechung für die Schaltflächenaktion bereitzustellen. |

Die meisten Containerelemente (z. B. Bereiche) stellen ihren Inhalt nicht als Namen zur Verwendung durch die Sprachausgabe bereit. Dies ist darauf zurückzuführen, dass der Name und die entsprechende Rolle nicht vom Container, sondern vom Elementinhalt gemeldet werden sollen. Das Containerelement kann in einer Darstellung der Microsoft-Benutzeroberflächenautomatisierung melden, dass es ein Element mit untergeordneten Elementen ist, damit es von der Logik der Hilfstechnologie durchlaufen werden kann. Da Benutzer von Hilfstechnologien in der Regel nichts über die Container selbst wissen müssen, werden die meisten Container nicht benannt.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>Rolle und Wert  
Die Steuerelemente und anderen UI-Elemente des XAML-Vokabulars implementieren im Rahmen ihrer Definitionen Benutzeroberflächenautomatisierungsunterstützung für die Meldung von Rolle und Wert. Sie können sich die Rollen- und Wertinformationen für die Steuerelemente mithilfe von Tools zur Automatisierung von Benutzeroberflächen ansehen oder die Dokumentation für die [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)-Implementierungen jedes Steuerelements lesen. Die verfügbaren Rollen in einem Benutzeroberflächenautomatisierungs-Framework sind in der [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)-Enumeration definiert. Benutzeroberflächenautomatisierungs-Clients, z. b. Hilfstechnologien, können Rollen Informationen abrufen, indem Sie Methoden aufrufen, die das Benutzeroberflächenautomatisierungs-Framework mithilfe des **automationpeers** des

Nicht alle Steuerelemente haben einen Wert. Steuerelemente mit einem Wert stellen diese Informationen über die von ihnen unterstützten Peers und Muster für die Benutzeroberflächenautomatisierung bereit. Ein [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Formularelement hat z. B. einen Wert. Eine Hilfstechnologie kann ein Client für die Benutzeroberflächenautomatisierung sein und somit erkennen, ob ein Wert vorhanden ist und wie der Wert lautet. In diesem speziellen Fall unterstützt die **TextBox** -Klasse das [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider)-Muster über die [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer)-Definitionen.

> [!NOTE]
> Wenn Sie [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) oder andere Techniken verwenden, um den Namen zur Verwendung durch Screenreader-Software explizit anzugeben, darf der in den Informationen zur Steuerelementrolle bzw. zum Steuerelementtyp verwendete Text nicht in diesem Namen enthalten sein. Verwenden Sie z. B. keine Zeichenfolgen wie „Schaltfläche“ oder „Liste“ im Namen. Die Rollen- und Typinformationen stammen aus einer anderen Oberflächenautomatisierungseigenschaft ( **LocalizedControlType** ), die von der standardmäßigen Steuerelementunterstützung für die UI-Automatisierung bereitgestellt wird. Die meisten Hilfstechnologien fügen den **LocalizedControlType** an den Namen zur Verwendung durch Screenreader-Software an. Wird die Rolle auch im Namen zur Verwendung durch Screenreader-Software – also doppelt – angegeben, kann dies zu einer unnötigen Wiederholung von Wörtern führen. Wenn Sie einem [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)-Steuerelement beispielsweise den Namen „Schaltfläche“ zur Verwendung durch Screenreader-Software geben oder „Schaltfläche“ am Ende des Namens einfügen, wird dies von der Sprachausgabe unter Umständen als „Schaltfläche Schaltfläche“ gelesen. Dieser Aspekt Ihrer Informationen zur Barrierefreiheit sollte mithilfe der Sprachausgabe getestet werden.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>Beeinflussen der UI-Automatisierungsstrukturansichten  
Das Benutzeroberflächenautomatisierungs-Framework verfügt über ein Konzept mit Strukturansichten, bei dem Benutzeroberflächenautomatisierungsclients die Beziehungen zwischen Elementen in einer UI mithilfe von drei möglichen Ansichten abrufen können: unformatiert, Steuerelement und Inhalt. Die Steuerelementansicht wird häufig von Benutzeroberflächenautomatisierungsclients verwendet, da sie eine gute Darstellung und Organisation der interaktiven Elemente einer UI bietet. Mithilfe von Testtools können Sie in der Regel auswählen, welche Strukturansicht verwendet werden soll, wenn das Tool die Organisation von Elementen darstellt.

Standardmäßig werden alle von [**Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Control) Element abgeleiteten Klassen und einige andere Elemente in der Steuerelement Ansicht angezeigt, wenn das Benutzeroberflächenautomatisierungs-Framework die Benutzeroberfläche für eine Windows-APP darstellt. Es kann aber vorkommen, dass ein Element in der Steuerelementansicht aufgrund einer UI-Komposition nicht angezeigt werden soll, bei der das Element Informationen dupliziert oder Informationen darstellt, die für Barrierefreiheitsszenarien unwichtig sind. Ändern Sie mithilfe der angefügten [**AutomationProperties.AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty)-Eigenschaft, wie Elemente für die Strukturansicht verfügbar gemacht werden. Wenn Sie ein Element in der **Raw** -Struktur platzieren, melden die meisten Hilfstechnologien dieses Element nicht als Teil ihrer Ansichten. Um Beispiele für die Funktionsweise in vorhandenen Steuerelementen zu erhalten, öffnen Sie die XAML-Entwurfsreferenzdatei „generic.xaml“ in einem Text-Editor und suchen in den Vorlagen nach **AutomationProperties.AccessibilityView** .

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>Name aus innerem Text  
Damit bereits in der sichtbaren Benutzeroberfläche vorhandene Zeichenfolgen leichter als Namen zur Verwendung durch Screenreader-Software genutzt werden können, unterstützen die meisten Steuerelemente und anderen UI-Elemente die automatische Bestimmung eines standardmäßigen Namens zur Verwendung durch Screenreader-Software. Dabei basiert der Name auf dem inneren Text im Element oder auf Zeichenfolgenwerten von Inhaltseigenschaften.

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) und **RichTextBlock** unterstützen den Wert der **Text** -Eigenschaft als standardmäßigen Namen zur Verwendung durch Screenreader-Software.
* Jegliche [**ContentControl**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)-Unterklassen verwenden eine iterative „ToString“-Technik, um Zeichenfolgen im [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)-Wert zu finden. Sie unterstützen diese Zeichenfolgen als standardmäßigen Namen für Screenreader-Programme.

> [!NOTE]
> Von der Benutzeroberflächenautomatisierung wird vorgegeben, dass der Name für Bildschirmleseprogramme nicht länger als 2048 Zeichen sein darf. Überschreitet eine zum automatischen Bestimmen des Namens zur Verwendung durch Screenreader-Software verwendete Zeichenfolge dieses Zeichenlimit, wird der Name an der entsprechenden Stelle abgeschnitten.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>Namen zur Verwendung durch Bildschirmleseprogramme für Bilder
Um die Sprachausgabe zu unterstützen und die grundlegenden Identifikationsinformationen für jedes Element der Benutzeroberfläche bereitzustellen, müssen Sie manchmal Textalternativen für nicht textliche Informationen wie Bilder und Diagramme bereitstellen (ausgenommen hiervon sind rein dekorative Elemente oder Strukturelemente). Diese Elemente verfügen nicht über inneren Text, sodass der Name zur Verwendung durch Screenreader-Software keinen berechneten Wert aufweist. Sie können den Namen zur Verwendung durch Screenreader-Software direkt angeben, indem Sie die angefügte [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)-Eigenschaft wie im folgenden Beispiel gezeigt festlegen.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

Alternativ können Sie eine Textbeschriftung hinzufügen, die in der sichtbaren Benutzeroberfläche angezeigt wird und gleichzeitig als Information zur Barrierefreiheit für die Beschriftung des Bildinhalts dient. Hier sehen Sie ein Beispiel:

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>Bezeichnungen und „LabeledBy“  
Um einem Formularelement eine Beschriftung zuzuordnen, wird empfohlen, eine [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Klasse mit **x:Name** für Beschriftungstext zu verwenden und die angefügte [**AutomationProperties.LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95))-Eigenschaft für das Formularelement so festzulegen, dass sie anhand des XAML-Namens auf die **TextBlock** -Klasse mit der Beschriftung verweist. Bei Verwendung dieses Musters wird der Fokus auf das zugeordnete Steuerelement verschoben, wenn der Benutzer auf die Beschriftung klickt, und Hilfstechnologien können den Beschriftungstext als Namen zur Verwendung durch Screenreader-Software für das Formularfeld verwenden. Das folgende Beispiel veranschaulicht diese Vorgehensweise.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>Beschreibung der Barrierefreiheit (optional)  
Eine Beschreibung der Barrierefreiheit stellt weitere Informationen zur Barrierefreiheit für ein bestimmtes Element der Benutzeroberfläche bereit. Normalerweise stellen Sie eine Beschreibung zur Verwendung durch Screenreader-Software bereit, wenn ein Name zur Verwendung durch Screenreader-Software allein den Zweck eines Elements nicht ausreichend beschreibt.

Die Sprachausgabe liest die Beschreibung eines Elements nur vor, wenn der Benutzer mit FESTSTELLTASTE+F weitere Informationen zum Element anfordert.

Der Name zur Verwendung durch Screenreader-Software soll keine vollständige Beschreibung des Steuerelementverhaltens bereitstellen, sondern das Steuerelement identifizieren. Wenn eine kurze Beschreibung für das Steuerelement nicht ausreicht, können Sie zusätzlich zu [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)die angefügte [**AutomationProperties. HelpText**](/dotnet/api/system.windows.automation.automationproperties.helptext) -Eigenschaft festlegen.

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>Frühzeitiges und häufiges Testen der Barrierefreiheit  
Der beste Ansatz beim Implementieren der Unterstützung der Sprachausgabe besteht letztlich darin, die App selbst mit einer Sprachausgabe zu testen. So können Sie sehen, wie sich die Sprachausgabe verhält und welche grundlegenden Barrierefreiheitsinformationen in der App fehlen. Anschließend können Sie die Benutzeroberfläche oder Eigenschaftswerte der Benutzeroberflächenautomatisierung entsprechend anpassen. Weitere Informationen finden Sie unter [Barrierefreiheits Tests](accessibility-testing.md).

Eines der Tools, das Sie zum Testen der Barrierefreiheit verwenden können, heißt **EH-Viewer** . Das Tool **AccScope** ist besonders nützlich, weil Sie visuelle Darstellungen Ihrer UI anzeigen können, mit denen anhand einer Automatisierungsstruktur verdeutlicht wird, wie Ihre App mit Hilfstechnologie angezeigt wird. Es ist beispielsweise ein Sprachausgabemodus vorhanden, in dem Sie sehen, wie die Sprachausgabe Text aus der App abruft und die Elemente in der UI organisiert. EH-Viewer ist so konzipiert, dass das Tool während des gesamten Entwicklungszyklus einer App verwendet werden kann und nützlich ist. Dies gilt auch für die Phase des vorläufigen Entwurfs. Weitere Informationen finden Sie unter [AccScope](/windows/desktop/WinAuto/accscope).

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>Namen zur Verwendung durch Bildschirmleseprogramme aus dynamischen Daten  
Windows unterstützt viele Steuerelemente, mit denen durch ein als *Datenbindung* bezeichnetes Feature Werte aus einer zugeordneten Datenquelle angezeigt werden können. Wenn Sie Listen mit Datenelementen auffüllen, müssen Sie nach dem Auffüllen der anfänglichen Liste möglicherweise eine Technik verwenden, die Namen zur Verwendung durch Screenreader-Software für datengebundene Listenelemente festlegt. Weitere Informationen finden Sie unter "Szenario 4" im [XAML-Barrierefreiheits Beispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample).

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>Namen zur Verwendung durch Bildschirmleseprogramme und Lokalisierung  
Um sicherzustellen, dass der Name zur Verwendung durch Screenreader-Software auch ein lokalisiertes Element ist, sollten Sie lokalisierbare Zeichenfolgen mithilfe entsprechender Techniken als Ressourcen speichern und dann mit [x:Uid-Direktiven](../../xaml-platform/x-uid-directive.md)-Werten auf die Ressourcenverbindungen verweisen. Wenn der Name zur Verwendung durch Screenreader-Software aus einer explizit festgelegten [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)-Eigenschaft stammt, muss die dort angegebene Zeichenfolge ebenfalls lokalisierbar sein.

Beachten Sie, dass angefügte Eigenschaften, z. B. die [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties)-Eigenschaften, eine spezielle Qualifizierungssyntax für den Ressourcennamen verwenden. Die Ressource verweist dann so auf die angefügte Eigenschaft, wie diese auf ein bestimmtes Element angewendet wurde. Der Ressourcenname für die [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)-Eigenschaft, die auf ein UI-Element mit dem Namen `MediumButton` angewendet wurde, lautet beispielsweise wie folgt: `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`.

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen  
* [Bedienungshilfen](accessibility.md)
* [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)
* [XAML-Beispiel für Barrierefreiheit](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Testen der Barrierefreiheit](accessibility-testing.md)
