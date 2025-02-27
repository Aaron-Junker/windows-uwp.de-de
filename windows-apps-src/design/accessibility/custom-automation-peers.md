---
description: Beschreibt das Konzept von Automatisierungspeers für die Microsoft-Benutzeroberflächenautomatisierung und erläutert, wie Sie eine Unterstützung der Benutzeroberflächenautomatisierung für eine eigene benutzerdefinierte UI-Klasse bereitstellen können.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Benutzerdefinierte Automatisierungspeers
label: Custom automation peers
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 21f583cc529092b28aa8bc3efde9de6247a4373a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032513"
---
# <a name="custom-automation-peers"></a>Benutzerdefinierte Automatisierungspeers  

Beschreibt das Konzept von Automatisierungspeers für die Microsoft-Benutzeroberflächenautomatisierung und erläutert, wie Sie eine Unterstützung der Benutzeroberflächenautomatisierung für eine eigene benutzerdefinierte UI-Klasse bereitstellen können.

Mit der Benutzeroberflächenautomatisierung wird ein Framework bereitgestellt, das Automatisierungsclients verwenden können, um die Benutzeroberflächen einer Vielzahl von UI-Plattformen und Frameworks zu untersuchen oder zu betreiben. Wenn Sie eine Windows-app schreiben, bieten die Klassen, die Sie für die Benutzeroberfläche verwenden, bereits Unterstützung für die Benutzeroberflächen Automatisierung. Sie können vorhandene nicht versiegelte -Klassen ableiten, um einen neuen UI-Steuerelementtyp oder eine Unterstützungsklasse zu definieren. Hierbei kann mit Ihrer Klasse möglicherweise Verhalten zur Unterstützung der Barrierefreiheit hinzugefügt werden, das die Standard-Benutzeroberflächenautomatisierung nicht bereitstellt. In diesem Fall sollten Sie die vorhandene Benutzeroberflächenautomatisierungs-Unterstützung erweitern, indem Sie von der [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) -Klasse ableiten, die von der Basis Implementierung verwendet wurde, indem Sie die erforderliche Unterstützung ihrer Peer Implementierung hinzufügen und die Windows-App-Steuerungs Infrastruktur darüber informieren, dass Sie Ihren neuen Peer erstellen sollte

Die Benutzeroberflächenautomatisierung ermöglicht nicht nur Anwendungen für die Barrierefreiheit und Hilfstechnologien wie Bildschirmleseprogramme, sondern auch Qualitätssicherungscode (Testcode). In beiden Szenarien können Benutzeroberflächenautomatisierungs-Clients die Benutzeroberflächenelemente untersuchen und die Benutzerinteraktion mit Ihrer App für Code simulieren, der nicht aus Ihrer App stammt. Informationen zur plattformübergreifenden Benutzeroberflächenautomatisierung und ihrer weiteren Bedeutung finden Sie unter [Übersicht über die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-uiautomationoverview).

Es gibt zwei unterschiedliche Zielgruppen, von denen das Benutzeroberflächenautomatisierungs-Framework verwendet wird.

* **Benutzeroberflächenautomatisierungs- *Clients*** werden Benutzeroberflächenautomatisierungs-APIs aufgerufen, um mehr über die gesamte Benutzeroberfläche zu erfahren Beispielsweise fungiert eine Hilfstechnologie wie etwa eine Bildschirmsprachausgabe als Benutzeroberflächenautomatisierungs-Client. Die Benutzeroberfläche wird als Struktur aus verwandten Automatisierungselementen dargestellt. Der Benutzeroberflächenautomatisierungs-Client benötigt u. U. Informationen zu jeweils nur einer App oder aber zur gesamten Struktur. Der Benutzeroberflächenautomatisierungs-Client kann mithilfe von Benutzeroberflächenautomatisierungs-APIs in der Struktur navigieren und Informationen in den Automatisierungselementen lesen oder ändern.
* **Benutzeroberflächenautomatisierungs- *Anbieter*** tragen Informationen zur Benutzeroberflächenautomatisierungs-Struktur bei, indem APIs implementiert werden, die die Elemente in der Benutzeroberfläche verfügbar machen, die Sie als Teil Ihrer APP eingeführt haben Wenn Sie ein neues Steuerelement erstellen, sollten Sie jetzt als Teilnehmer des Benutzeroberflächenautomatisierungs-Anbieterszenarios fungieren. Als Anbieter sollten Sie aus Gründen der Barrierefreiheit und zu Testzwecken sicherstellen, dass alle Benutzeroberflächenautomatisierungs-Clients das Benutzeroberflächenautomatisierungs-Framework für die Interaktion mit Ihrem Steuerelement nutzen können.

Gewöhnlich sind im Benutzeroberflächenautomatisierungs-Framework parallele APIs vorhanden: eine API für Benutzeroberflächenautomatisierungs-Clients und eine andere API mit einem ähnlichen Namen für Benutzeroberflächenautomatisierungs-Anbieter. In diesem Thema geht es hauptsächlich um die APIs für den Benutzeroberflächenautomatisierungs-Anbieter und speziell um die Klassen und Schnittstellen, die eine Anbietererweiterbarkeit in diesem Benutzeroberflächenframework ermöglichen. Gelegentlich werden auch von Benutzeroberflächenautomatisierungs-Clients verwendete Benutzeroberflächenautomatisierungs-APIs angesprochen, um eine andere Perspektive zu bieten oder in einer Suchtabelle einen Vergleich von Client- und Anbieter-APIs zu veranschaulichen. Weitere Informationen zur Client Perspektive finden Sie im [Programmier Handbuch für Benutzeroberflächenautomatisierungs-Clients](/windows/desktop/WinAuto/uiauto-clientportal).

> [!NOTE]
> Benutzeroberflächenautomatisierungs-Clients verwenden normalerweise keinen verwalteten Code und werden meist auch nicht als UWP-App implementiert (sondern als Desktop-Apps). Die Benutzeroberflächenautomatisierung basiert auf einem Standard, nicht auf einer bestimmten Implementierung oder einem bestimmten Framework. Viele vorhandene Benutzeroberflächenautomatisierungs-Clients, z. B. Hilfstechnologieprodukte wie Bildschirmleseprogramme, verwenden Component Object Model (COM)-Schnittstellen für die Interaktion mit der Benutzeroberflächenautomatisierung, dem System und den in untergeordneten Fenstern ausgeführten Apps. Weitere Informationen zu COM-Schnittstellen und zum Schreiben eines Benutzeroberflächenautomatisierungs-Clients mithilfe von COM finden Sie unter [Grundlagen der Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/entry-uiautocore-overview).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Ermitteln des vorhandenen Zustands der Benutzeroberflächenautomatisierung für benutzerdefinierte UI-Klassen  
Bevor Sie versuchen, einen Automatisierungspeer für ein benutzerdefiniertes Steuerelement zu implementieren, sollten Sie testen, ob die Basisklasse und der Automatisierungspeer bereits die erforderliche Barrierefreiheit oder Automatisierungsunterstützung bereitstellen. In vielen Fällen kann die Kombination aus [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)-Implementierungen, bestimmten Peers und den Mustern, die diese implementieren, eine einfache Barrierefreiheit bieten, die den Anforderungen genügt. Dies hängt davon ab, wie viele Änderungen Sie an der Objektmodellverfügbarkeit Ihres Steuerelements im Vergleich zur Basisklasse vorgenommen haben. Zudem hängt dies davon ab, ob Ihre Ergänzungen der Basisklassenfunktionalität mit den neuen UI-Elementen im Vorlagenvertrag oder mit der visuellen Darstellung des Steuerelements korrelieren. In einigen Fällen führen Ihre Änderungen möglicherweise zu neuen Aspekten der Benutzererfahrung, die eine zusätzliche Unterstützung der Barrierefreiheit erfordern.

