---
title: Hinzufügen einer Benutzeroberfläche
description: Erfahren Sie, wie Sie mithilfe von Direct2D-APIs dem DirectX UWP-Spiel eine Überlagerung der 2D-Benutzeroberfläche mit den Menüs "Heads up" und "Game State" hinzufügen.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Games, User Interface, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 0cc736bf58ea70068713218e0e8fc2e06c87ece5
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939085"
---
# <a name="add-a-user-interface"></a>Hinzufügen einer Benutzeroberfläche

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Nachdem das Spiel seine 3D-Visualisierungen eingerichtet hat, ist es an der Zeit, sich auf das Hinzufügen von 2D-Elementen zu konzentrieren, damit das Spiel dem Player Feedback zum Spielzustand geben kann. Dies kann erreicht werden, indem einfache Menü Optionen und Heads-Up-Anzeige Komponenten oberhalb der 3D-Grafik Pipeline Ausgabe hinzugefügt werden.

>[!Note]
>Wenn Sie den neuesten Spiel Code für dieses Beispiel nicht heruntergeladen haben, besuchen Sie das [Direct3D-Beispiel Spiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel ist Teil einer großen Auflistung von UWP-Funktions Beispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter herunterladen [der UWP-Beispiele von GitHub](../get-started/get-app-samples.md).

## <a name="objective"></a>Ziel

Fügen Sie mit Direct2D dem UWP DirectX-Spiel eine Reihe von Benutzeroberflächen Grafiken und Verhalten hinzu, einschließlich:
- Heads-Up-Anzeige, einschließlich [Move-Look Controller](tutorial--adding-controls.md) BOUNDRY Rechtecke
- Menü "Spielstatus"


## <a name="the-user-interface-overlay"></a>Benutzeroberflächenoverlay


Obwohl es viele Möglichkeiten gibt, Text-und Benutzeroberflächen Elemente in einem DirectX-Spiel anzuzeigen, konzentrieren wir uns auf die Verwendung von [Direct2D](/windows/desktop/Direct2D/direct2d-portal). Wir verwenden für die Textelemente auch [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) .


Direct2D ist ein Satz von 2D-Zeichnungs-APIs, mit denen pixelbasierte primitive und Effekte gezeichnet werden. Beim Einstieg in Direct2D ist es am besten, die Dinge einfach zu halten. Komplexe Layouts und Benutzeroberflächenverhalten erfordern Zeit und Planung. Wenn Ihr Spiel eine komplexe Benutzeroberfläche erfordert, wie z. b. in Simulations-und Strategie spielen, sollten Sie stattdessen XAML verwenden.

> [!NOTE]
> Informationen zum Entwickeln einer Benutzeroberfläche mit XAML in einem UWP DirectX-Spiel finden Sie unter [Erweitern des Beispiel Spiels](tutorial-resources.md).

Direct2D ist nicht speziell für Benutzeroberflächen oder Layouts wie HTML und XAML konzipiert. Sie stellt keine Benutzeroberflächen Komponenten wie Listen, Felder oder Schaltflächen bereit. Außerdem werden keine Layoutkomponenten wie divs, Tabellen oder Raster bereitgestellt.


Für dieses Beispiel Spiel verfügen wir über zwei Hauptkomponenten der Benutzeroberfläche.
1. Ein Heads-Up-Display für das Score-und in-Game-Steuerelement.
2. Ein Overlay, das verwendet wird, um den Text des Spiel Zustands und Optionen wie z. b. Informationen zur Pause und Startoptionen anzuzeigen

### <a name="using-direct2d-for-a-heads-up-display"></a>Verwenden von Direct2D für die Heads-Up-Anzeige

Die folgende Abbildung zeigt die in-Game-Heads-Up-Anzeige für das Beispiel. Es ist einfach und unüberladen und ermöglicht dem Player, sich auf die Navigation in der 3D-Welt und auf das Durcharbeiten der Ziele zu konzentrieren. Eine gute Oberfläche oder ein Heads-Up-Display muss niemals die Fähigkeit des Players erschweren, die Ereignisse im Spiel zu verarbeiten und darauf zu reagieren.

![Screenshot des Spieloverlays](images/simple-dx-game-ui-overlay.png)

Das Overlay besteht aus den folgenden grundlegenden primitiven.
- [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) -Text in der oberen rechten Ecke, der den Player informiert 
    - Erfolgreiche Treffer
    - Anzahl von Fotos, die der Spieler erstellt hat
    - Verbleibende Zeit auf der Ebene
    - Aktuelle Ebene 
- Zwei überschneidende Liniensegmente, die zum bilden von kreuzhaaren verwendet werden
- Zwei Rechtecke in den unteren Ecken für den Verschiebungs [Controller](tutorial--adding-controls.md) . 


Der in-Game-Heads-Up-Anzeige Zustand der Überlagerung wird in der [**gamehud:: Rendering**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) -Methode der [**gamehud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) Class gezeichnet. Innerhalb dieser Methode wird das Direct2D Overlay, das unsere Benutzeroberfläche repräsentiert, aktualisiert, um die Änderungen in der Anzahl der Treffer, der verbleibenden Zeit und der Anzahl der Ebenen widerzuspiegeln.

Wenn das Spiel initialisiert wurde, fügen wir `TotalHits()` , `TotalShots()` und `TimeRemaining()` zu einem [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) Puffer hinzu und geben das Druckformat an. Anschließend können wir ihn mithilfe der [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) -Methode zeichnen. Das gleiche gilt für den Indikator der aktuellen Ebene, das Zeichnen von leeren Zahlen, um nicht abgeschlossene Ebenen wie ➀ anzuzeigen, und gefüllte Zahlen wie ➊, um anzuzeigen, dass die bestimmte Ebene abgeschlossen wurde.


Der folgende Code Ausschnitt durchläuft den Prozess der **gamehud:: Rendering** -Methode für 
- Erstellen einer Bitmap mit [* * ID2D1RenderTarget::D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Unterteilen von Benutzeroberflächen Bereichen mithilfe von [ **D2D1:: RectF** in Rechtecke](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- Verwenden von **DrawText** zum Erstellen von Textelementen

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
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
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

Wenn Sie die Methode weiter unten unterbrechen, zeichnet dieser Teil der [**gamehud:: Rendering**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) -Methode die Verschiebungs-und auslösrechtecke mit [**ID2D1RenderTarget::D rawrechteck**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))und einem Fadenkreuz mithilfe von zwei Aufrufen von [**ID2D1RenderTarget::D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

In der **gamehud:: Rendering** -Methode speichern wir die logische Größe des Spielfensters in der `windowBounds` Variablen. Dabei wird die- [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) Methode der **deviceresources** -Klasse verwendet. 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

Das Abrufen der Größe des Spielfensters ist für die Benutzeroberflächen Programmierung von entscheidender Bedeutung. Die Größe des Fensters wird in einer Maßeinheit namens Dips (geräteunabhängige Pixel) angegeben, bei der eine DIP als 1/96 Zoll definiert ist. Direct2D skaliert die Zeichnungs Einheiten bei der Zeichnung auf tatsächliche Pixel, indem die Einstellung Windows-dpi (dpi per inch) verwendet wird. Wenn Sie mithilfe von [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)Text zeichnen, geben Sie auch Dips anstelle von Punkten für die Größe der Schriftart an. DIPs werden als Gleitkommazahlen angegeben. 

### <a name="displaying-game-state-info"></a>Anzeigen von Informationen zum Spielzustand

Neben der Heads-Up-Anzeige hat das Beispiel Spiel eine Überlagerung, die sechs Spiel Zustände darstellt. Alle Zustände verfügen über ein großes schwarzes Rechteck mit Text, den der Spieler lesen soll. Die verschiebunden Controller Rechtecke und der Fadenkreuz werden nicht gezeichnet, da Sie in diesen Zuständen nicht aktiv sind.

Das Overlay wird mithilfe der [**gameinfooverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) -Klasse erstellt, sodass wir auslagern können, welcher Text angezeigt wird, um den Status des Spiels auszurichten.

![Status und Aktion der Überlagerung](images/simple-dx-game-ui-finaloverlay.png)

Das Overlay ist in zwei Abschnitte unterteilt: **Status** und **Aktion**. Der **statussecton** ist weiter unterteilt in **Titel** und **Text** Rechtecke. Der **Aktions** Abschnitt hat nur ein Rechteck. Jedes Rechteck hat einen anderen Zweck.

-   `titleRectangle` enthält den Titeltext.
-   `bodyRectangle` enthält den Textkörper.
-   `actionRectangle` enthält den Text, der den Player anweist, eine bestimmte Aktion auszuführen.

Das Spiel hat sechs Zustände, die festgelegt werden können. Der Zustand des Spiels, das mithilfe des **Status** Teils der Überlagerung übermittelt wird. Die **Status** Rechtecke werden mithilfe einer Reihe von Methoden aktualisiert, die den folgenden Zuständen entsprechen.

- Laden
- Anfängliche Start/High Score-Statistik
- Start der Ebene
- Spiel angehalten
- Spielende
- Spiel gewonnen


Der **Aktions** Teil der Überlagerung wird mithilfe der [**gameinfooverlay:: setAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) -Methode aktualisiert, sodass der Aktions Text auf einen der folgenden Werte festgelegt werden kann.
- "Tippen Sie, um erneut zu spielen..."
- "Laden der Ebene, bitte warten..."
- "Tippen Sie, um fortzufahren..."
- Keine

> [!NOTE]
> Beide Methoden werden im Abschnitt [darstellen des Spiel Zustands](#representing-game-state) ausführlicher erläutert.

Je nachdem, was im Spiel passiert, werden die Textfelder " **Status** " und " **Aktions** Abschnitt" angepasst.
Sehen wir uns nun an, wie wir das Overlay für diese sechs Zustände initialisieren und zeichnen.

### <a name="initializing-and-drawing-the-overlay"></a>Initialisieren und Zeichnen des Overlays

Die sechs **Status** Zustände haben einige Gemeinsamkeiten, sodass die benötigten Ressourcen und Methoden sehr ähnlich sind.
    - Sie verwenden alle ein schwarzes Rechteck in der Mitte des Bildschirms als Hintergrund.
    - Der angezeigte Text ist entweder **Titel** oder Text **Körper** .
    - Der Text verwendet die Segoe UI Schriftart und wird oberhalb des Rückseite-Rechtecks gezeichnet. 


Das Beispiel Spiel verfügt über vier Methoden, die beim Erstellen der Überlagerung ins Spiel kommen.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>Gameingefooverlay:: gameingefooverlay
Der [**gameinfooverlay:: gameinfooverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) -Konstruktor initialisiert die Überlagerung und verwaltet dabei die Bitmapoberfläche, auf der die Informationen für den Player angezeigt werden. Der Konstruktor ruft eine Factory aus dem an ihn übergebenen [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) -Objekt ab, das zum Erstellen eines [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) verwendet wird, zu dem das Überlagerungs Objekt selbst gezeichnet werden kann. [Idschreitefactory:: kreatetextformat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameingefooverlay:: createdevicedependentresources
[**Gameingefooverlay:: createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) ist unsere Methode zum Erstellen von Pinseln, die zum Zeichnen von Text verwendet werden. Zu diesem Zweck wird ein [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) -Objekt abgerufen, das das Erstellen und Zeichnen von Geometrie sowie die Funktionalität ermöglicht, wie z. b. Freihand-und Farbverlaufs Gitter Rendering. Anschließend erstellen wir eine Reihe farbiger Pinsel mit [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) , um die Elemente der Benutzeroberfläche zu zeichnen.
- Schwarzer Pinsel für Rechteck Hintergründe
- Weißer Pinsel für Status Text
- Orangefarbener Pinsel für Aktions Text

#### <a name="deviceresourcessetdpi"></a>Deviceresources:: setdpi

Die [**deviceresources:: setdpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) -Methode legt die Punkte pro Zoll des Fensters fest. Diese Methode wird aufgerufen, wenn der dpi-Wert geändert wird, und muss umgelesen werden, was geschieht, wenn die Größe des Spielfensters geändert wird. Nach dem Aktualisieren des dpi-Wert wird von dieser Methode auch [**deviceresources:: createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) aufgerufen, um sicherzustellen, dass die erforderlichen Ressourcen jedes Mal neu erstellt werden, wenn die Größe des Fensters geändert wird.

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>Gameingefooverlay:: createwindowssizedependentresources

Bei der [**gameinfooverlay:: createwindowssizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) -Methode findet all unsere Zeichnung statt. Im folgenden finden Sie eine Übersicht über die Schritte der Methode.
- Es werden drei Rechtecke erstellt, um den Benutzeroberflächen Text für den **Titel**, Text **Körper** und **Aktions** Text zu überlaufen.
    ```cppwinrt 
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

- Eine Bitmap wird `m_levelBitmap` mit dem Namen erstellt, wobei der aktuelle dpi-Wert mithilfe von " **kreatebitmap**" berücksichtigt wird.
- `m_levelBitmap` wird als 2D-Renderziel mithilfe von [**ID2D1DeviceContext:: SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget)festgelegt.
- Die Bitmap wird bei jedem Pixel, das mit [**ID2D1RenderTarget:: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear)schwarz gemacht wurde, gelöscht.
- [**ID2D1RenderTarget:: beginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) wird aufgerufen, um das Zeichnen zu initiieren. 
- **DrawText** wird aufgerufen, um den in `m_titleString` , und gespeicherten Text `m_bodyString` `m_actionString` im approperiate-Rechteck mithilfe des entsprechenden **ID2D1SolidColorBrush** zu zeichnen.
- [**ID2D1RenderTarget:: EndDraw**](ID2D1RenderTarget::EndDraw) wird aufgerufen, um alle Zeichnungsvorgänge für zu unterbinden `m_levelBitmap` .
- Eine andere **Bitmap wird** mit dem Namen erstellt `m_tooSmallBitmap` , der als Fallback verwendet wird, und nur dann angezeigt, wenn die Anzeige Konfiguration zu klein für das Spiel ist.
- Wiederholen Sie den Prozess zum Zeichnen von `m_levelBitmap` für `m_tooSmallBitmap` , und dieses Mal wird nur die Zeichenfolge `Paused` im Text gezeichnet.




Wir benötigen nun lediglich sechs Methoden, um den Text unserer sechs Überlagerungs Zustände auszufüllen!

### <a name="representing-game-state"></a>Darstellen des Spiel Zustands


Jeder der sechs Überlagerungs Zustände im Spiel verfügt über eine entsprechende Methode im **gameinfooverlay** -Objekt. Diese Methoden zeichnen eine Variante des Overlays, um dem Spieler explizite Informationen zum Spiel selbst mitzuteilen. Diese Kommunikation wird durch einen **Titel** und eine **Text** Zeichenfolge dargestellt. Da im Beispiel die Ressourcen und das Layout für diese Informationen bereits bei der Initialisierung und mit der [**gameinfooverlay:: createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) -Methode konfiguriert wurden, müssen nur die Überlagerungs Zustands spezifischen Zeichen folgen bereitgestellt werden.

Der **Status** Teil der Überlagerung wird durch einen-Rückruf einer der folgenden Methoden festgelegt.

Spielzustand | Statusset-Methode | Status Felder
:----- | :------- | :---------
Laden | [Gameingefooverlay:: setgameloading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Titel**</br>Laden von Ressourcen </br>**Text**</br> Druckt "." inkrementell, um das Laden von Aktivitäten zu implizieren
Anfängliche Start/High Score-Statistik | [Gameingefooverlay:: setgamestats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Titel**</br>Hohe Bewertung</br> **Text**</br> Abgeschlossene Ebenen # </br>Gesamtanzahl der Punkte #</br>Gesamtzahl der Fotos #
Start der Ebene | [Gameingefooverlay:: setlevelstart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Titel**</br>Geringen #</br>**Text**</br>Beschreibung der Ebene.
Spiel angehalten | [Gameingefooverlay:: setpause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Titel**</br>Spiel angehalten</br>**Text**</br>Keine
Spielende | [Gameingefooverlay:: setgameover](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titel**</br>Spielende</br> **Text**</br> Abgeschlossene Ebenen # </br>Gesamtanzahl der Punkte #</br>Gesamtzahl der Fotos #</br>Abgeschlossene Ebenen #</br>Hohe Bewertung #
Spiel gewonnen | [Gameingefooverlay:: setgameover](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titel**</br>Sie haben gewonnen!</br> **Text**</br> Abgeschlossene Ebenen # </br>Gesamtanzahl der Punkte #</br>Gesamtzahl der Fotos #</br>Abgeschlossene Ebenen #</br>Hohe Bewertung #

Mit der [**gameingefooverlay:: createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) -Methode hat das Beispiel drei rechteckige Bereiche deklariert, die bestimmten Bereichen der Überlagerung entsprechen.

Unter Berücksichtigung dieser Bereiche wollen wir nun eine der zustandsspezifischen Methoden (**GameInfoOverlay::SetGameStats**) näher untersuchen und sehen, wie das Overlay gezeichnet wird.

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

Mithilfe des Direct2D-Geräte Kontexts, der vom **gameinfooverlay** -Objekt initialisiert wurde, füllt diese Methode den Titel und die Text Rechtecke mit Black mithilfe des Hintergrund Pinsels. Sie zeichnet mit dem weißen Textpinsel den Text für die Zeichenfolge „High Score“ im Titelrechteck und eine Zeichenfolge mit den aktualisierten Spielzuständen im Textkörperrechteck.


Das Aktions Rechteck wird durch einen nachfolgenden Aufrufen von [**gameinfooverlay:: setAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) aus einer Methode für das **gamemain** -Objekt aktualisiert, das die für **gameinfooverlay:: setAction** benötigten Informationen zum Spielzustand bereitstellt, um die richtige Meldung an den Player zu ermitteln, z. b. "tippen Sie zum Fortfahren".

Das Overlay für einen beliebigen Zustand wird in der [**gamemain:: setgamanfooverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L606-L661) -Methode wie folgt ausgewählt:

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
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

Nun hat das Spiel eine Möglichkeit, Textinformationen auf der Grundlage des Spiel Zustands an den Player zu übermitteln, und wir haben eine Möglichkeit zum wechseln, was Ihnen im gesamten Spiel angezeigt wird.

### <a name="next-steps"></a>Nächste Schritte

Im nächsten Thema, dem [Hinzufügen von Steuerelementen](tutorial--adding-controls.md), wird erläutert, wie der Player mit dem Beispiel Spiel interagiert und wie die Eingabe den Spielzustand ändert.