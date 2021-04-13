---
title: IWindowNative.WindowHandle-Eigenschaft
description: WinUI-COM-Eigenschaft zum Abrufen des angeforderten Fenster-HWND.
ms.topic: reference
ms.date: 03/09/2021
keywords: WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: 9c8278b5b19f4e7eeee447ed4a4261064f36194d
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730668"
---
# <a name="iwindownativewindowhandle-property-microsoftuixamlwindowh"></a>IWindowNative.WindowHandle-Eigenschaft (microsoft.ui.xaml.window.h)

Ruft das angeforderte Handle für das Fenster ab.

## <a name="syntax"></a>Syntax

<!--
[
    object,
    uuid( EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB ),
    local,
    pointer_default(unique)
]
interface IWindowNative: IUnknown
{
    [propget] HRESULT WindowHandle([out, retval] HWND* hWnd);
};
-->

```cpp
HRESULT WindowHandle(
  HWND* hWnd
);
```

## <a name="parameters"></a>Parameter

*hWnd* [out]

Typ: **HWND***

Handle zum Fenster.

## <a name="return-value"></a>Rückgabewert

Typ: HRESULT

Wenn diese Methode erfolgreich ist, wird S_OK zurückgegeben. Andernfalls wird ein HRESULT-Fehlercode zurückgegeben.

## <a name="remarks"></a>Bemerkungen

## <a name="examples"></a>Beispiele

Bevor Sie das folgende Beispiel ausprobieren, lesen Sie die folgenden Themen:

- Um WinUI 3 für Desktopprojektvorlagen zu verwenden, konfigurieren Sie den Entwicklungscomputer, und [richten Sie Ihre Entwicklungsumgebung ein](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment).
- Vergewissern Sie sich, dass Ihre Entwicklungsumgebung erwartungsgemäß funktioniert, indem Sie eine erste Vorlagen-App erstellen und ausführen, wie unter [Erste Schritte mit WinUI 3 für Desktop-Apps](../winui3/get-started-winui3-for-desktop.md) beschrieben.

### <a name="customized-window-icon"></a>Angepasstes Fenstersymbol

Im folgenden Beispiel beginnen wir mit dem ersten **WinUI in Desktop C#/.NET 5**-Vorlagencode und zeigen, wie das für ein App-Fenster verwendete Symbol mithilfe eines **WindowHandle**-Objekts angepasst wird.

#### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

1. Zuerst fügen Sie die folgenden using-Direktiven hinzu:

    - [System.Runtime.InteropServices](/dotnet/api/system.runtime.interopservices): Bietet Unterstützung für COM-Interop- und Plattformaufrufdienste. In diesem Beispiel für die PInvoke-Funktion erforderlich.
    - [WinRT](/uwp/cpp-ref-for-winrt/winrt): Stellt benutzerdefinierte Datentypen bereit, die zu C++/WinRT gehören. In diesem Beispiel für die IWindowNative-COM-Schnittstelle erforderlich.

    ```csharp
    using System.Runtime.InteropServices;
    using WinRT;
    ```

1. Anschließend wird eine ICO-Datei zu dem Projekt hinzugefügt („Images/windowIcon.ico“) und die „Build-Aktion“ (mit der rechten Maustaste auf die Datei klicken und „Eigenschaften“ auswählen) für diese Datei auf „Inhalt“ festgelegt.

1. In der MainWindow-Methode wird ein Aufruf zur `LoadIcon("Images/windowIcon.ico");`-Funktion (im nächsten Schritt beschrieben) mit einem Verweis auf die im vorherigen Schritt hinzugefügte ICO-Datei hinzugefügt.

    ```csharp
    public MainWindow()
    {
        LoadIcon("Images/windowIcon.ico");
    
        this.InitializeComponent();
    }
    ```

1. Als Nächstes wird eine `LoadIcon(string iconName)`-Funktion hinzugefügt, die ein Handle für die Anwendung abruft und verschiedene [PInvoke](/dotnet/standard/native-interop/pinvoke)-Features (z. B. [LoadImage](/windows/win32/api/winuser/nf-winuser-loadimagew) und [SendMessage](/windows/win32/api/winuser/nf-winuser-sendmessage)) verwendet, um das Anwendungssymbol festzulegen.

    ```csharp
    private void LoadIcon(string iconName)
    {
        //Get the Window's HWND
        var hwnd = this.As<IWindowNative>().WindowHandle;
    
        IntPtr hIcon = PInvoke.User32.LoadImage(
            IntPtr.Zero, 
            iconName,
            PInvoke.User32.ImageType.IMAGE_ICON, 
            16, 16, 
            PInvoke.User32.LoadImageFlags.LR_LOADFROMFILE);
    
        PInvoke.User32.SendMessage(
            hwnd, 
            PInvoke.User32.WindowMessage.WM_SETICON, 
            (IntPtr)0, 
            hIcon);
    }    
    ```

1. Schließlich werden die Typinformationen aus der öffentlichen IWindowNative-Schnittstelle eingebettet und eine Instanz der Klasse aus der Laufzeitassembly erstellt. Weitere Informationen finden Sie unter [Einbetten von Typen aus verwalteten Assemblys](/dotnet/standard/assembly/embed-types-visual-studio).

    ```csharp
    [ComImport]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB")]
    internal interface IWindowNative
    {
        IntPtr WindowHandle { get; }
    }
    ```

1. Wenn Sie diese Schritte in ihrer eigenen App befolgt haben, erstellen Sie die App, und führen Sie sie aus. Es sollte ein Anwendungsfenster ähnlich dem folgenden angezeigt werden (mit dem benutzerdefinierten App-Symbol):

    :::image type="content" source="../winui3/images/build-basic/template-app-windowhandle.png" alt-text="Vorlagen-App mit benutzerdefiniertem Anwendungssymbol.":::<br/>*Vorlagen-App mit benutzerdefiniertem Anwendungssymbol.*

## <a name="applies-to"></a>Gilt für:

| „Product“ (Produkt) | Versionen |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5, 3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>Weitere Informationen

[IWindowNative-Schnittstelle](iwindownative.md)
