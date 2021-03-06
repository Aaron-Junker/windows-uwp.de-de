---
title: Erstellen einer Android-App mit xamarin. Forms
description: Einstieg in das Schreiben von Android-Apps mit xamarin. Forms
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Forms, XAML, Tutorial
ms.date: 04/28/2020
ms.openlocfilehash: b6c6a00c430dcc04994c0f8fd537d7c5c2083d4e
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254259"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Einstieg in die Entwicklung für Android mit xamarin. Forms

Dieser Leitfaden hilft Ihnen beim Einstieg in die Verwendung von xamarin. Forms unter Windows zum Erstellen einer plattformübergreifenden APP, die auf Android-Geräten funktioniert.

In diesem Artikel erstellen Sie eine einfache Android-App mit xamarin. Forms und Visual Studio 2019.

## <a name="requirements"></a>Anforderungen

Um dieses Lernprogramm verwenden zu können, benötigen Sie Folgendes:

- Windows 10
- [Visual Studio 2019: Community, Professional oder Enterprise](https://visualstudio.microsoft.com/downloads/) (siehe Hinweis)
- Arbeitsauslastung "Mobile-Entwicklung mit .net" für Visual Studio 2019

> [!NOTE]
> Diese Anleitung funktioniert mit Visual Studio 2017 oder 2019. Wenn Sie Visual Studio 2017 verwenden, sind möglicherweise einige Anweisungen aufgrund von UI-unterschieden zwischen den beiden Versionen von Visual Studio falsch.

Sie müssen auch über ein Android-Telefon oder einen konfigurierten Emulator verfügen, auf dem die app ausgeführt werden soll. Siehe [Testen auf einem Android-Gerät oder-Emulator](emulator.md).

## <a name="create-a-new-xamarinforms-project"></a>Erstellen eines neuen xamarin. Forms-Projekts

Starten Sie Visual Studio. Klicken Sie auf Datei > neue > Projekt, um ein neues Projekt zu erstellen.

Wählen Sie im Dialogfeld Neues Projekt die Vorlage **Mobile App (xamarin. Forms)** aus, und klicken Sie auf **weiter**.

Nennen Sie das Projekt **timechangerforms** , und klicken Sie auf **Erstellen**.

Wählen Sie im Dialogfeld neue plattformübergreifende APP die Option **leer** aus. Aktivieren Sie im Abschnitt Plattform die Option **Android** , und deaktivieren Sie alle anderen Felder. Klicken Sie auf **OK**.

Xamarin erstellt eine neue Projekt Mappe mit zwei Projekten: **timechangerforms** und **timechangerforms. Android.**

## <a name="create-a-ui-with-xaml"></a>Erstellen einer Benutzeroberfläche mit XAML

Erweitern Sie das Projekt **timechangerforms** , und öffnen Sie die Datei **MainPage. XAML**. Der XAML-Code in dieser Datei definiert den ersten Bildschirm, den ein Benutzer beim Öffnen von timechanger sehen wird.

Die Benutzeroberfläche von timechanger ist einfach. Es zeigt die aktuelle Uhrzeit an und verfügt über Schaltflächen, um die Zeit in Schritten von einer Stunde anzupassen. Es verwendet ein vertikales Stacklayout, um die Zeit oberhalb der Schaltflächen auszurichten, und ein horizontales Stacklayout, um die Schaltflächen nebeneinander anzuordnen. Der Inhalt wird auf dem Bildschirm zentriert, indem die **horizontaloptions** -und **verticaloptions-Optionen** für das vertikale Stacklayout auf **"centerandexpand"** festgelegt werden.

Ersetzen Sie den Inhalt von "MainPage. XAML" durch den folgenden Code.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

An diesem Punkt ist die Benutzeroberfläche fertig. Timechangerforms wird jedoch nicht erstellt, da auf die Methoden **UpButton_Clicked** und **DownButton_Clicked** in der XAML verwiesen wird, die aber nicht an einer beliebigen Stelle definiert sind. Auch wenn die app ausgeführt wurde, wird die aktuelle Zeit nicht angezeigt. Im nächsten Abschnitt beheben Sie diese Fehler und fügen Ihrer Benutzeroberfläche Funktionen hinzu.

## <a name="add-logic-code-with-c"></a>Hinzufügen von Logik Code mit C #

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf MainPage. XAML, und klicken Sie dann auf **Code anzeigen**. Diese Datei enthält den Code Behind, mit dem der Benutzeroberfläche Funktionen hinzugefügt werden.

### <a name="set-the-current-time"></a>Festlegen der aktuellen Uhrzeit

Der Code in dieser Datei kann auf Steuerelemente verweisen, die in der XAML mit dem Wert des **x:Name** -Attributs des-Steuer Elements deklariert wurden. In diesem Fall wird die Bezeichnung, die die aktuelle Uhrzeit anzeigt, aufgerufen `time` .

UI-Steuerelemente müssen im Haupt Thread aktualisiert werden. Von einem anderen Thread vorgenommene Änderungen können das Steuerelement nicht ordnungsgemäß aktualisieren Da es keine Garantie gibt, dass dieser Code immer im Haupt Thread ausgeführt wird, verwenden Sie die **begininvokeonmainthread** -Methode, um sicherzustellen, dass alle Updates ordnungsgemäß angezeigt werden. Hier ist die vollständige updatetimelabel-Methode.

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>Aktuelle Zeit einmal pro Sekunde aktualisieren

An diesem Punkt wird die aktuelle Zeit für höchstens eine Sekunde nach dem Start von timechangerforms genau sein. Die Bezeichnung muss in regelmäßigen Abständen aktualisiert werden, um die Zeit exakt zu halten. Ein **Timer** -Objekt ruft in regelmäßigen Abständen eine Rückruf Methode auf, die die Bezeichnung mit der aktuellen Uhrzeit aktualisiert.

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>Sanoffset hinzufügen

Die Schaltflächen "nach oben" und "nach unten" passen die Zeit in Schritten von einer Stunde an. Fügen Sie eine **houroffset** -Eigenschaft hinzu, um die aktuelle Anpassung zu verfolgen.

```csharp
public int HourOffset { get; private set; }
```

Aktualisieren Sie nun die updatetimelabel-Methode, um die Eigenschaft "houroffset" zu beachten.

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>Click-Ereignishandler für Schaltflächen hinzufügen

Alle auf-und nach-unten-Schaltflächen müssen die houroffset-Eigenschaft Inkrement oder Dekrement erhöhen und updatetimelabel aufrufen.

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

Wenn Sie fertig sind, sollte MainPage.XAML.cs wie folgt aussehen:

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>Ausführen der App

Um die APP auszuführen, drücken Sie **F5** , oder klicken Sie auf Debuggen > Debugging starten. Abhängig davon, wie der [Debugger konfiguriert ist](emulator.md), wird die APP auf einem Gerät oder in einem Emulator gestartet.

## <a name="related-links"></a>Verwandte Links

- [Testen Sie auf einem Android-Gerät oder-Emulator](emulator.md).

- [Erstellen einer Android-Beispiel-App mit xamarin. Android](xamarin-android.md)
