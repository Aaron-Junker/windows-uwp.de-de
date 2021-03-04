---
title: Erstellen einer einfachen Android-App mit xamarin. Android
description: Eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von xamarin. Android unter Windows zum Erstellen einer plattformübergreifenden APP, die auf Android-Geräten funktioniert.
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Android, Tutorial, XAML
ms.date: 04/28/2020
ms.openlocfilehash: 3bcecf24fe6bb90dc2b94dfa62a5768481b298e5
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823174"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Einstieg in die Entwicklung für Android mit xamarin. Android

Dieser Leitfaden hilft Ihnen beim Einstieg in die Verwendung von xamarin. Android unter Windows zum Erstellen einer plattformübergreifenden APP, die auf Android-Geräten funktioniert.

In diesem Artikel erstellen Sie eine einfache Android-App mit xamarin. Android und Visual Studio 2019.

## <a name="requirements"></a>Anforderungen

Um dieses Lernprogramm verwenden zu können, benötigen Sie Folgendes:

- Windows 10
- [Visual Studio 2019: Community, Professional oder Enterprise](https://visualstudio.microsoft.com/downloads/) (siehe Hinweis)
- Arbeitsauslastung "Mobile-Entwicklung mit .net" für Visual Studio 2019

> [!NOTE]
> Diese Anleitung funktioniert mit Visual Studio 2017 oder 2019. Wenn Sie Visual Studio 2017 verwenden, sind möglicherweise einige Anweisungen aufgrund von UI-unterschieden zwischen den beiden Versionen von Visual Studio falsch.

Sie müssen auch über ein Android-Telefon oder einen konfigurierten Emulator verfügen, auf dem die app ausgeführt werden soll. Siehe [Konfigurieren eines Android-Emulators](emulator.md).

## <a name="create-a-new-xamarinandroid-project"></a>Erstellen eines neuen Xamarin.Android-Projekts

Starten Sie Visual Studio. Wählen Sie Datei > neue > Projekt aus, um ein neues Projekt zu erstellen.

Wählen Sie im Dialogfeld Neues Projekt die Vorlage **Android-App (xamarin)** aus, und klicken Sie auf **weiter**.

Nennen Sie das Projekt **timechangerandroid** , und klicken Sie auf **Erstellen**.

Wählen Sie im Dialogfeld neue plattformübergreifende APP die Option **leere App** aus. Wählen Sie in der **Android-Mindestversion** **Android 5,0 (Lollipop)** aus. Klicken Sie auf **OK**.

Xamarin erstellt eine neue Projekt Mappe mit einem einzelnen Projekt mit dem Namen **timechangerandroid**.

## <a name="create-a-ui-with-xaml"></a>Erstellen einer Benutzeroberfläche mit XAML

Öffnen Sie im Verzeichnis **resources\layout** Ihres Projekts **activity_main.xml**. Der XML-Code in dieser Datei definiert den ersten Bildschirm, den ein Benutzer beim Öffnen von timechanger sehen wird.

Die Benutzeroberfläche von timechanger ist einfach. Es zeigt die aktuelle Uhrzeit an und verfügt über Schaltflächen, um die Zeit in Schritten von einer Stunde anzupassen. Er verwendet eine vertikale `LinearLayout` , um die Zeit oberhalb der Schaltflächen und eine horizontale `LinearLayout` anzuordnen, um die Schaltflächen nebeneinander anzuordnen. Der Inhalt wird auf dem Bildschirm zentriert, indem das **Android: Gravity** - **Attribut auf zentriert** in der vertikalen festgelegt wird `LinearLayout` .

Ersetzen Sie den Inhalt **activity_main.xml** durch den folgenden Code.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

An diesem Punkt können Sie **timechangerandroid** ausführen und die Benutzeroberfläche anzeigen, die Sie erstellt haben. Im nächsten Abschnitt fügen Sie der Benutzeroberfläche Funktionen zum Anzeigen der aktuellen Uhrzeit und zum Aktivieren der Schaltflächen zum Ausführen einer Aktion hinzu.

## <a name="add-logic-code-with-c"></a>Hinzufügen von Logik Code mit C #

Öffnen Sie **MainActivity.cs**. Diese Datei enthält die Code-Behind-Logik, mit der die Funktionalität der Benutzeroberfläche hinzugefügt wird.

### <a name="set-the-current-time"></a>Festlegen der aktuellen Uhrzeit

Beziehen Sie zuerst einen Verweis auf den, `TextView` der die Uhrzeit anzeigt. Verwenden Sie **findviewbyid** , um alle Elemente der Benutzeroberfläche mit der richtigen **Android: ID** (die `"@+id/timeDisplay"` im XML-Code aus dem vorherigen Schritt auf festgelegt wurde) zu durchsuchen. Dies ist die `TextView` , die die aktuelle Uhrzeit anzeigt.

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

UI-Steuerelemente müssen im UI-Thread aktualisiert werden. Von einem anderen Thread vorgenommene Änderungen können das Steuerelement nicht ordnungsgemäß aktualisieren Da es keine Garantie gibt, dass dieser Code immer im UI-Thread ausgeführt wird, verwenden Sie die **runonuithread** -Methode, um sicherzustellen, dass alle Updates ordnungsgemäß angezeigt werden. Dies ist die komplette `UpdateTimeLabel` Methode.

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>Aktuelle Zeit einmal pro Sekunde aktualisieren

An diesem Punkt wird die aktuelle Zeit für höchstens eine Sekunde nach dem Start von timechangerandroid genau sein. Die Bezeichnung muss in regelmäßigen Abständen aktualisiert werden, um die Zeit exakt zu halten. Ein **Timer** -Objekt ruft in regelmäßigen Abständen eine Rückruf Methode auf, die die Bezeichnung mit der aktuellen Uhrzeit aktualisiert.

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>Erstellen der Click-Ereignishandler für die Schaltfläche

Alle auf-und nach-unten-Schaltflächen müssen die houroffset-Eigenschaft Inkrement oder Dekrement erhöhen und updatetimelabel aufrufen.

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>Richten Sie die nach-oben-und nach-unten-Schaltflächen an die entsprechenden Ereignishandler

Um die Schaltflächen den entsprechenden Ereignis Handlern zuzuordnen, verwenden Sie zuerst findviewbyid, um die Schaltflächen anhand ihrer IDs zu suchen. Sobald Sie über einen Verweis auf das Schaltflächen Objekt verfügen, können Sie dem Ereignis einen Ereignishandler hinzufügen `Click` .

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>Abgeschlossene MainActivity.cs-Datei

Wenn Sie fertig sind, sollte MainActivity.cs wie folgt aussehen:

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>Ausführen der App

Um die APP auszuführen, drücken Sie **F5** , oder klicken Sie auf Debuggen > Debugging starten. Abhängig davon, wie der [Debugger konfiguriert ist](emulator.md), wird die APP auf einem Gerät oder in einem Emulator gestartet.

## <a name="related-links"></a>Verwandte Links

- [Testen Sie auf einem Android-Gerät oder-Emulator](emulator.md).
- [Erstellen einer Android-Beispiel-App mit xamarin. Forms](xamarin-forms.md)