Auch wenn durch die Verwendung der vorhandenen Basispeerklasse eine grundlegende Unterstützung für die Barrierefreiheit bereitgestellt wird, empfiehlt es sich, einen Peer zu definieren. So können Sie der Benutzeroberflächenautomatisierung genaue **ClassName** -Informationen für automatisierte Testszenarien angeben. Diese Überlegung ist besonders wichtig, wenn Sie ein Steuerelement für den Einsatz in Drittanbieterlösungen entwerfen.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Automatisierungspeerklassen  
Die UWP baut auf vorhandenen Techniken und Konventionen für die Benutzeroberflächenautomatisierung auf, die von vorherigen Benutzeroberflächenframeworks mit verwaltetem Code verwendet wurden, z. B. Windows Forms, Windows Presentation Foundation (WPF) und Microsoft Silverlight. Viele der Steuerelementklassen und deren Funktion und Zweck entstammen auch einem vorherigen Benutzeroberflächenframework.

Gemäß der Konvention beginnen Namen von Peerklassen mit dem Klassennamen des Steuerelements und enden mit „AutomationPeer“. Beispielsweise ist [**ButtonAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer) die Peerklasse für die [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)-Steuerelementklasse.

> [!NOTE]
> Im Rahmen dieses Themas räumen wir den Eigenschaften, die sich auf die Barrierefreiheit beziehen, beim Implementieren eines Peers für ein Steuerelement eine höhere Priorität ein. In einem allgemeineren Konzept für die Unterstützung der Benutzeroberflächenautomatisierung sollten Sie jedoch einen Peer gemäß den Empfehlungen im [Programmierhandbuch für Benutzeroberflächenautomatisierungs-Anbieter](/windows/desktop/WinAuto/uiauto-providerportal) und in den [Grundlagen der Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/entry-uiautocore-overview) implementieren. In diesen Themen werden zwar keine spezifischen [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)-APIs behandelt, die Sie zum Bereitstellen der Informationen im UWP-Framework für die Benutzeroberflächenautomatisierung verwenden können, aber es werden die Eigenschaften beschrieben, mit denen Ihre Klasse identifiziert wird oder andere Informationen oder Interaktionen bereitgestellt werden.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Peers, Muster und Steuerelementtypen  
Ein *Steuerelementmuster* ist eine Schnittstellenimplementierung, die einen bestimmten Aspekt der Funktionalität eines Steuerelements für einen Benutzeroberflächenautomatisierungs-Client verfügbar macht. Benutzeroberflächenautomatisierungs-Clients verwenden die Eigenschaften und Methoden, die über ein Steuerelementmuster verfügbar gemacht werden, um Informationen über die Funktionen des Steuerelements abzurufen oder das Verhalten des Steuerelements zur Laufzeit zu ändern.

Steuerelementmuster bieten eine Möglichkeit zum Kategorisieren und Verfügbarmachen der Funktionalität eines Steuerelements, unabhängig vom Typ des Steuerelements oder vom Erscheinungsbild des Steuerelements. Ein Steuerelement, das eine tabellarische Oberfläche darstellt, verwendet das **Grid** -Steuerelementmuster, um die Anzahl der Zeilen und Spalten in der Tabelle verfügbar zu machen und einem Benutzeroberflächenautomatisierungs-Client zu ermöglichen, Objekte aus der Tabelle abzurufen. Der Benutzeroberflächenautomatisierungs-Client kann das **Invoke** -Steuerelementmuster für aufrufbare Steuerelemente (z. B. Schaltflächen) und das **Scroll** -Steuerelementmuster für Steuerelemente mit Bildlaufleisten (z. B. Listenfelder, Listenansichten oder Kombinationsfelder) verwenden. Jedes Steuerelementmuster stellt eine separate Funktion dar, und Steuerelementmuster können kombiniert werden, um den gesamten Funktionsumfang zu beschreiben, der von einem bestimmten Steuerelement unterstützt wird.

Steuerelementmuster verhalten sich zur Benutzeroberfläche wie Schnittstellen zu COM-Objekten. In COM können Sie von einem Objekt abfragen, welche Schnittstellen es unterstützt, und dann mithilfe dieser Schnittstellen auf die Funktionen zugreifen. Bei der Benutzeroberflächenautomatisierung können Benutzeroberflächenautomatisierungs-Clients von einem Benutzeroberflächenautomatisierungs-Element abfragen, welche Steuerelementmuster es unterstützt, und dann über die vom unterstützten Steuerelementmuster verfügbar gemachten Eigenschaften, Methoden, Ereignisse und Strukturen mit dem Element und dem Peersteuerelement interagieren.

Eine der Hauptaufgaben eines Automatisierungspeers besteht darin, dem Benutzeroberflächenautomatisierungs-Client zu melden, welche Steuerelementmuster das UI-Element über seinen Peer unterstützen kann. Hierzu werden von Benutzeroberflächenautomatisierungs-Anbietern neue Peers implementiert, mit denen das Verhalten der [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern)-Methode durch Überschreiben der [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)-Methode geändert wird. Benutzeroberflächenautomatisierungs-Clients führen Aufrufe durch, die vom Benutzeroberflächenautomatisierungs-Anbieter dem aufrufenden **GetPattern** -Element zugeordnet werden. Benutzeroberflächenautomatisierungs-Clients fragen jedes spezielle Muster ab, mit dem sie interagieren möchten. Wenn der Peer das Muster unterstützt, gibt er einen Objektverweis auf sich selbst oder andernfalls **null** zurück. Wird nicht **null** zurückgegeben, erwartet der Benutzeroberflächenautomatisierungs-Client, dass er APIs der Musterschnittstelle als Client aufrufen kann, um mit diesem Steuerelementmuster zu interagieren.

Ein *Steuerelementtyp* ist eine Möglichkeit, die Funktionalität eines vom Peer dargestellten Steuerelements allgemein zu definieren. Dieses Konzept unterscheidet sich insofern von dem eines Steuerelementmusters, da ein Muster die Benutzeroberflächenautomatisierung informiert, welche Informationen sie abrufen kann oder welche Aktionen sie über eine bestimmte Schnittstelle ausführen kann, während sich der Steuerelementtyp eine Ebene höher befindet. Jeder Steuerelementtyp verfügt über Informationen zu den folgenden Aspekten der Benutzeroberflächenautomatisierung:

* Steuerelementmuster für die Benutzeroberflächenautomatisierung: Ein Steuerelementtyp kann mehrere Muster unterstützen, die jeweils eine andere Klassifizierung von Informationen oder Interaktionen darstellen. Jeder Steuerelementtyp verfügt über einen Satz von Steuerelementmustern, die das Steuerelement unterstützen muss. Dabei ist ein Satz optional, und der andere darf vom Steuerelement nicht unterstützt werden.
* Eigenschaftswerte für die Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp hat einen Satz von Eigenschaften, die das Steuerelement unterstützen muss. Dabei handelt es sich um die allgemeinen Eigenschaften, die unter [Übersicht über Eigenschaften für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-propertiesoverview) beschrieben werden, nicht um die musterspezifischen Eigenschaften.
* Ereignisse für die Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp hat einen Satz von Ereignissen, die das Steuerelement unterstützen muss. Diese sind ebenfalls allgemein und nicht Muster spezifisch, wie in [Übersicht über Benutzeroberflächen Automatisierungs Ereignisse](/windows/desktop/WinAuto/uiauto-eventsoverview)beschrieben.
* Baumstruktur der Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp definiert, wie das Steuerelement in der Baumstruktur der Benutzeroberflächenautomatisierung dargestellt werden muss.

