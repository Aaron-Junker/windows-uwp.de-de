---
Description: Eine Kachel ist die Darstellung einer App im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues Windows-App-App-Projekt in Microsoft Visual Studio erstellen, enthält es eine Standard Kachel, die den Namen und das Logo Ihrer APP anzeigt.
title: Kacheln für Windows-apps
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0882ac67766bc2ce037133cf8a39b5393f616e13
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970995"
---
# <a name="tiles-for-windows-apps"></a>Kacheln für Windows-apps

 

Eine *Kachel* ist eine APP-Darstellung im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues Windows-App-App-Projekt in Microsoft Visual Studio erstellen, enthält es eine Standard Kachel, die den Namen und das Logo Ihrer APP anzeigt.Windows zeigt diese Kachel bei der erstmaligen Installation Ihrer App an. Nachdem Ihre App installiert wurde, können Sie den Inhalt der Kachel mithilfe von Benachrichtigungen ändern. Sie können die Kachel zum Beispiel so ändern, dass dem Benutzer neue Informationen angezeigt werden, wie etwa neue Schlagzeilen oder der Betreff der letzten ungelesenen Nachricht.

## <a name="configure-the-default-tile"></a>Konfigurieren der Standardkachel


Wenn Sie ein neues Projekt in Visual Studio erstellen, wird eine einfache Standardkachel erstellt, die den Namen und das Logo Ihrer App anzeigt.

Zum Bearbeiten der Kachel Doppelklicken Sie auf die Datei " **Package. appxmanifest** " im Haupt-UWP-Projekt, um den Designer zu öffnen (oder klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie Code anzeigen

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Aktualisieren Sie die folgenden Elemente:

-   DisplayName: Ersetzen Sie diesen Wert mit dem Namen, der auf der Kachel angezeigt werden soll.
-   ShortName: Da der Platz für Ihren Anzeigenamen auf Kacheln begrenzt ist, empfehlen wir, auch einen ShortName (Kurznamen) anzugeben, um sicherzustellen, dass der Name Ihrer App nicht abgeschnitten wird.
-   Logobilder:

    Ersetzen Sie diese Bilder durch eigene. Sie können Bilder für verschiedene visuelle Skalierungen bereitstellen, Sie müssen aber nicht alle bereitstellen. Um sicherzustellen, dass die App auf vielen Geräten gut aussieht, wird empfohlen, skalierte Versionen der Bilder mit jeweils 100 %, 200 % und 400 % bereitzustellen. Weitere Informationen zum Erstellen dieser Assets finden Sie unter [App-Symbole und-Logos](/windows/uwp/design/style/app-icons-and-logos) .

    Skalierte Bilder haben die folgende Benennungskonvention:
    
    *&lt;&gt;* Bildname. Skalierungsfaktor. * &lt;&gt;* *Image Dateierweiterung&gt; &lt;* 

    Beispiel: SplashScreen. Scale-100. png

    Wenn Sie auf das Image verweisen, verweisen Sie darauf als * &lt;Bildname&gt;*. *Image Dateierweiterung&gt; ("SplashScreen. png" in diesem Beispiel). &lt;* Das System wählt aus den bereitgestellten Bildern automatisch das entsprechend skalierte Bild für das Gerät aus.

-   Es wird ausdrücklich empfohlen, Logos für breite und große Kacheln bereitzustellen, damit der Benutzer die Größe der Kachel Ihrer App entsprechend anpassen kann. Um diese zusätzlichen Images bereitzustellen, erstellen Sie ein **defaulttile** -Element, und verwenden Sie die Attribute **Wide310x150Logo** und **Square310x310Logo** , um die zusätzlichen Images anzugeben:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>Verwenden von Benachrichtigungen zum Anpassen der Kachel


Nachdem Ihre App installiert wurde, können Sie die Kachel mit Benachrichtigungen anpassen. Dies ist möglich, wenn Ihre APP zum ersten Mal gestartet wird, oder als Reaktion auf ein Ereignis, z. b. eine Pushbenachrichtigung.

Informationen zum Senden von Kachel Benachrichtigungen finden Sie unter [Senden einer lokalen Kachel Benachrichtigung](sending-a-local-tile-notification.md).
