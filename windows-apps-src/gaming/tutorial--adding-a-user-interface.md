---
title: Hinzufügen einer Benutzeroberfläche
description: Erfahren Sie, wie ein DirectX UWP-Spiel eine 2D Benutzer Schnittstelle Überlagerung hinzugefügt.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Benutzeroberfläche, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609055"
---
# <a name="add-a-user-interface"></a>Hinzufügen einer Benutzeroberfläche


Nachdem unser Spiel 3D seine visuellen Elemente verfügt, ist es Zeit, um zum Hinzufügen von 2D Benutzeroberflächenelemente, damit das Spiel für den Player Feedback über Spielzustands erhalten konzentrieren. Dies kann erreicht werden, indem Sie einfach im Menüoptionen hinzufügen und Heads-Up-Display-Komponenten über die 3D-Grafiken pipeline Ausgabe.

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Ziel

Fügen Sie eine Reihe von Benutzer-Schnittstelle Grafiken und Verhalten mit Direct2D zu unserer UWP DirectX-Spiele-einschließlich:
- Heads-Up anzuzeigen, einschließlich [verschieben aussehen Controller](tutorial--adding-controls.md) ist Rechtecke
- Spielzustands-Menüs


## <a name="the-user-interface-overlay"></a>Benutzeroberflächenoverlay


Es gibt viele Möglichkeiten zum Anzeigen von Text und Elemente der Benutzeroberfläche in einer DirectX-Spielen, wir werden den Fokus zur Verwendung von [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx). Wir auch verwenden [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) für die Textelemente.


Direct2D ist, dass eine Reihe von 2D Zeichnungs-APIs verwendet wird, um primitive pixelbasierter und Effekte gezeichnet werden soll. Beim Starten Sie mit Direct2D empfiehlt es sich um Dinge einfach zu halten. Komplexe Layouts und Benutzeroberflächenverhalten erfordern Zeit und Planung. Wenn Ihr Spiel eine komplexen Benutzeroberfläche, wie in der Simulation und Strategiespiele, erfordert erwägen Sie stattdessen mit XAML.

> [!NOTE]
> Weitere Informationen über das Entwickeln einer Benutzeroberfläche mit XAML in eine UWP-DirectX-Spielen, finden Sie unter [erweitert das Beispiel](tutorial-resources.md).

Direct2D ist nicht speziell für Benutzeroberflächen oder Layouts wie HTML und XAML. Es stellt keine Benutzeroberflächenkomponenten wie Listen, Textfelder und Schaltflächen bereit. Es stellt auch Layoutkomponenten wie div-Tags, Tabellen oder Rastern bereit.


Für dieses Beispiel haben wir zwei wesentliche Komponenten der Benutzeroberfläche.
1. Ein Heads-Up-Display für die Bewertung und die in-Game-Steuerelemente.
2. Overlay verwendet wird, um Spielzustands Text und Optionen wie z. B. Anhalten Informationen anzuzeigen, und Ebene Startoptionen.

### <a name="using-direct2d-for-a-heads-up-display"></a>Verwenden von Direct2D für die Heads-Up-Anzeige

Die folgende Abbildung zeigt die in-Game-Heads-Up-Anzeige für das Beispiel. Es ist einfach und übersichtlich ist, sodass der Player Navigieren in der 3D-Welt und Schießen Ziele konzentrieren. Eine gute Schnittstelle oder ein Heads-Up-Display müssen Sie nie die Möglichkeit des Spielers zu verarbeiten und die Reaktion auf die Ereignisse im Spiel erschweren.

![Screenshot des Spieloverlays](images/simple-dx-game-ui-overlay.png)

Das Overlay umfasst die folgenden grundlegenden primitiven.
- [**DirectWrite** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) Text in der oberen rechten Ecke, die vom Spieler zu informieren 
    - Erfolgreiche Treffer
    - Anzahl von Screenshots, die der Spieler vorgenommen wurde
    - Auf der Ebene verbleibende Zeit
    - Die Nummer der aktuellen Ebene 
- Zwei Liniensegmente verwendet, um ein Fadenkreuz bilden überschneidenden
- Zwei Rechtecke an den unteren Ecken für die [verschieben aussehen Controller](tutorial--adding-controls.md) Grenzen. 