Unabhängig von der Implementierungsweise von Automatisierungspeers für das Framework sind die Funktionen von Benutzeroberflächenautomatisierungs-Clients nicht an die UWP gebunden. Es ist sogar wahrscheinlich, dass vorhandene Benutzeroberflächenautomatisierungs-Clients wie beispielsweise Hilfstechnologien andere Programmiermodelle (z. B. COM) verwenden. In COM können Clients **QueryInterface** für die Schnittstelle des COM-Steuerelementmusters, das das angeforderte Muster implementiert, oder das allgemeine Benutzeroberflächenautomatisierungs-Framework für Eigenschaften, Ereignisse oder die Strukturuntersuchung verwenden. Mithilfe des Benutzeroberflächenautomatisierungs-Frameworks wird für die Muster das Marshalling in UWP-Code vorgenommen, der für den Benutzeroberflächenautomatisierungs-Anbieter der App und den entsprechenden Peer ausgeführt wird.

Wenn Sie Steuerelement Muster für ein Framework mit verwaltetem Code implementieren, z. b. eine UWP-APP \# , die C-oder Microsoft-Visual Basic verwendet, können Sie diese Muster mithilfe .NET Framework Schnittstellen anstelle der COM-Schnittstellen Darstellung darstellen. Beispielsweise ist die UI-Automatisierungs Muster Schnittstelle für eine Microsoft .NET Anbieter Implementierung des **Aufruf** Musters [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider).

Eine Liste mit Steuerelementmustern, Anbieterschnittstellen und deren Zweck finden Sie unter [Steuerelementmuster und -schnittstellen](control-patterns-and-interfaces.md). Die Liste der Steuerelement Typen finden Sie unter [UI Automation Control Types Overview](/windows/desktop/WinAuto/uiauto-controltypesoverview).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Leitfaden für die Implementierung von Steuerelementmustern  
Die Steuerelement Muster und deren Ziel sind Teil einer größeren Definition des Benutzeroberflächenautomatisierungs-Frameworks und gelten nicht nur für die Barrierefreiheits Unterstützung für eine UWP-app. Wenn Sie ein Steuerelement Muster implementieren, sollten Sie sicherstellen, dass Sie es auf eine Weise implementieren, die mit den in diesen Dokumentationen dokumentierten Anleitungen und auch in der Benutzeroberflächenautomatisierungs-Spezifikation übereinstimmt. Wenn Sie nach Anleitungen suchen, können Sie die Microsoft-Dokumentation im Allgemeinen verwenden und müssen sich nicht auf die Spezifikation beziehen. Einen Leitfaden für die einzelnen Muster finden Sie unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Jedes Thema in diesem Bereich enthält einen Abschnitt mit Implementierungsrichtlinien und -konventionen und einen mit den erforderlichen Membern. Die Richtlinien beziehen sich in der Regel auf bestimmte APIs der relevanten Steuerelementmuster-Schnittstelle in der Referenz [Steuerelementmuster-Schnittstellen für Anbieter](/windows/desktop/WinAuto/uiauto-cpinterfaces). Bei diesen Schnittstellen handelt es sich um die systemeigenen Schnittstellen bzw. COM-Schnittstellen (und die zugehörigen APIs verwenden eine Syntax im COM-Stil). Für alles, was Sie hier sehen, gibt es jedoch eine Entsprechung im [**Windows.UI.Xaml.Automation.Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider)-Namespace.

Wenn Sie die Standardautomatisierungspeers verwenden und deren Verhalten erweitern, ist zu beachten, dass diese Peers bereits gemäß den Richtlinien für die Benutzeroberflächenautomatisierung für Anbieter geschrieben wurden. Wenn sie Steuerelementmuster unterstützen, können Sie sich darauf verlassen, dass die jeweiligen Muster den Richtlinien unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) entsprechen. Wenn ein Steuerelementpeer meldet, dass er einen durch die Benutzeroberflächenautomatisierung definierten Steuerelementtyp darstellt, orientiert sich dieser Peer an den unter [Unterstützen von Steuerelementtypen für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) dokumentierten Richtlinien.

Trotzdem benötigen Sie u. U. weitere Richtlinien für Steuerelementmuster oder -typen, um in Ihrer Peerimplementierung den Empfehlungen für die Benutzeroberflächenautomatisierung zu folgen. Dies ist besonders dann der Fall, wenn Sie eine Unterstützung für Muster oder Steuerelementtypen implementieren, die noch nicht als Standardimplementierung in einem UWP-Steuerelement vorhanden ist. Das Muster für Anmerkungen ist z. B. in keinem der standardmäßigen XAML-Steuerelemente implementiert. Vielleicht haben Sie aber eine App, die Anmerkungen intensiv nutzt, und möchten daher den Zugriff auf diese Funktionalität ermöglichen. Für dieses Szenario sollte Ihr Peer [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) implementieren und sich vermutlich als **Document** -Steuerelementtyp mit entsprechenden Eigenschaften melden. Dies ist ein Hinweis darauf, dass Ihre Dokumente Anmerkungen unterstützen.

Wir empfehlen, die Ratschläge für die Muster unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) oder für Steuerelementtypen unter [Unterstützen von Steuerelementtypen für die Benutzeroberflächenautomatisierung](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) zur Orientierung und als allgemeinen Leitfaden zu nutzen. Sie können auch einigen der API-Links folgen, unter denen Sie Beschreibungen und Anmerkungen zum Zweck der APIs finden. Einzelheiten zur speziellen Syntax für die Programmierung von UWP-Apps finden Sie unter der entsprechenden API im [**Windows.UI.Xaml.Automation.Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider)-Namespace und den zugehörigen Referenzseiten.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Integrierte Automatisierungspeerklassen  
Im Allgemeinen implementieren Elemente eine Automatisierungspeerklasse, wenn sie UI-Aktivität vom Benutzer akzeptieren. Ein weiterer Fall ist das Vorhandensein von Informationen, die Benutzer von Hilfstechnologien benötigen, mit denen die interaktive oder aussagekräftige Benutzeroberfläche von Apps dargestellt wird. Nicht alle visuellen UWP-Elemente verfügen über Automatisierungspeers. Beispiele für Klassen, die Automatisierungspeers implementieren, sind [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) und [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox). Beispiele für Klassen, die keine Automatisierungspeers implementieren, sind [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) und Klassen auf Basis von [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel), z. B. [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) und [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas). Ein **Panel** weist keinen Peer auf, weil es nur ein visuelles Layoutverhalten bereitstellt. Benutzer haben keine für die Barrierefreiheit relevante Möglichkeit, mit einem **Panel** zu interagieren. Unabhängig davon, welche untergeordneten Elemente ein **Panel** enthält, werden diese den Benutzeroberflächenautomatisierungs-Bäumen als untergeordnete Elemente des nächsten verfügbaren übergeordneten Elements in der Struktur gemeldet, das über eine Peer- oder Elementdarstellung verfügt.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>UWP-Prozessgrenzen und Benutzeroberflächenautomatisierung  
In der Regel wird Benutzeroberflächenautomatisierungs-Client-Code, der auf eine UWP-App zugreift, außerhalb des Prozesses ausgeführt. Die Infrastruktur des Benutzeroberflächenautomatisierungs-Frameworks ermöglicht es, Informationen über Prozessgrenzen hinweg zu übertragen. Dieses Konzept wird in den [Grundlagen der Benutzeroberflächen Automatisierung](/windows/desktop/WinAuto/entry-uiautocore-overview)ausführlicher erläutert.

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Alle Klassen, die von [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) abgeleitet werden, enthalten die geschützte virtuelle Methode [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer). Die Objektinitialisierungssequenz für Automatisierungspeers ruft **OnCreateAutomationPeer** auf, um das Automatisierungspeerobjekt für jedes Steuerelement abzurufen und so einen Benutzeroberflächenautomatisierungs-Baum zur Laufzeitverwendung zu erstellen. Der Benutzeroberflächenautomatisierungs-Code kann den Peer verwenden, um Informationen über die Merkmale und Features eines Steuerelements abzurufen und mithilfe der Steuerelementmuster die interaktive Verwendung des Steuerelements zu simulieren. Ein benutzerdefiniertes Steuerelement, das Automatisierung unterstützt, muss **OnCreateAutomationPeer** überschreiben und eine Instanz der Klasse zurückgeben, die von [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) abgeleitet wird. Wenn beispielsweise ein benutzerdefiniertes Steuerelement von der Klasse [**ButtonBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) abgeleitet wird, sollte das von **OnCreateAutomationPeer** zurückgegebene Objekt von [**ButtonBaseAutomationPeer**](/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer) abgeleitet werden.

