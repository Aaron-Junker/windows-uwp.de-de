---
title: Erste Schritte mit der Navigation
description: Erste Schritte mit der Navigation
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 22d2f73ba6a14ace1319285ca436db4738f84548
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493295"
---
# <a name="getting-started-navigation"></a>Erste Schritte: Navigation


## <a name="adding-navigation"></a>Hinzufügen von Navigation

iOS bietet die **UINavigationController**-Klasse als In-App-Navigationshilfe: Sie erstellen eine Hierarchie von **UIViewControllers**, die Ihre App definieren, durch das Drücken und Aufklappen von Ansichten.

Eine Windows 10-App mit mehreren Ansichten holt dagegen mehr aus einem Website-orientierten Ansatz für die Navigation heraus. Sie können sich vorstellen, wie Benutzer von Seite zu Seite wechseln, indem Sie auf Steuerelemente klicken, um durch die App zu navigieren. Weitere Informationen finden Sie unter [Navigationsdesigngrundlagen](https://docs.microsoft.com/windows/uwp/layout/navigation-basics).

Bei einer Windows 10-App ist die Verwendung der [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)-Klasse eine der Methoden zum Verwalten dieser Navigation. Die folgende exemplarische Vorgehensweise veranschaulicht, wie Sie dies ausprobieren können.

Fahren Sie mit der Lösung aus früheren Schritten fort, öffnen Sie die **MainPage.xaml**-Datei, und fügen Sie eine Schaltfläche in der Ansicht **Entwurf** hinzu. Ändern Sie die **Content**-Eigenschaft der Schaltfläche von „Button“ in „Go To Page“. Erstellen Sie anschließend einen Handler für das **Click**-Ereignis der Schaltfläche, wie in der folgenden Abbildung dargestellt. Wenn Sie nicht mehr wissen, wie das geht, schlagen Sie unter der exemplarischen Vorgehensweise im vorherigen Abschnitt nach (Hinweis: Doppelklicken Sie auf die Schaltfläche in der Ansicht **Entwurf**).

![Hinzufügen einer Schaltfläche und des zugehörigen Click-Ereignisses in Visual Studio](images/ios-to-uwp/vs-go-to-page.png)

Als Nächstes fügen wir eine neue Seite hinzu. Tippen Sie in der Ansicht **Lösung** auf das Menü **Projekt**, und tippen Sie auf **Neues Element hinzufügen**. Tippen Sie wie in der folgenden Abbildung dargestellt auf **Leere Seite** und dann auf **Hinzufügen**.

![Hinzufügen einer neuen Seite in Visual Studio](images/ios-to-uwp/vs-add-new-page.png)

Fügen Sie als Nächstes der Datei „BlankPage.xaml“ eine Schaltfläche hinzu. Nun wird das AppBarButton-Steuerelement verwendet und soll ein Bild mit einem Zurück-Pfeil erhalten: Fügen Sie hierzu in der **XAML**-Ansicht ` <AppBarButton Icon="Back"/>` zwischen den `<Grid> </Grid>`-Elementen hinzu.

Nun fügen wir der Schaltfläche einen Ereignishandler hinzu: Doppelklicken Sie auf das Steuerelement in der **Entwurfs** Ansicht, und Microsoft Visual Studio fügt dem Click-Feld den Text "appbarbutton \_ Click" hinzu, wie in der folgenden Abbildung gezeigt, und fügt dann den entsprechenden Ereignishandler in der BlankPage.XAML.cs-Datei hinzu und zeigt ihn an. **Click**

![Hinzufügen einer Zurück-Schaltfläche und des zugehörigen Click-Ereignisses in Visual Studio](images/ios-to-uwp/vs-add-back-button.png)

Wenn Sie zur **XAML**-Ansicht der Datei „BlankPage.xaml“ zurückkehren, sollte der Extensible Application Markup Language (XAML)-Code des `<AppBarButton>`-Elements nun wie folgt lauten:

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

Kehren Sie zur Datei „BlankPage.xaml.cs“ zurück, und fügen Sie diesen Code hinzu, damit der Benutzer zur vorherigen Seite zurückkehrt, nachdem er auf die Schaltfläche getippt hat.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

Öffnen Sie zum Schluss die Datei „MainPage.xaml.cs“, und fügen Sie diesen Code hinzu. Mithilfe dieses Codes wird BlankPage geöffnet, nachdem der Benutzer auf die Schaltfläche getippt hat.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

Führen Sie nun das Programm aus. Tippen Sie auf die Schaltfläche „Go To Page“, um zu der anderen Seite zu wechseln, und tippen Sie dann auf die Schaltfläche mit dem Zurück-Pfeil, um zur vorherigen Seite zurückzukehren.

Die Seitennavigation wird mithilfe der [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)-Klasse verwaltet. Da die **UINavigationController** -Klasse in ios **pushviewcontroller** -und **popviewcontroller** -Methoden verwendet, stellt die **Frame** -Klasse für UWP-apps [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) -und [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) -Methoden bereit. Die **Frame**-Klasse verfügt außerdem über eine Methode mit dem Namen [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), die genau das tut, was Sie vermutlich erwartet haben.

Bei dieser exemplarischen Vorgehensweise wird immer dann eine neue Instanz von BlankPage erstellt, wenn Sie zu dieser Seite navigieren. (Die vorherige Instanz wird automatisch *freigegeben*.) Wenn nicht jedes Mal eine neue Instanz erstellt werden soll, fügen Sie dem Konstruktor der BlankPage-Klasse in der Datei „BlankPage.xaml.cs“ den folgenden Code hinzu. Dadurch wird das [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode)-Verhalten aktiviert.

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

Sie können auch die [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize)-Eigenschaft der **Frame**-Klasse abrufen oder festlegen, um zu definieren, wie viele Seiten im Navigationsverlauf zwischengespeichert werden können.

Weitere Informationen zur Navigation finden Sie unter [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) und unter [XAML-Beispiel für Charakteranimationen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20personality%20animations%20sample%20(Windows%208)).

**Hinweis**    Informationen zur Navigation für UWP-apps mit JavaScript und HTML finden Sie unter [Schnellstart: Verwenden der Einzelseiten Navigation](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10)).
 
### <a name="next-step"></a>Nächster Schritt

[Erste Schritte: Animation](getting-started-animation.md)

