---
description: Dieses Thema bietet eine umfassende Zuordnung von Windows Phone Silverlight-APIs zu ihren universelle Windows-Plattform Entsprechungen (UWP).
title: WPSL-zu-UWP-Namespace und Klassen Zuordnungen
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fdb1dc8ad4b4e61e1ffec294cfbf17e8abcc8586
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735055"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone von Silverlight zu UWP-API-Zuordnungen


Dieses Thema bietet eine umfassende Zuordnung von Windows Phone Silverlight-APIs zu ihren universelle Windows-Plattform Entsprechungen (UWP). Im Allgemeinen erfolgt keine 1: 1-Zuordnung von Funktionen, jedoch gilt: Jede Plattform kann ggf. mehr oder weniger Funktionalität bieten als ihr Gegenstück in einem Namespace oder einer Klasse.

Die Mapping-Tabelle hilft Ihnen bei der Arbeit in einem UWP-Projekt, und Sie verwenden Quellcode aus einem Windows Phone Silverlight-Projekt wieder. Zwischen den beiden Plattformen gibt es Unterschiede bei den Namen von Namespaces und Klassen (einschließlich UI-Steuerelemente). In vielen Fällen ist es einfach: Sie ändern z. B. einen Namespacenamen, und der Code wird kompiliert Manchmal wird neben dem Namespacenamen auch der Name einer Klasse oder API geändert In anderen Fällen ist die Zuordnung etwas schwieriger, und in seltenen Fällen muss der Ansatz geändert werden.

**Verwenden der Tabelle:  ** Suchen Sie zuerst nach dem Namen der Klasse, die Sie verwenden. Klassen werden aufgelistet, wenn es sich um eine kompliziertere Zuordnung als eine Änderung des Namespacenamens handelt. Wenn Ihre Klasse nicht aufgeführt ist, handelt es sich bei der Zuordnung nur um eine Namespaceänderung. Wenn Sie den Namespacenamen Ihrer Klasse finden, finden Sie auch den entsprechenden Namen des UWP-Namespaces. Ihre Klasse ist in diesem Namespace enthalten. Wenn der Namespace nicht aufgeführt ist, hat sich dessen Name nicht geändert.

**Beachten  Sie** , dass Windows 10 viel mehr .NET Framework als eine Windows Phone Store-App unterstützt. Windows 10 hat z. b. mehrere System. Service Model.\* Namespaces sowie System.net, System .net. NetworkInformation und System .net. Sockets.
Außerdem profitieren Sie in einer Windows 10-APP von .net Native, bei der es sich um eine vorzeitliche Kompilierungs Technologie handelt, die MSIL in nativ ausführbaren Computercode konvertiert. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

