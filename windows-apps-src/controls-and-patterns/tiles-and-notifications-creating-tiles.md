---
author: mijacobs
Description: "Eine Kachel ist die Darstellung einer App im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues App-Projekt für die Universelle Windows-Plattform (UWP) in Microsoft Visual Studio erstellen, enthält es eine Standardkachel, die den Namen und das Logo der App anzeigt."
title: Kacheln
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, UWP
ms.openlocfilehash: 8907b57bce9c39c1c508b97536485a08e8e1bf83
ms.sourcegitcommit: 9a1310468970c8d1ade0fb200126dff56ea8c9e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2017
---
# <a name="tiles-for-uwp-apps"></a>Kacheln für UWP-Apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Eine *Kachel* ist die Darstellung einer App im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues UWP-App-Projekt in Microsoft Visual Studio erstellen, enthält es eine Standardkachel, die den Namen und das Logo Ihrer App anzeigt. Windows zeigt diese Kachel bei der erstmaligen Installation Ihrer App an. Nachdem Ihre App installiert wurde, können Sie den Inhalt der Kachel mithilfe von Benachrichtigungen ändern. Sie können die Kachel zum Beispiel so ändern, dass dem Benutzer neue Informationen angezeigt werden, wie etwa neue Schlagzeilen oder der Betreff der letzten ungelesenen Nachricht.

## <a name="configure-the-default-tile"></a>Konfigurieren der Standardkachel


Wenn Sie ein neues Projekt in Visual Studio erstellen, wird eine einfache Standardkachel erstellt, die den Namen und das Logo Ihrer App anzeigt.

Um die Kachel zu bearbeiten, doppelklicken Sie auf die Datei **Package.appxmanifest** in Ihrem Haupt-UWP-Projekt, öffnen Sie den Designer (oder klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie „Code anzeigen” aus).

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

    Ersetzen Sie diese Bilder durch eigene. Sie können Bilder für verschiedene visuelle Skalierungen bereitstellen, Sie müssen aber nicht alle bereitstellen. Um sicherzustellen, dass die App auf vielen Geräten gut aussieht, wird empfohlen, skalierte Versionen der Bilder mit jeweils 100 %, 200 % und 400 % bereitzustellen. Weitere Informationen zum Erstellen dieser Ressourcen finden Sie unter [Ressourcen für Kacheln und Symbole](tiles-and-notifications-app-assets.md).

    Skalierte Bilder haben die folgende Benennungskonvention:
    
    *&lt;Bildname&gt;*.scale-*&lt;Skalierungsfaktor&gt;*.*&lt;Bilddateierweiterung&gt;* 

    Beispiel: SplashScreen.scale-100.png

    Wenn Sie auf das Bild verweisen, verweisen Sie auf *&lt;Bildname&gt;*.*&lt;Bilddateierweiterung&gt;* („SplashScreen.png“ in diesem Beispiel). Das System wählt aus den bereitgestellten Bildern automatisch das entsprechend skalierte Bild für das Gerät aus.

-   Es wird ausdrücklich empfohlen, Logos für breite und große Kacheln bereitzustellen, damit der Benutzer die Größe der Kachel Ihrer App entsprechend anpassen kann. Erstellen Sie zum Bereitstellen dieser zusätzlichen Bilder ein `DefaultTile`-Element, und verwenden Sie die `Wide310x150Logo`- und `Square310x310Logo`-Attribute, um zusätzliche Bilder anzugeben:
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


Nachdem Ihre App installiert wurde, können Sie die Kachel mit Benachrichtigungen anpassen. Dies kann entweder beim ersten Start der App oder als Reaktion auf ein Ereignis wie eine Pushbenachrichtigung geschehen.

Informationen über das Senden von Kachelbenachrichtigungen finden Sie unter [Senden einer lokalen Kachelbenachrichtigung ](tiles-and-notifications-sending-a-local-tile-notification.md).