Wenn Sie eine benutzerdefinierte Steuerelementklasse schreiben und außerdem einen neuen Automatisierungspeer bereitstellen möchten, sollten Sie die [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)-Methode für das benutzerdefinierte Steuerelement überschreiben, sodass eine neue Instanz Ihres Peers zurückgegeben wird. Ihre Peer Klasse muss direkt oder indirekt von [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)abgeleitet werden.

Mit dem folgenden Code wird beispielsweise deklariert, dass das benutzerdefinierte `NumericUpDown`-Steuerelement den Peer `NumericUpDownPeer` zur Benutzeroberflächenautomatisierung verwenden soll.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> Die [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)-Implementierung sollte lediglich eine neue Instanz des benutzerdefinierten Automatisierungspeers initialisieren, das aufrufende Steuerelement als Besitzer übergeben und die neue Instanz zurückgeben. Versuchen Sie nicht, in diese Methode zusätzliche Logik einzubinden. Besonders Logik, die möglicherweise zur Zerstörung des [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)-Elements innerhalb desselben Aufrufs führt, kann ein unvorhergesehenes Laufzeitverhalten zur Folge haben.

In typischen Implementierungen von [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) ist *owner* als **this** oder **Me** angegeben, da die Überschreibung der Methode den gleichen Umfang wie der Rest der Steuerelement-Klassendefinition hat.

Die tatsächliche Peerklassendefinition kann in derselben Codedatei wie das Steuerelement oder in einer separaten Codedatei erfolgen. Die Peerdefinitionen befinden sich alle im [**Windows.UI.Xaml.Automation.Peers**](/uwp/api/Windows.UI.Xaml.Automation.Peers)-Namespace, also getrennt von den Steuerelementen, für die sie Peers bereitstellen. Sie können Ihre Peers auch in separaten Namespaces deklarieren, sofern Sie auf die erforderlichen Namespaces für den [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)-Methodenaufruf verweisen.

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Auswählen der richtigen Peerbasisklasse  
Stellen Sie sicher, dass Ihr [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) von der Basisklasse abgeleitet wird, die am besten mit der vorhandenen Peerlogik der Steuerelementklasse übereinstimmt, von der Sie ableiten. Da `NumericUpDown` von [**RangeBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase) abgeleitet wird, ist im vorherigen Beispiel eine [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)-Klasse verfügbar, auf der Ihr Peer basieren sollte. Indem Sie entsprechend der Art, wie Sie das Steuerelement selbst ableiten, die am besten passende Peerklasse verwenden, können Sie zumindest für einen Teil der [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider)-Funktionalität eine Überschreibung vermeiden, da diese bereits von der Basispeerklasse implementiert wird.

Die [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control)-Basisklasse besitzt keine entsprechende Peerklasse. Wenn Sie eine Peerklasse benötigen, die mit einem benutzerdefinierten Steuerelement übereinstimmt, das von **Control** abgeleitet wird, leiten Sie die benutzerdefinierte Peerklasse von [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) ab.

Wenn Sie direkt von [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) ableiten, hat diese Klasse kein standardmäßiges Automatisierungspeerverhalten, da keine [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)-Implementierung vorhanden ist, die auf eine Peerklasse verweist. Implementieren Sie also entweder **OnCreateAutomationPeer** , um Ihren eigenen Peer zu verwenden, oder verwenden Sie [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) als Peer, wenn die hiermit bereitgestellte Unterstützung für die Barrierefreiheit für Ihr Steuerelement ausreichend ist.

> [!NOTE]
> Normalerweise führen Sie keine Ableitungen von [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) anstelle von [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) durch. Wenn Sie von **AutomationPeer** direkt abgeleitet haben, müssen Sie viele grundlegende Barrierefreiheits Unterstützung duplizieren, die andernfalls von **FrameworkElementAutomationPeer** stammen würde.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Initialisieren einer benutzerdefinierten Peerklasse  
Der Automatisierungspeer sollte einen typsicheren Konstruktor definieren, der eine Instanz des Besitzersteuerelements für die Basisinitialisierung verwendet. Im nächsten Beispiel übergibt die-Implementierung den *Besitzer* Wert an die [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) -Basis, und schließlich ist es der [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) , der den *Besitzer* verwendet, um " [**FrameworkElementAutomationPeer. Owner**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)" festzulegen.

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Core-Methoden von AutomationPeer  
Aufgrund der UWP-Infrastruktur sind die überschreibbaren Methoden eines Automatisierungspeers Teil eines Methodenpaars – der öffentlichen Zugriffsmethode, die der Benutzeroberflächenautomatisierungs-Anbieter als Weiterleitungspunkt für Benutzeroberflächenautomatisierungs-Clients verwendet, und der geschützten „Core“-Anpassungsmethode, die von einer UWP-Klasse überschrieben werden kann, um das Verhalten zu beeinflussen. Das Methodenpaar wird standardmäßig so gebündelt, dass mit dem Aufruf der Zugriffsmethode stets die parallele „Core“-Methode mit der Anbieterimplementierung oder (als Fallback) eine Standardimplementierung aus den Basisklassen aufgerufen wird.

Überschreiben Sie beim Implementieren eines Peers für ein benutzerdefiniertes Steuerelement eine beliebige „Core“-Methode aus der Basis-Automatisierungspeerklasse, in der Sie für Ihr benutzerdefiniertes Steuerelement spezifisches Verhalten verfügbar machen möchten. Der Benutzeroberflächenautomatisierungs-Code ruft durch Aufruf der öffentlichen Methoden der Peerklasse Informationen über Ihr Steuerelement ab. Um Informationen zu Ihrem Steuerelement bereitzustellen, überschreiben Sie jede Methode, deren Name auf „Core“ endet, wenn durch die Implementierung und das Design Ihres Steuerelements Barrierefreiheitsszenarien oder andere Benutzeroberflächenautomatisierungs-Szenarien erstellt werden, die von jenen abweichen, die von der Basis-Automatisierungspeerklasse unterstützt werden.

Zumindest sollten Sie jedes Mal, wenn Sie eine neue Peerklasse definieren, die Methode [**GetClassNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) implementieren, wie im nächsten Beispiel gezeigt.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Es ist ratsam, die Zeichenfolgen als Konstanten zu speichern, anstatt direkt in der Methode. Die Entscheidung liegt jedoch bei Ihnen. Für [**GetClassNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) müssen Sie diese Zeichenfolge nicht lokalisieren. Die **LocalizedControlType** -Eigenschaft wird immer dann verwendet, wenn eine lokalisierte Zeichenfolge von einem Benutzeroberflächenautomatisierungs-Client, nicht von **className** benötigt wird.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Einige Hilfstechnologien verwenden den Wert [**GetAutomationControlType**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) direkt, wenn sie Merkmale der Elemente eines Benutzeroberflächenautomatisierungs-Baums angeben, um zusätzliche Informationen über das **Name** -Element der Benutzeroberflächenautomatisierung hinaus bereitzustellen. Wenn sich Ihr Steuerelement erheblich von dem Steuerelement unterscheidet, von dem Sie ableiten, und Sie einen anderen Steuerelementtyp melden möchten als den, der durch die vom Steuerelement verwendete Basispeerklasse gemeldet wird, müssen Sie einen Peer implementieren und in Ihrer Implementierung des Peers [**GetAutomationControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) überschreiben. Dies ist besonders wichtig, wenn Sie von einer allgemeinen Basisklasse wie [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) oder [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) ableiten, in der der Peer keine genauen Informationen über den Steuerelementtyp liefert.