| Windows Phone von Silverlight | Windows-Runtime |
| ------------------------- | --------------- |
| Werbung | |
| **Microsoft.Advertising.Mobile.UI.AdControl**-Klasse | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries)-Klasse |
| Alarme, Erinnerungen und Hintergrund-Agents | |
| **Microsoft.Phone.BackgroundAgent**-Klasse | [**Backgroundtaskbuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse |
| **Microsoft.Phone.Scheduler** Namespace | [**Windows. applicationmodel. Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) -Namespace |
| **Microsoft.Phone.Scheduler.Alarm** Klasse | [**Backgroundtaskbuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse und die Klasse " [**deastnotificationmanager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) " |
| **Microsoft.Phone.Scheduler.PeriodicTask**-, **ScheduledAction**-, **ScheduledActionService**-, **ScheduledTask**- und **ScheduledTaskAgent**-Klassen | [**Backgroundtaskbuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse |
| **Microsoft.Phone.Scheduler.Reminder** Klasse | [**Backgroundtaskbuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse und die Klasse " [**deastnotificationmanager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) " |
| **Microsoft.Phone.PictureDecoder** Klasse | [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) -Klasse |
| **Microsoft.Phone.BackgroundAudio** Namespace | [**Windows. Media. Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) -Namespace |
| **Microsoft.Phone.BackgroundTransfer**-Namespace | [**Windows. Networking. backgroundtransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) -Namespace |
| App-Modell und -Umgebung | |
| **System.AppDomain** Klasse | Keine direkte Entsprechung Siehe [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), Klassen |
| **System.Environment** Klasse | Keine direkte Entsprechung |
| **System.ComponentModel.Annotations** Klasse  | Keine direkte Entsprechung |
| **System.ComponentModel.BackgroundWorker** Klasse | [**Thread Pool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) -Klasse |
| **System.ComponentModel.DesignerProperties** Klasse | [**Design Mode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) -Klasse |
| **System.Threading.Thread**, **System.Threading.ThreadPool** Klassen | [**Thread Pool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) -Klasse |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** Methode | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** Methode |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** Eigenschaft | (S = **System**) <br/> **S.Environment.ManagedThreadId** Eigenschaft |
| **System.Threading.Timer** Klasse | [**Threadpooltimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) -Klasse |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** Klasse | [**Coredispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) -Klasse |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** Klasse | [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) -Klasse |
| Blend für Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity** Namespace | [Microsoft.Xaml.Interactivity](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactivity.aspx) Namespace |
| **Microsoft.Expression.Interactivity.Core** Namespace | [Microsoft.Xaml.Interactions.Core](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.core.aspx) Namespace |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Input** Namespace | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Media** Namespace | [Microsoft.Xaml.Interactions.Media](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.media.aspx) Namespace |
| **Microsoft.Expression.Shapes** Namespace | Keine direkte Entsprechung |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** Schnittstelle | Keine direkte Entsprechung |
| Kontakt- und Kalenderdaten | |
| **Microsoft.Phone.UserData** Namespace | [**Windows. applicationmodel. Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows. applicationmodel. Termine**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) -Namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**-, **ContactAddress**-, **ContactCompanyInformation**-, **ContactEmailAddress**-, **ContactPhoneNumber** Klassen | [**Contact**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) -Klasse |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** Klasse | Klasse " [**Terminkalender**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) " |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** Klasse | [**Contactstore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) -Klasse |
| Steuerelemente und UI-Infrastruktur | |
| **ControlTiltEffect.TiltEffect** Klasse | Animationen aus der Windows-Runtime-Animationsbibliothek sind in die Standardstile der allgemeinen Steuerelemente integriert. Siehe [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **Microsoft.Phone.Controls** Namespace | [**Windows. UI. XAML. Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) -Namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** Klasse | [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** Klasse | [**Datepickerflyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** Klasse | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** Klasse | [**Semanticzoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** Klasse | [**Systemprotection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection)-, [**windowactivatedeventargs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) -Klassen |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** Klasse | [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** Klassen | [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** Klasse | [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** Klasse | [**Pointerdownpoinmeanimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** Klasse | [**Timepickerflyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** Klasse | [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) -Klasse |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** Klasse | Keine direkte Entsprechung |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**Itemtaurapgrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) und [**wrapgrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) können in der Element Panel-Vorlage eines Items-Steuer Elements verwendet werden. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** Namespace | Keine direkte Entsprechung |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** Namespace | Keine direkte Entsprechung |
| **Microsoft.Phone.Globalization** Namespace | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**, **DeviceStatus** Klassen | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)-, [**memorymanager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) -Klassen. Weitere Infos finden Sie unter [Gerätestatus](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** Klasse | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** Klasse | [**Werbung**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) |
| **System.Windows** Namespace | [**Windows. UI. XAML**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) -Namespace |
| **System.Windows.Automation** Namespace | [**Windows. UI. XAML. Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) -Namespace |
| **System.Windows.Controls**, **System.Windows.Input** Namespaces | [**Windows. UI. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows. UI. XAML. Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) -Namespaces |
| **System.Windows.Controls.DrawingSurface**-, **DrawingSurfaceBackgroundGrid** Klassen | Klasse " [**Swap-chainpanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) " |
| **System.Windows.Controls.RichTextBox** Klasse | [**Richeditbox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) -Klasse |
| **System.Windows.Controls.WrapPanel** Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**Itemtaurapgrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) und [**wrapgrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) können in der Element Panel-Vorlage eines Items-Steuer Elements verwendet werden. |
| **System.Windows.Controls.Primitives** Namespace | [**Windows. UI. XAML. Controls. Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) -Namespace |
| **System.Windows.Controls.Shapes** Namespace | [**Windows. UI. XAML. Controls. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) -Namespace |
| **System.Windows.Data** Namespace | [**Windows. UI. XAML. Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) -Namespace |
| **System.Windows.Documents** Namespace | [**Windows. UI. XAML. Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) -Namespace |
| **System.Windows.Ink** Namespace | Keine direkte Entsprechung |
| **System.Windows.Markup** Namespace | [**Windows. UI. XAML. Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) -Namespace | 
| **System.Windows.Navigation** Namespace | [**Windows. UI. XAML. Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) -Namespace |
| **System.Windows.UIElement.Tap** Ereignis, **EventHandler&lt;GestureEventArgs&gt;** Delegat | [**Gezapftes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) Ereignis, [**tappedeventhandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) -Delegat |
| Daten und Dienste |  |
| **System.Data.Linq.DataContext** Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.Mapping.ColumnAttribute** Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.SqlClient.SqlHelpers** Klasse | Keine direkte Entsprechung |
| Geräte | |
| **Microsoft.Devices**, **Microsoft.Devices.Sensors** Namespaces | [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows. Devices. Enumeration. PNP**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows. Devices. Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) -Namespaces |
| **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** Klassen | [**Mediacapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse. Auch [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft.Devices.CameraButtons** Klasse | [**Hardwarebuttons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) -Klasse |
| **Microsoft.Devices.CameraVideoBrushExtensions** Klasse | [**Captureelement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) -Klasse |
| **Microsoft.Devices.Environment** Klasse | Keine direkte Entsprechung Um dieses Problem zu umgehen, verwenden Sie die bedingte Kompilierung und definieren Sie ein benutzerdefiniertes Symbol. Unter Umständen können Sie das Problem auch mit der [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) Eigenschaft umgehen. |
| **Microsoft.Devices.MediaHistory** Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.VibrateController** Klasse | [**Vibrationdevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) -Klasse |
| **Microsoft.Devices.Radio.FMRadio** Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.Sensors.Accelerometer**, **Compass** Klassen | Im [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)-Namespace |
| **Microsoft.Devices.Sensors.Gyroscope** Klasse | [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) -Klasse |
| **Microsoft.Devices.Sensors.Motion** Klasse | [**Neigungsmesser**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) Klasse |
| Globalization | |
| **System.Globalization** Namespace | [**Windows. Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) -Namespace |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** Eigenschaft | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** Eigenschaft |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** Eigenschaft | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** Eigenschaft |
| Grafiken und Animationen | |
| **Microsoft. XNA. Framework.\*** -Namespaces, [XNA-Framework-Klassenbibliothek](https://msdn.microsoft.com/library/bb203940.aspx), [Inhalts Pipeline-Klassenbibliothek](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx) | Keine direkte Entsprechung Verwenden Sie im Allgemeinen [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) mit C++. Siehe [Entwickeln von Spielen](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) und [Interoperabilität von DirectX und XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| **Microsoft.Xna.Framework.Audio.Microphone** Klasse | [**Mediacapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse |
| **Microsoft.Xna.Framework.Audio.SoundEffect** Klasse | [**Media Element**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) -Klasse |
| **Microsoft.Xna.Framework.GamerServices** Namespace | (WPS = **Windows.Phone.System**) <br/> [**WPS. Userprofile. gameservices. Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) -Namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide** Klasse | Keine direkte Entsprechung |
| **Microsoft.Xna.Framework.Input.GamePad** Klasse | [**Hardwarebuttons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) -Klasse |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** Klasse | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) -Klasse |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**-, **MXFM.PhoneExtensions.MediaLibraryExtensions** Klassen | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) -Klasse |
| **Microsoft.Xna.Framework.Media.MediaQueue** Klasse | [**Systemmediatransportcontrols**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) -Klasse |
| **Microsoft.Xna.Framework.Media.Playlist** Klasse | [**Backgroundmediaplayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) -Klasse |
| **System.Windows.Media** Namespace | [**Windows. UI. XAML. Media**](/uwp/api/Windows.UI.Xaml.Media) -Namespace |
| **System.Windows.Media.RadialGradientBrush** Klasse | Keine direkte Entsprechung Siehe [Medien und Grafiken](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **System.Windows.Media.Animation** Namespace | [**Windows. UI. XAML. Media. Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) -Namespace |
| **System.Windows.Media.Effects** Namespace | Keine direkte Entsprechung |
| **System.Windows.Media.Imaging** Namespace | [**Windows. UI. XAML. Media. Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) -Namespace |
| **System.Windows.Media.Media3D** Namespace | [**Windows. UI. XAML. Media. Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) -Namespace |
| **System.Windows.Shapes** Namespace | [**Windows. UI. XAML. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) -Namespace |
| Launcher und Chooser | |
| **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** Klassen | [**Contactpicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) -Klasse |
| **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** Klassen | [**Windows. applicationmodel. Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) -Namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.CameraCaptureTask** Klasse | [**Mediacapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse. Auch [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft. Phone. Tasks. marketplacedetailtask** | [**Currentapp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse ([**requestapppurchaseasync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) -Methode) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**-, **MarketplaceHubTask**-, **MarketplaceReviewTask**-, **MarketplaceSearchTask**-, **MediaPlayerLauncher**-, **SearchTask**-, **SmsComposeTask**-, **WebBrowserTask** Klassen | Start [**Programm Klasse**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) |
| **Microsoft.Phone.Tasks.EmailComposeTask** Klasse | [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) -Klasse |
| **Microsoft.Phone.Tasks.GameInviteTask** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.PhoneCallTask** Klasse | [**Phonecallmanager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) -Klasse |
| **Microsoft.Phone.Tasks.PhotoChooserTask** Klasse | [**Fileopenpicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) -Klasse |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** Klasse | Termin [**Manager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) -Klasse |
| **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** Klassen | [**Storedcontact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) -Klasse (nur Windows Phone) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** Klassen | [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) -Klasse |
| Pfad | |
| **System.Device.Location** Namespace | [**Windows. Devices. Geolokation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) -Namespace |
| **System.Device.GeoCoordinateWatcher** Klasse | [**GeoLocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) -Klasse |
| Karten | |
| **Microsoft.Phone.Maps** Namespaces | [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) -Namespace |
| **Microsoft.Phone.Maps.Controls** Namespace | [**Windows. UI. XAML. Controls. Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) -Namespace |
| **Microsoft.Phone.Maps.Controls.Map** Klasse | [**Mapcontrol**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) -Klasse |
| **Microsoft.Phone.Maps.Services** Namespace | [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) -Namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** Klassen | [**Maplocationfinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse |
| **System.Device.Location.GeoCoordinate** Klasse | [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) -Klasse |
| **Microsoft.Phone.Maps.Services.Route** Klasse | [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) -Klasse |
| **Microsoft.Phone.Maps.Services.RouteQuery** Klasse | [**Maproutefinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) -Klasse |
| Monetisierung | |
| **Microsoft.Phone.Marketplace** Namespace | [**Windows. applicationmodel. Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) -Namespace |
| Media | |
| **Microsoft.Phone.Media** Namespace | [**Media Element**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) -Klasse |
| -Netzwerk | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** Klasse | [**Hostname**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) -Klassen |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** Klasse | [**Network Information**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) -Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** Klasse | [**Connectionprofile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) -Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** Klasse | [**Network Information**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) -Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** Klasse | Keine direkte Entsprechung |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Networking.Voip** Namespace | Keine direkte Entsprechung |
| **System.Net.CookieCollection** Klasse | Wird noch unterstützt, aber einige Eigenschaften fehlen (z. B. IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ableitung von [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) zum Messen des Fortschritts |
| **System.Net.DnsEndPoint**-, **IPAddress** Klassen | Diese Klassen werden zwar noch unterstützt, aber einige Eigenschaften fehlen. Alternativ dazu ist das Portieren zur [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName)-Klasse möglich. |
| **System.Net.HttpUtility** Klasse | [**Htmlformathelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) -Klasse |
| **System.Net.HttpWebRequest** Klasse | Wird teilweise unterstützt, aber die empfohlene fortschrittliche Alternative ist die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.HttpWebResponse** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** Klasse | Wird noch unterstützt, mit Ausnahme des Konstruktors |
| **System.Net.OpenReadCompletedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Alternativ dazu ist das Portieren zur[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)-Klasse möglich. |
| **System.Net.Sockets.SocketException** Klasse | Wird zwar noch unterstützt, aber anstelle von ErrorCode wird die SocketErrorCode-Eigenschaft verwendet. |
| **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** Klassen | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) -Klasse |
| **System.Net.UploadProgressChangedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** Klasse | Wird teilweise unterstützt (anderer Eigenschaftensatz), aber die empfohlene modernere Alternative ist die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.WebResponse** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** Klasse | **System.FormatException** Klasse |
| Benachrichtigungen | |
| MPN = **Microsoft.Phone.Notification**-Namespace | [**Windows. UI. Benachrichtigungen**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows. Networking. pushnotification**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) -Namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** Klasse | [**Tilenotifizierungsklasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** Klasse | [**Pushnotificationchannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) -Klasse |
| Programmierung | |
| **System** Namespace | [**Windows. Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) -Namespace |
| **System.Diagnostics.StackFrame**, **StackTrace** Klassen | Keine direkte Entsprechung |
| **System.Diagnostics** Namespace | [**Windows. Foundation. Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) -Namespace |
| **System.ICloneable** Schnittstelle | Eine benutzerdefinierte Methode, mit der der passende Typ zurückgegeben wird. |
| **System.Reflection.Emit.ILGenerator** Klasse | Keine direkte Entsprechung |
| Reaktive Erweiterungen | |
| **Microsoft.Phone.Reactive** Namespace | Keine direkte Entsprechung |
| Reflektion | |
| **System.Type** Klasse | **System.Reflection.TypeInfo** Klasse. Siehe [Reflektion in .NET Framework für UWP-Apps](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Ressourcen | |
| **System.Resources.ResourceManager** Klasse | (WA = **Windows.ApplicationModel**)<br/>[**WA. Resources. Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) und [**WA. Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) -Namespaces, [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) -Klasse. Siehe [Erstellen und Abrufen von Ressourcen in Windows-Runtime-Apps](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Sicheres Element | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**, **MPS.SecureElementSession** Klassen | [**Smartcardconnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) -Klasse |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** Klasse | [**Smartcardreader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) -Klasse |
| Security | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**, **SSC.RSA** Klassen | [**Cryptographicengine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) -Klasse |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**, **SSC.SHA256** Klassen | [**Hashalgorithmprovider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) -Klasse |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** Klasse | [**Dataschutzprovider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) -Klasse |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** Klasse | [**Cryptographicbuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) -Klasse |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** Klasse | [**Certificateregistrimentmanager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) -Klasse |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** Klasse | [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** Klasse | [**Appbarbutton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) -Klasse (bei Verwendung innerhalb der [**primarycommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) -Eigenschaft) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** Klasse | [**Appbarbutton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) -Klasse (bei Verwendung innerhalb der [**secondarycommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) -Eigenschaft) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** Klassen | [**Tiletemplatetype**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** Klasse | [**Coreapplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication)-, [**displayrequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) -Klassen |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** Klasse | [**Status barprogressindicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** Klasse | [**Secondarytile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** Klasse | [**Tileupdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** Klasse | " [ **-Klasse"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) -Klasse |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** Klasse | [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) -Klasse |
| Speicher und E/A | |
| **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** Klassen | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) -Klasse |
| **System.IO** Namespace | [**Windows. Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows. Storage. Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) -Namespaces |
| **System.IO.Directory** Klasse | [**Storagefolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) -Klasse |
| **System.IO.File** Klasse | [**Storagefile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) -und [**pthio**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) -Klassen
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** Klasse |[**ApplicationData. localfolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) (Eigenschaft) |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** Klasse | [**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) (Eigenschaft) |
| **System.IO.Stream** Klasse | Wird noch unterstützt, aber anstelle von BeginRead()/EndRead() und BeginWrite()/EndWrite() wird ReadAsync() und WriteAsync() verwendet. |
| Brieftasche | |
| **Microsoft.Phone.Wallet** Namespace | [**Windows. applicationmodel. Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) -Namespace |
| XML | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** Methode |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** Methode |


Nächstes Thema: [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md).