Der in-Game-Heads-Up-Display-Status der Überlagerung wird gezeichnet, der [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) Methode der [ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) Klasse. Innerhalb dieser Methode ist das Direct2D-Overlay, das unserer Benutzeroberfläche darstellt. die Änderungen in der Anzahl von Treffern, Zeitpunkt verbleibenden und Ebenen Anzahl entsprechend aktualisiert.

Wenn das Spiel initialisiert wurde, fügen wir `TotalHits()`, `TotalShots()`, und `TimeRemaining()` auf eine [ **Swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) gepuffert, und geben Sie das Drucken-Format. Wir können dann ziehen Sie es mithilfe der [ **DrawText** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) Methode. Dasselbe gilt für den Indikator des aktuellen Ebene, Zeichnen von leeren Zahlen nicht abgeschlossene Ebenen wie ➀ anzeigen und gefüllte Zahlen wie ➊ an, dass die bestimmte Zugriffsebene abgeschlossen wurde.


Der folgende Codeausschnitt durchläuft die **GameHud::Render** Methode wie 
- Erstellen einer Bitmap mit [** ID2D1RenderTarget::DrawBitmap **](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- In Rechtecke, die mithilfe von UI-Bereiche unterteilen [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- Mithilfe von **DrawText** um Textelemente zu machen.

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

Unterbrechen die Methode nach unten weitere, dieser Teil der [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) Methode zeichnet unseren Umstieg und Auslösen von Rechtecke mit [ **ID2D1RenderTarget::DrawRectangle** ](https://msdn.microsoft.com/library/windows/desktop/dd371902), und einem Fadenkreuz über zwei Aufrufe [ **ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895).

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

In der **GameHud::Render** Methode speichern wir die logische Größe des Fensters "game" in der `windowBounds` Variable. Verwendet die [ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) Methode der **DeviceResources** Klasse. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Abrufen der Größe des Fensters Spiele ist entscheidend für UI-Programmierung. Die Größe des Fensters wird angegeben, in einer Maßeinheit namens DIPs (geräteunabhängigen Pixeln), in denen eine DIP-Adresse als 1/96 Zoll definiert ist. Direct2D skaliert die tatsächlichen Pixel zeichnen Einheiten bei der die Zeichnung werden auf diese Weise mithilfe der Windows-Punkte pro Zoll (DPI). Auf ähnliche Weise, wenn Sie Text mit zeichnen [ **DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), Sie geben Sie DIPs anstelle von Punkten für die Größe der Schriftart an. DIPs werden als Gleitkommazahlen angegeben.

 

### <a name="displaying-game-state-info"></a>Anzeigen von Statusinformationen für Spiele

Neben dem Heads-Up-Display ist das Spiele Beispiel Overlay, das Spiele in sechs verschiedenen Zuständen darstellt. Alle Bundesstaaten auf der Funktion eines großen schwarzen Rechteck primitiven durch den Text für den Player zu lesen. Move aussehen Controller Rechtecke und einem Fadenkreuz werden nicht gezeichnet werden, da sie in diesen Zuständen nicht aktiv sind.

Die Überlagerung wird erstellt, mit der [ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) Klasse, die es uns ermöglicht, welcher Text angezeigt wird, um mit dem Status des Spiels ausrichten auslagern.

![Status und die Aktion der Überlagerung](images/simple-dx-game-ui-finaloverlay.png)

Die Überlagerung wird in zwei Abschnitte unterteilt: **Status** und **Aktion**. Die **Status** Abschnitt wird weiter unterteilt in in **Titel** und **Text** Rechtecken. Die **Aktion** Abschnitt hat nur ein Rechteck. Jedes Rechteck hat einen anderen Zweck.

-   `titleRectangle` enthält den Text an.
-   `bodyRectangle` enthält den Textkörpertext.
-   `actionRectangle` enthält den Text an, der den Player, um eine bestimmte Aktion ausgeführt werden soll.

Das Spiel verfügt über sechs verschiedenen Zuständen, die festgelegt werden können. Der Status des Spiels vermittelt, mit der **Status** Teil der Überlagerung. Die **Status** Rechtecke werden aktualisiert, indem eine Reihe von Methoden, die mit den folgenden Zuständen entspricht.

- Beim Laden
- Erste Startmenü/hohe Bewertung Statistiken
- Ebene beginnen.
- Spiel angehalten
- Spielende
- Spiel gewonnen hat


Die **Aktion** Teil der Überlagerung wird aktualisiert, mit der [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) -Methode, mit der Aktionstext, der auf einen der folgenden festgelegt werden.
- "Tippen, um erneut wiedergeben..."
- "Ebene geladen, bitte warten..."
- "Tippen, um fortzufahren..."
- Keine

> [!NOTE]
> Werden beide Methoden behandelt werden weiter unten in der [darstellt Spielzustands](#representing-game-state) Abschnitt.

Je nachdem, was im Spiel, geht der **Status** und **Aktion** Abschnitt Text-Felder angepasst werden.
Sehen wir uns wie wir zu initialisieren und die Überlagerung für diese sechs verschiedenen Zuständen zu zeichnen.

### <a name="initializing-and-drawing-the-overlay"></a>Initialisieren und Zeichnen des Overlays

Die sechs **Status** Zustände haben Folgendes gemeinsam, sodass die Ressourcen und die Methoden müssen sie sehr ähnlich.
    - Sie alle verwenden ein schwarzes Rechteck in der Mitte des Bildschirms an, wie der Hintergrund.
    - Der Anzeigetext lautet entweder **Titel** oder **Text** Text.
    - Der Text, verwendet die Schriftart Segoe UI und auf das Back-Rechteck gezeichnet wird. 


Das Beispiel verfügt über vier Methoden, die ins Spiel kommen, wenn das Overlay zu erstellen.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
Die [ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) Konstruktor initialisiert das Overlay an, die Bitmap-Oberfläche, die wir zum Anzeigen von Informationen an den Spieler verwenden auf verwalten. Der Konstruktor erhält eine Factory von der [ **ID2D1Device** ](https://msdn.microsoft.com/library/windows/desktop/hh404478) -Objekt übergeben, um, die er zum Erstellen verwendet einer [ **ID2D1DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/hh404479) um das Overlay-Objekt selbst zeichnen kann. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) ist unsere Methode Pinsel erstellen, die zum Zeichnen von Text verwendet wird. Zu diesem Zweck erhalten wir eine [ **ID2D1DeviceContext2** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) Objekt, das ermöglicht das Erstellen und Zeichnen von Geometry-Instanz, sowie Funktionen wie z. B. Freihand- und Farbverlauf mesh rendern. Anschließend erstellen wir eine Reihe von farbigen Pinsel mit [ **ID2D1SolidColorBrush** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) zeichnen Sie die folgenden Elemente der Benutzeroberfläche.
- Schwarzer Pinsel für die Rechteck-Hintergründen
- Weiß Pinsel für Statustext
- Orange Pinsel für Aktionstext

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
Die [ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) Methode legt den DPI-Wert des Fensters. Diese Methode wird aufgerufen, wenn der DPI-Wert geändert und muss angepasst, was erfolgt, wenn das Fenster des Spiele geändert wird. Nach der Aktualisierung der DPI-Wert, ruft diese Methode auch[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) um sicherzustellen, dass erforderlich machen Ressourcen neu erstellt werden jedes Mal, wenn die Größe des Fensters geändert wird.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
Die [ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) Methode ist, in dem alle unsere Zeichnung stattfindet. Im folgenden finden einen Überblick über die Schritte von der Methode.
- Drei Rechtecke werden erstellt, um den Abschnitt aus der UI-Text für die **Titel**, **Text**, und **Aktion** Text.
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- Erstellt eine Bitmap benannte `m_levelBitmap`, wobei der aktuelle DPI-Wert-Konto verwenden **CreateBitmap**.
- `m_levelBitmap` wird festgelegt, wie unsere 2D Render-Ziel mit [ **ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533).
- Die Bitmap wird gelöscht, mit jedem Pixel, die mithilfe von Schwarz vorgenommen [ **ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772).
- [**ID2D1RenderTarget::beginDraw** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) wird aufgerufen, um die Zeichnung zu initiieren. 
- **DrawText** wird aufgerufen, um das Zeichnen des Texts in gespeicherten `m_titleString`, `m_bodyString`, und `m_actionString` im Approperiate Rechteck mithilfe der entsprechenden **ID2D1SolidColorBrush**.
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw) wird aufgerufen, um auf alle Zeichenoperationen beenden `m_levelBitmap`.
- Mit anderen Bitmap erstellt **CreateBitmap** mit dem Namen `m_tooSmallBitmap` verwendet als ausweichlösung, zeigt nur, wenn die Anzeigekonfiguration für das Spiel zu klein ist.
- Wiederholen Sie Vorgang für das Zeichnen auf `m_levelBitmap` für `m_tooSmallBitmap`, diesmal Zeichnen nur auf die Zeichenfolge `Paused` im Text.




Jetzt sind wir müssen sechs Methoden, um den Text der unsere sechs Overlay-Status zu füllen.

### <a name="representing-game-state"></a>Spielzustands darstellt


Jeder der sechs Overlay Zustände im Spiel verfügt über eine entsprechende Methode in der **GameInfoOverlay** Objekt. Diese Methoden zeichnen eine Variante des Overlays, um dem Spieler explizite Informationen zum Spiel selbst mitzuteilen. Diese Kommunikation wird dargestellt, mit einem **Titel** und **Text** Zeichenfolge. Da das Beispiel bereits die Ressourcen und das Layout dieser Informationen so konfiguriert, als er initialisiert wurde und mit der [ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) -Methode, muss er nur bereitstellen die Überlagerung Zustandsspezifische-Zeichenfolgen.

Die **Status** Teil der Überlagerung durch einen Aufruf einer der folgenden Methoden festgelegt ist.

Spielzustands | Status set-Methode | Statusfelder
:----- | :------- | :---------
Beim Laden | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Titel**</br>Das Laden von Ressourcen </br>**Text**</br> Inkrementell gibt "." zum Laden von Aktivität beinhalten.
Erste Startmenü/hohe Bewertung Statistiken | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Titel**</br>Höchste Punktzahl</br> **Text**</br> Ebenen abgeschlossen # </br>Insgesamt zeigt #</br>Insgesamt Bildschirmdarstellung #
Ebene beginnen. | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Titel**</br>Ebene #</br>**Text**</br>Beschreibung der Dienstebenen SLO.
Spiel angehalten | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Titel**</br>Spiel angehalten</br>**Text**</br>Keine
Spielende | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titel**</br>Spielende</br> **Text**</br> Ebenen abgeschlossen # </br>Insgesamt zeigt #</br>Insgesamt Bildschirmdarstellung #</br>Ebenen abgeschlossen #</br>Hohe Bewertung #
Spiel gewonnen hat | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titel**</br>Sie haben gewonnen!</br> **Text**</br> Ebenen abgeschlossen # </br>Insgesamt zeigt #</br>Insgesamt Bildschirmdarstellung #</br>Ebenen abgeschlossen #</br>Hohe Bewertung #




Mit der [ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) -Methode des Beispiels deklariert drei rechteckige Bereiche, die an bestimmte Regionen der Überlagerung entsprechen.



Unter Berücksichtigung dieser Bereiche wollen wir nun eine der zustandsspezifischen Methoden (**GameInfoOverlay::SetGameStats**) näher untersuchen und sehen, wie das Overlay gezeichnet wird.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

Mit der Direct2D-Gerätekontext, die die **GameInfoOverlay** Objekt initialisiert wurde, diese Methode füllt die Rechtecken Titel und Text mit Schwarz, die mit der Hintergrundpinsel. Sie zeichnet mit dem weißen Textpinsel den Text für die Zeichenfolge „High Score“ im Titelrechteck und eine Zeichenfolge mit den aktualisierten Spielzuständen im Textkörperrechteck.


Das Rechteck für die Aktion wird aktualisiert, indem ein nachfolgender Aufruf von [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) aus einer Methode auf der **GameMain** Objekt, das das Spiel Statusinformationen erforderlich bereitstellt. durch **GameInfoOverlay::SetAction** um zu bestimmen, die richtige Nachricht an den Spieler, z. B. "Tippen Sie, um den Vorgang fortzusetzen.".

Das Overlay für jeden angegebenen Zustand wird ausgewählt, der [ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) Methode wie folgt:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

Das Spiel weist jetzt eine Möglichkeit, Text-Informationen für den Player basierend auf Spielzustands zu kommunizieren, und wir haben eine Möglichkeit zum wechseln, was Ihnen während des Spiels angezeigt wird.

### <a name="next-steps"></a>Nächste Schritte

Im nächsten Thema zum [Hinzufügen von Steuerungen](tutorial--adding-controls.md) untersuchen wir, wie der Spieler mit dem Beispielspiel interagiert und wie sich der Spielzustand durch Eingaben ändert.



 