Ihre Implementierung von [**GetAutomationControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) beschreibt Ihr Steuerelement, indem ein [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)-Wert zurückgegeben wird. Obwohl Sie **AutomationControlType.Custom** zurückgeben können, sollten Sie einen der spezifischeren Steuerelementtypen zurückgeben, wenn dieser die Hauptszenarien Ihres Steuerelements genauer beschreibt. Hier sehen Sie ein Beispiel.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> Nur wenn Sie [**AutomationControlType.Custom**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) angeben, müssen Sie [**GetLocalizedControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) implementieren, um für Clients einen **LocalizedControlType** -Eigenschaftswert bereitzustellen. Die allgemeine Infrastruktur für die Benutzeroberflächen Automatisierung stellt übersetzte Zeichen folgen für alle möglichen **AutomationControlType** -Werte außer **AutomationControlType. Custom** bereit.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern und GetPatternCore  
Die Peerimplementierung von [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) gibt das Objekt zurück, von dem das im Eingabeparameter angeforderte Muster unterstützt wird. Dabei ruft ein Benutzeroberflächenautomatisierungs-Client eine Methode auf, die an die [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern)-Methode des Anbieters weitergeleitet wird, und gibt einen [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)-Enumerationswert an, mit dem das angeforderte Muster benannt wird. Ihre Überschreibung von **GetPatternCore** sollte das Objekt zurückgeben, mit dem das angegebene Muster implementiert wird. Bei diesem Objekt handelt es sich um den Peer selbst, da von diesem bei jeder Meldung, dass er ein Muster unterstützt, die entsprechende Musterschnittstelle implementiert werden sollte. Wenn Ihr Peer über keine benutzerdefinierte Implementierung eines Musters verfügt, Sie jedoch wissen, dass das Muster von der Basis des Peers implementiert wird, können Sie die Implementierung des Basistyps von **GetPatternCore** über **GetPatternCore** aufrufen. Das **GetPatternCore** eines Peers sollte **null** zurückgeben, wenn ein Muster nicht vom Peer unterstützt wird. Anstatt jedoch **null** direkt aus Ihrer Implementierung zurückzugeben, würden Sie gewöhnlich den Aufruf an die Basisimplementierung verwenden, um für jedes nicht unterstützte Muster **null** zurückzugeben.

Wenn ein Muster unterstützt wird, kann die [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)-Implementierung **this** oder **Me** zurückgeben. Es wird erwartet, dass der Benutzeroberflächenautomatisierungs-Client den [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) -Rückgabewert in die angeforderte Muster Schnittstelle wandelt, wenn er nicht **null** ist.

Wenn eine Peerklasse von einem anderen Peer erbt und die notwendige Unterstützung und das Melden des Musters bereits von der Basisklasse behandelt wird, ist die Implementierung von [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) nicht notwendig. Wenn Sie beispielsweise ein Bereichssteuerelement implementieren, das von [**RangeBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase) abgeleitet wird, und Ihr Peer von [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) abgeleitet wird, gibt dieser Peer sich selbst für [**PatternInterface.RangeValue**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) zurück und besitzt funktionierende Implementierungen der [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider)-Schnittstelle, die das Muster unterstützt.

Obwohl es sich nicht um den literalen Code handelt, wird in diesem Beispiel die Implementierung von [**getpatterncore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) , die bereits in [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)vorhanden ist, angegeben.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Wenn Sie einen Peer implementieren, der nicht die gesamte Unterstützung bietet, die Sie von einer Basispeerklasse benötigen, oder wenn Sie die Gruppe der Muster, die von der Basis geerbt und von Ihrem Peer unterstützt werden, ändern oder ergänzen möchten, sollten Sie [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) überschreiben, damit Benutzeroberflächenautomatisierungs-Clients die Muster verwenden können.

Unter [**Windows.UI.Xaml.Automation.Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider) finden Sie eine Liste der Anbietermuster, die in der UWP-Implementierung der Benutzeroberflächenautomatisierungs-Unterstützung verfügbar sind. Jedes solche Muster hat einen entsprechenden Wert der [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)-Enumeration. Dies ist die Art, wie Benutzeroberflächenautomatisierungs-Clients das Muster in einem [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern)-Aufruf anfordern.

Ein Peer kann melden, dass er mehrere Muster unterstützt. In diesem Fall sollte die Überschreibung eine Rückgabepfadlogik für jeden unterstützten [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)-Wert enthalten und den Peer bei jeder Übereinstimmung zurückgeben. Es wird erwartet, dass der Aufrufer jeweils nur eine Schnittstelle anfordert und dass der Aufrufer entscheiden kann, ob eine Umwandlung für die erwartete Schnittstelle durchgeführt wird.

