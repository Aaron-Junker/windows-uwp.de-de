---
title: Starten der Standard-App für eine Datei
description: Erfahren Sie, wie Sie die Standard-App für eine Datei starten.
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 94011a50bd339b98b6bb77ff82f5863d8c89c603
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318725"
---
# <a name="launch-the-default-app-for-a-file"></a>Starten der Standard-App für eine Datei

**Wichtige APIs**

-   [**Windows.System.Launcher.LaunchFileAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync)

Erfahren Sie, wie Sie die Standard-App für eine Datei starten. Viele Apps müssen mit Dateien arbeiten, die sie nicht selbst behandeln können. E-Mail-Apps erhalten beispielsweise eine Vielzahl von Dateitypen und müssen über eine Möglichkeit verfügen, diese Dateien mit ihren Standardhandlern zu starten. In den folgenden Schritten erfahren Sie, wie Sie den Standardhandler für eine Datei, die Ihre App nicht selbst behandeln kann, mithilfe der [**Windows.System.Launcher**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher)-API starten.

## <a name="get-the-file-object"></a>Abrufen des Dateiobjekts

Rufen Sie zunächst ein [**Windows.Storage.StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt für die Datei ab.

Wenn die Datei im Paket für die App enthalten ist, können Sie die [**Package.InstalledLocation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.installedlocation)-Eigenschaft verwenden, um ein [**Windows.Storage.StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)-Objekt abzurufen, und die [**Windows.Storage.StorageFolder.GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)-Methode, um das [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt abzurufen.

Falls sich die Datei in einem bekannten Ordner befindet, können Sie die Eigenschaften der [**Windows.Storage.KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)-Klasse verwenden, um ein [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)-Objekt abzurufen, und die [**GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)-Methode, um das [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt abzurufen.

## <a name="launch-the-file"></a>Starten der Datei

Windows stellt mehrere verschiedene Optionen zum Starten des Standardhandlers für eine Datei bereit. Diese Optionen werden in diesem Diagramm und den anschließenden Abschnitten ausführlich beschrieben.

| Option | Methode | Beschreibung |
|--------|--------|-------------|
| Standardstart | [**LaunchFileAsync(IStorageFile)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync) | Starten Sie die angegebene Datei mit dem Standardhandler. |
| Starten über „Öffnen mit“ | [**LaunchFileAsync(IStorageFile, LauncherOptions)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | Starten Sie die angegebene Datei und lassen Sie den Benutzer den Handler über das Dialogfeld „Öffnen mit“ auswählen. |
| Start mit einem empfohlenen App-Fallback | [**LaunchFileAsync(IStorageFile, LauncherOptions)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | Starten Sie die angegebene Datei mit dem Standardhandler. Wenn kein Handler im System installiert ist, empfehlen Sie dem Benutzer eine App im Store. |
| Starten mit einer gewünschten verbleibenden Ansicht | [**LaunchFileAsync(IStorageFile, LauncherOptions)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) (Windows-only) | Starten Sie die angegebene Datei mit dem Standardhandler. Geben Sie an, wie lange die App nach dem Start auf dem Bildschirm verbleiben soll. Fordern Sie zudem eine bestimmte Fenstergröße an. [**LauncherOptions.DesiredRemainingView** ](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) für mobile Geräte wird nicht unterstützt. |

### <a name="default-launch"></a>Standardstart

Rufen Sie die [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync)-Methode auf, um die Standard-App zu starten. In diesem Beispiel wird die [**Windows.Storage.StorageFolder.GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)-Methode verwendet, um eine im App-Paket enthaltene Bilddatei zu starten.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>Starten über „Öffnen mit“

Rufen Sie die [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_)-Methode mit dem [**LauncherOptions.DisplayApplicationPicker**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.displayapplicationpicker)-Wert **true** auf, um die App zu starten, die der Benutzer im Dialogfeld **Öffnen mit** auswählt.

Mit dem Dialogfeld **Öffnen mit** können Sie dem Benutzer das Auswählen einer anderen App als der Standard-App für einen bestimmten Dateityp ermöglichen. Angenommen, Ihre App ermöglicht dem Benutzer das Aufrufen einer Bilddatei. In diesem Fall ist der Standardhandler wahrscheinlich eine Anzeige-App. Nun kann es jedoch sein, dass der Benutzer die Datei nicht nur anzeigen, sondern auch bearbeiten möchte. Mit der Option **Öffnen mit** und einem alternativen Befehl in der **AppBar** oder in einem Kontextmenü können Sie dem Benutzer das Aufrufen des Dialogfelds **Öffnen mit** und das Auswählen einer entsprechenden Editor-App ermöglichen.

![Das Dialogfeld „Öffnen mit“ für den Start einer PNG-Datei. Das Dialogfeld enthält ein Kontrollkästchen, in dem angegeben wird, ob die Benutzerauswahl für alle PNG-Dateien oder nur für diese PNG-Datei verwendet werden soll. Das Dialogfeld enthält vier App-Optionen für den Start der Datei und einen Link „Weitere Optionen“.](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**Starten Sie mit der eine empfohlene app fallback**

Es kann jedoch sein, dass der Benutzer nicht über die erforderliche App zum Bearbeiten des aufgerufenen Dateityps verfügt. In diesen Fällen bietet Windows standardmäßig einen Link an, mit dessen Hilfe Benutzer im Store nach einer geeigneten App suchen können. Wenn Sie dem Benutzer eine bestimmte App für dieses spezifische Szenario empfehlen möchten, können Sie die Empfehlung zusammen mit der Datei weitergeben, die gestartet wird. Rufen Sie dazu die [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_)-Methode mit [**LauncherOptions.PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) (festgelegt auf den Paketfamiliennamen der empfohlenen App im Store) auf. Legen Sie anschließend [**LauncherOptions.PreferredApplicationDisplayName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationdisplayname) auf den Namen dieser App fest. Diese Informationen werden von Windows verwendet, um die allgemeine Option zum Suchen einer App im Store durch die spezifische Option zum Verwenden der empfohlenen App im Store zu ersetzen.

> [!NOTE]
> Sie müssen diese beiden Optionen, wird empfohlen, eine app festlegen. Eine Festlegung der einen ohne die andere führt zu einem Fehler.

![Das Dialogfeld „Öffnen mit“ für den Start einer „.contoso“-Datei. Da auf dem Computer kein Handler für „.contoso“ installiert ist, enthält das Dialogfeld eine Option mit dem Store-Symbol und Text, der den Benutzer auf den richtigen Handler im Store verweist. Das Dialogfeld enthält auch einen Link „Weitere Optionen“.](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>Starten mit einer gewünschten verbleibenden Ansicht (nur Windows)

Quell-Apps, die [**LaunchFileAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync) aufrufen, können anfordern, nach dem Start einer Datei auf dem Bildschirm zu verbleiben. Standardmäßig wird von Windows versucht, den gesamten verfügbaren Speicher gleichmäßig zwischen der Quell- und der Ziel-App aufzuteilen, die die Datei verarbeitet. Quell-Apps können die Eigenschaft [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) verwenden. Hiermit geben sie dem Betriebssystem an, mehr oder weniger des verfügbaren Speicherplatzes für ihr App-Fenster zu verwenden. **DesiredRemainingView** kann auch verwendet werden, um anzugeben, dass die Quell-App nach dem Start der Datei nicht auf dem Bildschirm verbleiben muss und vollständig durch die Ziel-App ersetzt werden kann. Mit dieser Eigenschaft wird nur die bevorzugte Fenstergröße der aufrufenden App angegeben. Es wird nicht das Verhalten anderer Apps angegeben, die ggf. zur gleichen Zeit auf dem Bildschirm angezeigt werden.

> [!NOTE]
> Windows berücksichtigt mehrere verschiedene Faktoren, wenn es der quellanwendung abschließende Fenstergröße, z. B. ermittelt, die Einstellung der Quell-app, die Anzahl der apps auf dem Bildschirm, bildschirmausrichtung und So weiter. Das Festlegen von [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) garantiert kein bestimmtes Fensterverhalten für die Quell-App.

**Mobile-Gerätefamilie:  **[**LauncherOptions.DesiredRemainingView** ](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) für mobile Geräte wird nicht unterstützt.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>Hinweise

Die gestartete App kann nicht von Ihrer App ausgewählt werden. Der Benutzer entscheidet, welche App gestartet wird. Der Benutzer kann eine App für die UWP (Universelle Windows-Plattform) oder eine Windows-Desktop-App auswählen.

Beim Starten einer Datei muss Ihre App im Vordergrund ausgeführt werden, d. h. sie muss für den Benutzer sichtbar sein. Durch diese Anforderung wird sichergestellt, dass der Benutzer zu jedem Zeitpunkt die Kontrolle behält. Verknüpfen Sie alle Dateistartvorgänge direkt mit der Benutzeroberfläche Ihrer App, um sicherzustellen, dass diese Anforderung erfüllt wird. Benutzer müssen wahrscheinlich immer eine Aktion ausführen, bevor eine Datei gestartet werden kann.

Dateitypen mit Code oder Skripts, die automatisch vom Betriebssystem ausgeführt werden (z. B. EXE-, MSI- oder JS-Dateien), können nicht gestartet werden. Diese Einschränkung schützt Benutzer vor der Ausführung von schädlichen Dateien, durch die das Betriebssystem verändert werden könnte. Mit dieser Methode können Sie Dateitypen mit Skripts starten, wenn das Skript dabei von der ausgeführten App isoliert wird, z. B. DOCX-Dateien. Apps wie Microsoft Word verhindern, dass das Betriebssystem durch DOCX-Dateien modifiziert wird.

Wenn Sie versuchen, einen eingeschränkten Dateityp zu starten, schlägt der Start fehl, und Ihr Fehlerrückruf wird aufgerufen. Wenn Ihre App viele verschiedene Dateitypen behandelt und Sie erwarten, dass dieser Fehler auftritt, sollten Sie Benutzern eine Fallbackoption zur Verfügung stellen. Bieten Sie Benutzern z. B. die Möglichkeit, die Datei auf dem Desktop zu speichern und zu öffnen.

## <a name="related-topics"></a>Verwandte Themen

### <a name="tasks"></a>Richtlinienübersicht

* [Starten der Standard-App für einen URI](launch-default-app.md)
* [Behandeln der Dateiaktivierung](handle-file-activation.md)

### <a name="guidelines"></a>Richtlinien

* [Richtlinien für die Dateitypen und URIs](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Referenz

* [**Windows.Storage.StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
* [**Windows.System.Launcher.LaunchFileAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync)