Im Folgenden finden Sie ein Beispiel für eine [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)-Überschreibung für einen benutzerdefinierten Peer. Der Peer meldet, dass die beiden Muster [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) und [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) unterstützt werden. Bei dem hier verwendeten Steuerelement handelt es sich um ein Steuerelement zum Anzeigen von Medien, das als Vollbild (Umschaltmodus) dargestellt werden kann und über eine Fortschrittsleiste verfügt, auf der Benutzer eine Position (das Bereichssteuerelement) auswählen können. Dieser Code stammt aus dem [XAML-Barrierefreiheits Beispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Weiterleiten von Mustern aus Unterelementen  
Eine [**getpatterncore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) -Methoden Implementierung kann auch ein Unterelement oder einen Teil als Muster Anbieter für den Host angeben. Dieses Beispiel zeigt, wie mit [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) die Behandlung des Bildlaufmusters an den Peer des internen [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)-Steuerelements übergeben wird. Um ein Unterelement für die Musterbehandlung anzugeben, ruft dieser Code das Unterelementobjekt ab, erstellt mit der [**FrameworkElementAutomationPeer.CreatePeerForElement**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement)-Methode einen Peer für das Unterelement und gibt den neuen Peer zurück.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Andere Core-Methoden  
Ihr Steuerelement muss vielleicht Tastaturentsprechungen für primäre Szenarien unterstützen. Weitere Informationen darüber, warum dies erforderlich sein kann, finden Sie unter [Barrierefreiheit für den Tastaturzugriff](keyboard-accessibility.md). Die Implementierung der Schlüsselunterstützung ist zwingend Teil des Codes für das Steuerelement und nicht des Codes für den Peer, da es sich hierbei um einen Teil der Logik von Steuerelementen handelt. Ihre Peerklasse sollte die [**GetAcceleratorKeyCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore)- und [**GetAccessKeyCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore)-Methoden überschreiben, um den Benutzeroberflächenautomatisierungs-Clients zu melden, welche Schlüssel verwendet werden. Bedenken Sie, dass Zeichenfolgen, in denen Schlüsselinformationen gemeldet werden, eventuell lokalisiert werden müssen und daher aus Ressourcen stammen sollten, nicht aus hartcodierten Zeichenfolgen.

Wenn Sie einen Peer für eine Klasse bereitstellen, die eine Sammlung unterstützt, sollte am besten sowohl von Funktionsklassen als auch von Peerklassen abgeleitet werden, die bereits diese Art von Sammlungsunterstützung besitzen. Wenn dies nicht möglich ist, müssen Peers für Steuerelemente, die untergeordnete Sammlungen verwalten, möglicherweise die sammlungsbezogene Peermethode [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) überschreiben, um dem Benutzeroberflächenautomatisierungs-Baum die Beziehungen zwischen übergeordneten und untergeordneten Elementen korrekt zu melden.

Implementieren Sie die Methoden [**IsContentElementCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) und [**IsControlElementCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore), um anzugeben, ob Ihr Steuerelement Dateninhalte enthält und/oder eine interaktive Rolle auf der Benutzeroberfläche ausübt. Standardmäßig geben beide Methoden **true** zurück. Diese Einstellungen verbessern die Benutzerfreundlichkeit von Hilfstechnologien wie Bildschirmleseprogrammen, die diese Methoden möglicherweise zum Filtern der Automatisierungsstruktur verwenden. Wenn die [**getpatterncore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) -Methode die Muster Verarbeitung an einen Teil Element-Peer überträgt, kann die **IsControlElementCore** -Methode des untergeordneten Elements **false** zurückgeben, um den Teil Element Peer aus der Automatisierungs Struktur auszublenden.

Einige Steuerelemente stellen möglicherweise Unterstützung für Bezeichnungsszenarien bereit, in denen ein Teil mit einer Textbezeichnung Informationen für einen Teil ohne Text liefert, oder in denen sich ein Steuerelement in einer bekannten Bezeichnungsbeziehung zu einem anderen Steuerelement in der Benutzeroberfläche befinden soll. Wenn es möglich ist, ein nützliches klassenbasiertes Verhalten bereitzustellen, können Sie [**GetLabeledByCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) überschreiben, um dieses Verhalten bereitzustellen.

[**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) und [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) werden hauptsächlich für automatisierte Testszenarien verwendet. Wenn Sie automatisierte Tests für Ihr Steuerelement unterstützen möchten, sollten Sie diese Methoden überschreiben. Dies kann für Bereichssteuerelemente nützlich sein, bei denen Sie nicht nur einen einzelnen Punkt vorschlagen können, weil sich abhängig von der Klickposition des Benutzers im Koordinatenbereich jeweils eine andere Auswirkung auf einen Bereich ergibt. Beispielsweise überschreibt der standardmäßige [**ScrollBar**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)-Automatisierungspeer **GetClickablePointCore** , um einen „keine Zahl“-Wert des Typs [**Point**](/uwp/api/Windows.Foundation.Point) zurückzugeben.

[**GetLiveSettingCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) beeinflusst den Steuerelementstandardwert für den **LiveSetting** -Wert für die Benutzeroberflächenautomatisierung. Sie sollten dies überschreiben, wenn das Steuerelement einen anderen Wert als [**AutomationLiveSetting.Off**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting) zurückgeben soll. Weitere Informationen zu den Funktionen von **livesetts** finden Sie unter [**AutomationProperties. livesesetting**](/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty).

Sie können [**GetOrientationCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) überschreiben, wenn das Steuerelement über eine Eigenschaft mit festlegbarer Ausrichtung verfügt, die [**AutomationOrientation**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation) zugeordnet werden kann. Dies ist mit den Klassen [**ScrollBarAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) und [**SliderAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) möglich.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Basisimplementierung in FrameworkElementAutomationPeer  
Die Basisimplementierung von [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) bietet einige Informationen zur Benutzeroberflächenautomatisierung, die von verschiedenen, auf der Frameworkebene definierten Layout- und Verhaltenseigenschaften interpretiert werden können.

* [**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): Gibt eine [**Rect**](/uwp/api/Windows.Foundation.Rect)-Struktur auf der Basis der bekannten Layouteigenschaften zurück. Gibt einen 0-Wert **Rect** zurück, wenn [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen)**true** ist.
* [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): Gibt eine [**Point**](/uwp/api/Windows.Foundation.Point)-Struktur basierend auf den bekannten Layouteigenschaften zurück, solange ein **BoundingRectangle** ungleich Null vorhanden ist.
* [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): Hier können umfassendere Verhaltensweisen zusammengefasst werden; siehe [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore). Grundsätzlich wird versucht, eine Zeichenfolgenkonvertierung für alle bekannten Inhalte einer [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl)-Klasse oder verwandter Klassen auszuführen, die über Inhalte verfügen. Wenn ein Wert für [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) vorhanden ist, wird darüber hinaus der **Name** -Wert dieses Elements als **Name** verwendet.
* [**HasKeyboardFocusCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): Ausgewertet basierend auf den Eigenschaften [**FocusState**](/uwp/api/windows.ui.xaml.controls.control.focusstate) und [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) des Besitzers. Elemente, die keine Steuerelemente sind, geben stets **false** zurück.
* [**IsEnabledCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): Ausgewertet basierend auf der Eigenschaft [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled), wenn diese ein [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) ist. Elemente, die keine Steuerelemente sind, geben stets **true** zurück. Dies bedeutet nicht, dass der Besitzer im herkömmlichen Sinn einer Interaktion aktiviert wird, sondern dass der Peer aktiviert wird, obwohl der Besitzer nicht über die Eigenschaft **IsEnabled** verfügt.
* [**IsKeyboardFocusableCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): Gibt **true** zurück, wenn der Besitzer ein [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) ist, andernfalls **false** .
* [**IsOffscreenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): Eine [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) mit dem Wert [**Collapsed**](/uwp/api/windows.ui.xaml.visibility) für das Besitzerelement oder eines seiner übergeordneten Elemente entspricht dem Wert **true** für [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Ausnahme: Ein [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)-Objekt kann sichtbar sein, auch wenn dies nicht auf seine übergeordneten Elemente zutrifft.
* [**SetFocus Core**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): Ruft den [**Fokus**](/uwp/api/windows.ui.xaml.controls.control.focus)auf.
* [**GetParent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): Ruft [**FrameworkElement.Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) vom Besitzer auf und findet den entsprechenden Peer. Da es sich hier nicht um ein Überschreibungspaar mit einer Core-Methode handelt, können Sie dieses Verhalten nicht ändern.

> [!NOTE]
> Standard-UWP-Peers implementieren ein Verhalten mithilfe von systemeigenem Code, der die UWP implementiert, und nicht unbedingt mit tatsächlichem UWP-Code. Sie können den Code oder die Logik der Implementierung nicht mittels Common Language Runtime (CLR)-Spiegelung oder anderer Verfahren anzeigen. Außerdem sehen Sie keine einzelnen Referenzseiten in der Referenz für unterklassenspezifische Überschreibungen von Basispeerverhalten. Beispielsweise gibt es möglicherweise zusätzliche Verhaltensweisen für das [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore)-Element von [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer), die nicht auf der **AutomationPeer.GetNameCore** -Referenzseite beschrieben werden, und es ist keine Referenzseite für **TextBoxAutomationPeer.GetNameCore** vorhanden. Es gibt noch nicht einmal eine **TextBoxAutomationPeer.GetNameCore** -Referenzseite. Lesen Sie stattdessen das Referenzthema für die unmittelbarste Peerklasse, und suchen Sie im Abschnitt für Anmerkungen nach Implementierungshinweisen.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Peers und %%amp;quot;AutomationProperties%%amp;quot;  
Ihr Automatisierungspeer sollte entsprechende Standardwerte für die Informationen bereitstellen, die sich auf die Barrierefreiheit des Steuerelements beziehen. Beachten Sie, dass jeder App-Code, der das Steuerelement verwendet, einen Teil dieses Verhaltens überschreiben kann, indem Werte für die angefügte [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) Eigenschaft in die Steuerelementinstanzen eingeschlossen werden. Aufrufer können dies entweder für die Standardsteuerelemente oder für benutzerdefinierte Steuerelemente ausführen. Mithilfe des folgenden XAML wird beispielsweise eine Schaltfläche erstellt, die über zwei angepasste Benutzeroberflächenautomatisierungs-Eigenschaften verfügt: `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Weitere Informationen zu den Eigenschaften der angefügten [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) finden Sie unter [grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md).

Einige [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)-Methoden sind aufgrund des allgemeinen Vertrags vorhanden, der festlegt, wie Benutzeroberflächenautomatisierungs-Anbieter Informationen melden sollen. Jedoch werden diese Methoden in der Regel nicht in Steuerelementpeers implementiert. Dies liegt daran, dass erwartet wird, dass die [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties)-Werte, die auf den App-Code angewendet werden, der die Steuerelemente auf einer bestimmten Benutzeroberfläche verwendet, diese Informationen liefern. Beispielweise würde die Mehrzahl der Apps die Bezeichnungsbeziehung zwischen zwei verschiedenen Steuerelementen in der Benutzeroberfläche definieren, indem sie den Wert [**AutomationProperties.LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) anwendet. [**LabeledByCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) ist jedoch in bestimmten Peers implementiert, die Daten- oder Objektbeziehungen in einem Steuerelement darstellen, beispielsweise die Verwendung einer Headerkomponente zum Bezeichnen einer Datenfeldkomponente, das Bezeichnen von Objekten mit ihren Containern oder ähnliche Szenarien.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implementieren von Mustern  
Als Nächstes wird beschrieben, wie ein Peer für ein Steuerelement geschrieben wird, mit dem durch die Implementierung der Steuerelement-Musterschnittstelle für Erweitern/Reduzieren das Verhalten zum Erweitern/Reduzieren implementiert wird. Der Peer sollte die Barrierefreiheit für das Erweitern/Reduzieren ermöglichen, indem er jedes Mal, wenn [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) mit einem Wert von [**PatternInterface.ExpandCollapse**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) aufgerufen wird, sich selbst zurückgibt. Anschließend sollte der Peer die Anbieterschnittstelle für dieses Muster ( [**IExpandCollapseProvider**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) erben und Implementierungen für alle Member dieser Anbieterschnittstelle bereitstellen. In diesem Fall muss die Schnittstelle drei Member überschreiben: [**Expand**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) und [**ExpandCollapseState**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Es ist hilfreich, die Barrierefreiheit im API-Entwurf der Klasse selbst vorauszuplanen. Stellen Sie für ein Verhalten, das möglicherweise durch typische Benutzerinteraktionen mit der UI oder ein Automatisierungsanbietermuster angefordert wird, eine einzelne Methode bereit, die entweder als Benutzeroberflächenreaktion oder durch das Automatisierungsmuster aufgerufen werden kann. Wenn Ihr Steuerelement beispielsweise Schaltflächenteile mit verknüpften Ereignishandlern, die das Steuerelement erweitern oder reduzieren können, sowie Tastaturentsprechungen für diese Aktionen enthält, sollten diese Ereignishandler die gleiche Methode aufrufen, die aus den [**Expand**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand)- oder [**Collapse**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse)-Implementierungen für [**IExpandCollapseProvider**](/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider) im Peer aufgerufen wird. Es kann auch nützlich sein, mit einer allgemeinen Logikmethode sicherzustellen, dass die visuellen Zustände Ihres Steuerelements aktualisiert werden, um den logischen Zustand einheitlich anzuzeigen, unabhängig davon, wie das Verhalten aufgerufen wurde.

Bei einer typischen Implementierung wird von den Anbieter-APIs zuerst der [**Owner**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) aufgerufen, um Zugriff zur Laufzeit auf die Steuerelementinstanz zu erhalten. Danach können die erforderlichen Verhaltensmethoden für dieses Objekt aufgerufen werden.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Eine alternative Implementierungsmöglichkeit besteht darin, dass das Steuerelement selbst auf seinen Peer verweist. Dies ist ein häufig verwendetes Muster, wenn Automatisierungsereignisse über das Steuerelement ausgelöst werden, da die [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent)-Methode eine Peermethode ist.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Benutzeroberflächenautomatisierungs-Ereignisse  

Benutzeroberflächenautomatisierungs-Ereignisse werden in die folgenden Kategorien unterteilt.

| Ereignis | BESCHREIBUNG |
|-------|-------------|
| Eigenschaftenänderung | Wird ausgelöst, wenn eine Eigenschaft eines Benutzeroberflächenautomatisierungs-Elements oder Steuerelementmusters geändert wird. Wenn ein Client beispielsweise das Steuerelement für das Kontrollkästchen einer App überwachen muss, kann dieser sich registrieren, um auf ein Eigenschaftsänderungsereignis im Hinblick auf die [**ToggleState**](/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate)-Eigenschaft zu hören. Wenn das Steuerelement für das Kontrollkästchen aktiviert oder deaktiviert wird, wird das Ereignis vom Anbieter ausgelöst, und der Client kann die erforderliche Aktion ausführen. |
| Elementaktion | Wird ausgelöst, wenn durch eine Aktivität des Benutzers oder eine programmgesteuerte Aktivität eine Änderung der Benutzeroberfläche ausgelöst wird, beispielsweise wenn über das Muster **Invoke** auf eine Schaltfläche geklickt oder diese aufgerufen wird. |
| Strukturänderung | Wird ausgelöst, wenn der Benutzeroberflächenautomatisierungs-Baum geändert wird. Die Struktur wird geändert, wenn neue Benutzeroberflächenelemente angezeigt, ausgeblendet oder vom Desktop entfernt werden. |
| Globale Änderung | Wird ausgelöst, wenn Aktionen ausgeführt werden, die den Client global betreffen, beispielsweise wenn der Fokus von einem Element auf ein anderes gewechselt wird oder wenn ein untergeordnetes Fenster geschlossen wird. Einige Ereignisse bedeuten nicht zwangsläufig, dass sich der Zustand der Benutzeroberfläche geändert hat. Wenn der Benutzer beispielsweise zu einem Texteingabefeld navigiert und anschließend auf eine Schaltfläche klickt, um das Feld zu aktualisieren, wird auch dann ein [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged)-Ereignis ausgelöst, wenn der Benutzer den Text nicht geändert hat. Bei der Verarbeitung eines Ereignisses muss eine Clientanwendung möglicherweise erst überprüfen, ob sich tatsächlich etwas geändert hat, bevor eine Aktion ausgeführt wird. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>AutomationEvents-Bezeichner  
Benutzeroberflächenautomatisierungs-Ereignisse werden durch [**AutomationEvents**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents)-Werte identifiziert. Durch die Werte der Enumeration wird die Art des Ereignisses eindeutig identifiziert.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Auslösen von Ereignissen  
Benutzeroberflächenautomatisierungs-Clients können Automatisierungsereignisse abonnieren. Im Automatisierungspeermodell müssen Peers für benutzerdefinierte Steuerelemente Änderungen am Zustand des Steuerelements, die für die Barrierefreiheit relevant sind, durch den Aufruf der [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent)-Methode melden. Entsprechend sollten die Peers des benutzerdefinierten Steuerelements die [**RaisePropertyChangedEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent)-Methode aufrufen, wenn sich der Wert einer wichtigen Benutzeroberflächenautomatisierungs-Eigenschaft ändert.

Das folgende Codebeispiel zeigt, wie das Peerobjekt aus dem Steuerelement-Definitionscode abgerufen und eine Methode aufgerufen wird, um von diesem Peer aus ein Ereignis auszulösen. Zur Optimierung stellt der Code fest, ob es Listener für diesen Ereignistyp gibt. Wenn das Ereignis nur ausgelöst und das Peerobjekt nur erstellt wird, wenn Listener vorhanden sind, entsteht kein unnötiger Aufwand, und das Steuerelement bleibt reaktionsfähig.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Peernavigation  
Nachdem ein Automatisierungspeer gefunden wurde, kann ein Benutzeroberflächenautomatisierungs-Client in der Peerstruktur einer App navigieren, indem die Methoden [**GetChildren**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) und [**GetParent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) des Peerobjekts aufgerufen werden. Die Navigation zwischen Benutzeroberflächenelementen in einem Steuerelement wird von der Implementierung der [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore)-Methode des Peers unterstützt. Das Benutzeroberflächenautomatisierungs-System ruft diese Methode auf, um eine Struktur der in einem Steuerelement enthaltenen Unterelemente zu erstellen, beispielsweise von Listenelementen in einem Listenfeld. Die **GetChildrenCore** -Standardmethode in [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) durchläuft die visuelle Struktur der Elemente, um die Struktur der Automatisierungspeers zu erstellen. Benutzerdefinierte Steuerelemente können diese Methode überschreiben, um eine andere Darstellung der untergeordneten Elemente für Automatisierungsclients verfügbar zu machen. Dabei werden die Automatisierungspeers der Elemente zurückzugeben, die Informationen übermitteln oder Benutzerinteraktion erlauben.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Systemeigene Automatisierungsunterstützung für Textmuster  
Einige der Standardautomatisierungspeers für UWP-Apps bieten Unterstützung für Steuerelementmuster ( [**PatternInterface.Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)). Diese Unterstützung erfolgt jedoch über systemeigene Methoden, sodass die betroffenen Peers nicht die [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider)-Schnittstelle in der (verwalteten) Vererbung benachrichtigen. Wenn ein verwalteter oder nicht verwalteter Benutzeroberflächenautomatisierungs-Client den Peer nach Mustern abfragt, meldet dieser dennoch die Unterstützung des Textmusters und stellt Verhaltensweisen für Teile des Musters bereit, wenn die Client-APIs aufgerufen werden.

Wenn Sie von einem Textsteuerelement einer UWP-App ableiten möchten, können Sie auch einen benutzerdefinierten Peer erstellen, der von einem der textspezifischen Peers abgeleitet ist. Im Abschnitt "Anmerkungen" des Peers finden Sie weitere Informationen zur möglichen systemeigenen Unterstützung von Mustern. Der Zugriff auf das systemeigene Verhalten Ihres benutzerdefinierten Peers erfolgt durch den Aufruf der Basisimplementierung Ihrer verwalteten Anbieterschnittstellenimplementierungen. es ist aber schwierig, das Verhalten der Basisimplementierung zu ändern, da die systemeigenen Schnittstellen weder im Peer noch im Eigentümersteuerelement verfügbar gemacht werden. Sie sollten im Allgemeinen entweder die Basisimplementierungen wie bereitgestellt verwenden (durch Aufruf der Basis) oder die Funktionen vollständig im eigenen verwalteten Code ersetzen und die Basisimplementierung gar nicht aufrufen. Im letzten Fall handelt es sich um ein fortgeschrittenes Szenario. Sie müssen mit dem von Ihrem Steuerelement verwendeten Textdienstframework vertraut sein, um die Anforderungen hinsichtlich der Barrierefreiheit zu unterstützen, wenn Sie dieses Framework verwenden.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
Zusätzlich zum Bereitstellen eines benutzerdefinierten Peers können Sie auch die Strukturansichtdarstellung für beliebige Steuerelementinstanzen anpassen, indem Sie im XAML-Code [**AutomationProperties.AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) festlegen. Dies wird zwar nicht als Teil einer Peerklasse implementiert, wird hier jedoch erwähnt, weil es für die allgemeine Barrierefreiheitsunterstützung für benutzerdefinierte Steuerelemente oder für von Ihnen angepasste Vorlagen relevant ist.

Das Hauptszenario für die Verwendung von **AutomationProperties.AccessibilityView** ist das absichtliche Auslassen bestimmter Steuerelemente in einer Vorlage aus den Benutzeroberflächenautomatisierungs-Ansichten, weil sie keinen nennenswerten Beitrag zur Barrierefreiheitsansicht des gesamten Steuerelements leisten. Um dies zu verhindern, legen Sie **AutomationProperties.AccessibilityView** auf „Raw“ fest.

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Auslösen von Ausnahmen über Automatisierungspeers  
Die APIs, die Sie für Ihre Automatisierungspeerunterstützung implementieren, dürfen Ausnahmen auslösen. Es wird erwartet, dass lauschende Benutzeroberflächenautomatisierungs-Clients stabil genug sind, um nach dem Auslösen der meisten Ausnahmen weiter zu funktionieren. Aller Wahrscheinlichkeit nach betrachtet der Listener eine Automatisierungsgesamtstruktur, die neben Ihrer eigenen App auch andere Apps enthält. Das Clientdesign darf nicht zum Ausfall des gesamten Clients führen, wenn beim Aufrufen der APIs in einem Bereich der Struktur eine peerbasierte Ausnahme ausgelöst wird.

Parameter, die an Ihren Peer übergeben werden, können die Eingabe überprüfen und beispielsweise [**ArgumentNullException**](/dotnet/api/system.argumentnullexception) auslösen, wenn **null** übergeben wurde und dies für Ihre Implementierung kein gültiger Wert ist. Wenn jedoch nachfolgende Vorgänge von Ihrem Peer ausgeführt werden, denken Sie daran, dass die Interaktionen des Peers mit dem hostenden Steuerelement einen gewissen asynchronen Charakter haben können. Durch Aktionen eines Peers wird nicht zwangsläufig der Benutzeroberflächenthread im Steuerelement blockiert (dies sollte vermutlich auch nicht geschehen). Daher sind Situationen denkbar, in denen ein Objekt bei der Erstellung des Peers oder beim ersten Aufruf einer Automatisierungspeermethode verfügbar war oder bestimmte Eigenschaften hatte, während sich der Zustand des Steuerelements in der Zwischenzeit geändert hat. Für diese Fälle gibt es zwei spezielle Ausnahmen, die ein Anbieter auslösen kann:

* Lösen Sie [**ElementNotAvailableException**](/dotnet/api/system.windows.automation.elementnotavailableexception) aus, wenn Sie basierend auf den ursprünglich an Ihre APIs übergebenen Informationen nicht auf den Besitzer des Peers oder ein verwandtes Peerelement zugreifen können. Beispielsweise gibt es möglicherweise einen Peer, der seine Methoden auszuführen versucht, dessen Besitzer jedoch in der Zwischenzeit aus der Benutzeroberfläche entfernt wurde. Dies ist beispielsweise der Fall, wenn ein modales Dialogfeld geschlossen wird. Bei einem Non-.NET-Client wird dies [**UIA \_ E \_ elementnotavailable**](/windows/desktop/WinAuto/uiauto-error-codes)zugeordnet.
* Lösen Sie [**ElementNotEnabledException**](/dotnet/api/system.windows.automation.elementnotenabledexception) aus, wenn zwar noch ein Besitzer vorhanden ist, dieser sich jedoch in einem Modus wie [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled)`=`**false** befindet, der einen Teil der spezifischen programmgesteuerten Änderungen blockiert, die der Peer auszuführen versucht. Bei einem Non-.NET-Client wird dies [**UIA \_ E \_ elementnotenabled**](/windows/desktop/WinAuto/uiauto-error-codes)zugeordnet.

Darüber hinaus sollten Peers mit Ausnahmen, die sie über die Peerunterstützung auslösen, vergleichsweise konservativ umgehen. Die meisten Clients können Ausnahmen von Peers nicht verarbeiten und wandeln diese in Wahlmöglichkeiten mit ausführbaren Aktionen um, die Benutzer bei Interaktionen mit dem Client auswählen können. Manchmal ist es also eine bessere Strategie, keinen Vorgang auszuführen und Ausnahmen ohne erneutes Auslösen in Ihren Peerimplementierungen abzufangen, als bei jeder fehlgeschlagenen Aktion des Peers Ausnahmen auszulösen. Berücksichtigen Sie auch, dass die meisten Benutzeroberflächenautomatisierungs-Clients nicht in verwaltetem Code geschrieben werden. Die meisten werden in com geschrieben und überprüfen immer, ob Sie in einem **HRESULT** immer **\_ OK** sind, wenn Sie eine Benutzeroberflächenautomatisierungs-Client Methode aufrufen, die letztendlich auf Ihren Peer zugreift.

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen  
* [Bedienungshilfen](accessibility.md)
* [XAML-Beispiel für Barrierefreiheit](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Steuerelement Muster und-Schnittstellen](control-patterns-and-interfaces.md)
