---
title: Erstellen eines UWP-Spiels in MonoGame-2D
description: Befolgen Sie dieses Tutorial, um ein einfaches 2D-UWP-Spiel für den Microsoft Store, geschrieben in C# und MonoGame, zu erstellen.
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: b6e4b5bd75e0aff96cfc61f9b1bac380c61ae46f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162874"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Erstellen eines UWP-Spiels in MonoGame-2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Ein einfaches 2D-UWP-Spiel für den Microsoft Store, geschrieben in C# und MonoGame


![Sprite-Blatt für laufenden Dinosaurier](images/JS2D_0.png)

## <a name="introduction"></a>Einführung

MonoGame ist ein einfaches Framework für die Spieleentwicklung. In diesem Tutorial lernen Sie die Grundlagen der Entwicklung von Spielen in MonoGame kennen und erfahren, wie Sie Inhalte laden, Sprites zeichnen und animieren und Benutzereingaben verarbeiten. Außerdem werden einige erweiterte Konzepte wie die Erkennung von Kollisionen und die Skalierung für Bildschirme mit hoher DPI-Auflösung besprochen. Dieses Tutorial dauert 30 bis 60 Minuten.

## <a name="prerequisites"></a>Voraussetzungen
+   Windows 10 und Microsoft Visual Studio 2019.  [Klicken Sie hier, um zu erfahren, wie Sie Visual Studio einrichten](./get-set-up.md).
+ Das .NET-Desktop-Entwicklungsframework. Wenn Sie das Framework noch nicht installiert haben, können Sie dies nachholen, indem Sie den Visual Studio-Installer erneut ausführen und Ihre Installation von Visual Studio 2019 anpassen.
+   Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache. [Klicken Sie hier, um Informationen zum Einstieg in C# zu erhalten](./create-a-hello-world-app-xaml-universal.md).
+   Kenntnisse der grundlegenden Konzepte der Informatik, wie Klassen, Methoden und Variablen, sind ebenfalls von Vorteil.

## <a name="why-monogame"></a>Warum MonoGame?
In Umgebungen für die Spieleentwicklung gibt es ausreichende Optionen. Von Engines mit vollem Funktionsumfang wie Unity bis hin zu umfassenden und komplexen Multimedia-APIs wie DirectX. Womit soll man beginnen? MonoGame ist ein Toolset, das in der Komplexität zwischen einer Spielengine und einer ausgefeilteren API wie DirectX liegt. Es bietet eine einfach zu verwendende Inhaltspipeline und alle erforderlichen Funktionen, um einfache Spiele zu erstellen, die auf einer Vielzahl von Plattformen ausgeführt werden können. Und das Beste ist: MonoGame-Apps werden in reinem C#-Code geschrieben und können schnell über den Microsoft Store oder ähnliche Plattformen verteilt werden.

## <a name="get-the-code"></a>Code abrufen
Wenn Sie das Tutorial nicht schrittweise durchgehen, sondern nur MonoGame in Aktion sehen möchten, [klicken Sie hier, um die fertige App abzurufen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Öffnen Sie das Projekt in Visual Studio 2019, und drücken Sie **F5**, um das Beispiel auszuführen. Beim ersten Mal kann dies etwas länger dauern, da Visual Studio alle NuGet-Pakete abrufen muss, die in Ihrer Installation nicht enthalten sind.

Wenn Sie dies getan haben, überspringen Sie den nächsten Abschnitt zum Einrichten von MonoGame, und sehen Sie sich eine schrittweise exemplarische Vorgehensweise zum Code an.

**Hinweis:** Das in diesem Beispiel erstellte Spiel hat keinen Anspruch darauf, vollständig zu sein (oder sogar Spaß zu machen). Sein einziger Zweck besteht darin, die grundlegenden Konzepte der 2D-Entwicklung in MonoGame zu veranschaulichen. Verwenden Sie diesen Code und verbessern Sie ihn – oder beginnen Sie einfach ganz von vorne, nachdem Sie die Grundlagen kennengelernt haben.

## <a name="set-up-monogame-project"></a>Einrichten des MonoGame-Projekts
1. Installieren Sie **MonoGame 3.6** für Visual Studio von [MonoGame.net](https://www.monogame.net/).

2. Starten Sie Visual Studio 2019.

3. Wählen Sie **Datei -> Neu -> Projekt** aus.

4. Wählen Sie in den Visual C#-Projektvorlagen **MonoGame** und **MonoGame Windows 10 Universal-Projekt** aus.

5. Nennen Sie Ihr Projekt „MonoGame2D“, und wählen Sie „OK“ aus. Wenn das Projekt erstellt wurde, enthält es anscheinend zahlreiche Fehler, die aber nach der ersten Ausführung des Projekts und nach der Installation aller fehlenden NuGet-Pakete beseitigt sein sollten.

6. Stellen Sie sicher, dass **x86** und **Lokaler Computer** als Zielplattform angegeben sind, und drücken Sie **F5**, um das leere Projekt zu erstellen und auszuführen. Wenn Sie die oben beschriebenen Schritte ausgeführt haben, sollte ein leeres blaues Fenster angezeigt werden, sobald das Projekt erstellt wurde.

## <a name="method-overview"></a>Übersicht über die Methoden
Nachdem Sie das Projekt erstellt haben, öffnen Sie die Datei **Game1.cs** im **Projektmappen-Explorer**. Hier wird ein Großteil der Spiellogik abgelegt. Viele wichtige Methoden werden hier automatisch generiert, wenn Sie ein neues MonoGame-Projekt erstellen. Diese Methoden sollen hier kurz erläutert werden:

**public Game1()** – Der Konstruktor. Diese Methode wird für dieses Tutorial nicht geändert.

**protected override void Initialize()** – Hier werden alle verwendeten Klassenvariablen initialisiert. Diese Methode wird zu Beginn des Spiels einmal aufgerufen.

**protected override void LoadContent()** – Diese Methode lädt Inhalt (z. B. Texturen, Audiodaten, Schriftarten) in den Arbeitsspeicher, bevor das Spiel gestartet wird. Wie die Initialize-Methode wird sie beim Starten der App einmal aufgerufen.

**protected override void UnloadContent()** – Mit dieser Methode wird Inhalt entladen, der nicht aus dem Inhalts-Manager stammt. Diese Methode wird nicht verwendet.

**protected override void Update(GameTime gameTime)** – Diese Methode wird einmalig für jeden Zyklus der Spieleschleife aufgerufen. Hier wird der Status der im Spiel verwendeten Objekte oder Variablen aktualisiert. Dies betrifft auch die Position, Geschwindigkeit oder Farbe eines Objekts. Außerdem werden hier Benutzereingaben verarbeitet. Kurz gesagt, behandelt diese Methode alle Teile der Spiellogik mit Ausnahme des Zeichnens von Objekten auf dem Bildschirm.

**protected override void Draw(GameTime gameTime)** – Hier werden Objekte auf dem Bildschirm gezeichnet. Dabei werden die Positionen verwendet, die von der Update-Methode angegeben wurde.

## <a name="draw-a-sprite"></a>Zeichnen eines Sprites
Sie haben das neue MonoGame-Projekt ausgeführt und sehen einen schöne blauen Himmel. Nun fügen wird Boden hinzu.
In MonoGame werden der App 2D-Zeichnungen in Form von „Sprites“ hinzugefügt. Ein Sprite ist eine Computergrafik, die als eine Einheit bearbeitet wird. Sprites können verschoben, skaliert, geformt, animiert und kombiniert werden, sodass alles erstellt werden kann, was Sie sich im 2D-Raum vorstellen können.

### <a name="1-download-a-texture"></a>1. Herunterladen einer Textur
Für unsere Zwecke soll das erste Sprite äußerst langweilig sein. [Klicken Sie hier, um dieses funktionslose grüne Rechteck herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Hinzufügen der Textur zum Inhaltsordner
- Öffnen Sie den **Projektmappen-Explorer**.
- Klicken Sie mit der rechten Maustaste im Ordner **Content** auf **Content.mgcb**, und wählen Sie **Öffnen mit** aus. Wählen Sie im Popupmenü **Monogame Pipeline** aus, und wählen Sie dann **OK** aus.
- Klicken Sie im neuen Fenster mit der rechten Maustaste auf das **Content**-Element, und wählen Sie **Hinzufügen -> Vorhandenes Element** aus.
- Suchen Sie das grüne Rechteck im Dateibrowser, und wählen Sie es aus.
- Nennen Sie das Element „grass.png“, und wählen Sie **Hinzufügen** aus.

### <a name="3-add-class-variables"></a>3. Hinzufügen von Klassenvariablen
Öffnen Sie zum Laden dieses Bilds als Sprite-Textur **Game1.cs**, und fügen Sie die folgenden Klassenvariablen hinzu.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

Die Variable SKYRATIO gibt das Verhältnis zwischen Himmel und Gras in der Szene an – in diesem Fall zwei Drittel zu einem Drittel. Die Variablen **screenWidth** und **screenHeight** überwachen die Größe des App-Fensters, und in **grass** speichern wir das grüne Rechteck.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Initialisieren der Klassenvariablen und Festlegen der Fenstergröße
Die Variablen **screenWidth** und **screenHeight** müssen noch initialisiert werden. Daher fügen Sie diesen Code der **Initialize**-Methode hinzu:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Wir haben nun die Höhe und Breite des Bildschirms und den Ansichtsmodus der App auf **Vollbild** festgelegt. Außerdem bleibt die Maus unsichtbar.

### <a name="5-load-the-texture"></a>5. Laden der Textur
Fügen Sie der **LoadContent**-Methode Folgendes hinzu, um die Textur in die grass-Variable zu laden:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Zeichnen des Sprite
Fügen Sie der **Draw**-Methode die folgenden Zeilen hinzu, um das Rechteck zu zeichnen:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Hier verwenden wir die **spriteBatch.Draw**-Methode, um die angegebene Textur innerhalb der Umrandung eines Rectangle-Objekts zu platzieren. Ein **Rectangle**-Objekt wird durch die x- und y-Koordinaten seiner oberen linken und unteren rechte Ecke definiert. Mit den zuvor definierten Variablen **screenWidth**, **screenHeight** und **SKYRATIO** zeichnen wir die Textur des grünen Rechtecks im unteren Drittel des Bildschirms. Wenn Sie das Programm ausführen, sehen Sie jetzt den vorherigen blauen Hintergrund, der teilweise durch das grüne Rechteck verdeckt ist.

![Grünes Rechteck](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Skalierung für Bildschirme mit hoher DPI-Auflösung
Wenn Sie Visual Studio auf einem Monitor mit hoher Pixeldichte ausführen, wie beispielsweise auf einem Surface Pro oder Surface Studio, könnte es sein, dass das untere Bildschirmdrittel von dem in den obigen Schritten erstellten grünen Rechteck nicht vollständig ausgefüllt wird. Möglicherweise befindet es sich unverankert über der unteren linken Bildschirmecke. Um dieses Problem beheben und die Oberfläche des Spiels auf allen Geräten zu vereinheitlichen, müssen wir eine Methode erstellen, mit der bestimmte Werte im Verhältnis zur Pixeldichte des Bildschirms skaliert werden:

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

Als Nächstes ersetzen Sie die Initialisierung der Variablen **screenHeight** und **screenWidth** in der **Initialize**-Methode wie folgt:

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

Wenn Sie einen Bildschirm mit hoher DPI-Auflösung verwenden und die App jetzt ausführen, sollte das grüne Rechteck das untere Drittel wie gewünscht verdecken.

## <a name="build-the-spriteclass"></a>Erstellen der SpriteClass
Bevor wir mit der Animation von Sprites beginnen, erstellen wir eine neue Klasse namens „SpriteClass“, um die Komplexität der Sprite-Bearbeitung auf der Oberfläche zu verringern.

### <a name="1-create-a-new-class"></a>1. Erstellen einer neuen Klasse
Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **MonoGame2D (Universal Windows)** , und wählen Sie **Hinzufügen -> Klasse** aus. Nennen Sie die Klasse „SpriteClass.cs“, und wählen Sie dann **Hinzufügen** aus.

### <a name="2-add-class-variables"></a>2. Hinzufügen von Klassenvariablen
Fügen Sie der Klasse, die Sie gerade erstellt haben, den folgenden Code hinzu:

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

Hier richten wir die Klassenvariablen ein, die wir zum Zeichnen und Animieren eines Sprites benötigen. Die Variablen **x** und **y** stellen die aktuelle Position des Sprite auf der Ebene dar, während die Variable **angle** der aktuelle Winkel des Sprites in Grad ist (0 – aufrecht, 90 – um 90 Grad im Uhrzeigersinn geneigt). Beachten Sie unbedingt, dass **x** und **y** für diese Klasse die Koordinaten des **Mittelpunkts** des Sprites darstellen (der standardmäßige Ursprung entspricht der oberen linken Ecke). Dadurch lassen sich Sprites einfacher drehen, da sie sich um einen beliebigen angegebenen Ursprung drehen. Durch die Drehung um den Mittelpunkt erhalten wir eine einheitliche Drehbewegung.

Danach haben wir **dX**, **dY** und **dA**, die die Änderungsraten der Variablen **x**, **y** und **angle** pro Sekunde darstellen.

### <a name="3-create-a-constructor"></a>3. Erstellen eines Konstruktors
Wenn wir eine Instanz der **SpriteClass** erstellen, stellen wir Folgendes für den Konstruktor bereit: das Grafikgerät aus **Game1.cs**, den Pfad zur Textur relativ zum Projektordner und die gewünschte Skalierung der Textur relativ zu ihrer Originalgröße. Wir richten die übrigen Klassenvariablen in der Update-Methode ein, nachdem wir das Spiel gestartet haben.

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. Aktualisieren und Zeichnen
Es gibt noch einige Methoden, die wir der SpriteClass-Deklaration hinzufügen müssen:

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

Die **Update**-Methode von SpriteClass wird in der **Update**-Methode von „Game1.cs“ aufgerufen. Mit ihr werden die Werte **x**, **y** und **angle** des Sprites auf der Grundlage ihrer jeweiligen Änderungsraten aktualisiert.

Die **Draw**-Methode wird in der **Draw**-Methode von Game1.cs aufgerufen. Mit ihr wird das Sprite im Fenster des Spiels gezeichnet.

## <a name="user-input-and-animation"></a>Benutzereingaben und Animation
Wir haben die SpriteClass erstellt und nutzen diese nun, um zwei neue Spielobjekte zu erstellen: zunächst einen Avatar, den der Spieler mit den Pfeiltasten und der LEERTASTE steuern kann. Das zweite Objekt ist ein Hindernis, dem der Spieler ausweichen muss.

### <a name="1-get-the-textures"></a>1. Herunterladen der Texturen
Für den Avatar des Spielers verwenden wir die Ninja Cat von Microsoft, die auf einem riesigen Tyrannosaurus Rex reitet. [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Nun geht es um das Hindernis, dem der Spieler ausweichen muss. Was hassen eine Ninja Cat und ein fleischfressender Dinosaurier am meisten? Gemüse! [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Genauso wie zuvor bei dem grünen Rechteck fügen Sie diese Bilder nun über die **MonoGame-Pipeline** in **Content.mgcb** hinzu. Nennen Sie sie „ninja-cat-dino.png“ und „broccoli.png“.

### <a name="2-add-class-variables"></a>2. Hinzufügen von Klassenvariablen
Fügen Sie folgenden Code zur Liste der Klassenvariablen in **Game1.cs** hinzu:

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** und **broccoli** sind unsere SpriteClass-Variablen. **dino** stellt den Avatar des Spielers dar, während **broccoli** das Brokkoli-Hindernis ist.

**spaceDown** überwacht, ob die LEERTASTE gehalten oder nach unten gedrückt und losgelassen wird.

**gameStarted** gibt an, ob der Benutzer das Spiel zum ersten Mal gestartet hat.

**broccoliSpeedMultiplier** bestimmt, wie schnell das Brokkoli-Hindernis sich über den Bildschirm bewegt.

**gravitySpeed** bestimmt, wie schnell der Avatar des Spielers nach einem Sprung wieder nach unten beschleunigt wird.

**dinoSpeedX** und **dinoJumpY** bestimmen, wie schnell der Avatar des Spielers sich bewegt und springt.
„score“ verfolgt, wie vielen Hindernissen der Spieler erfolgreich ausgewichen ist.

Mit **random** wird zuletzt ein Zufallswert auf das Verhalten des Brokkoli-Hindernisses angewendet.

### <a name="3-initialize-variables"></a>3. Initialisieren von Variablen
Als Nächstes müssen diese Variablen initialisiert werden. Fügen Sie der Initialize-Methode den folgenden Code hinzu:

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

Beachten Sie, dass die letzten drei Variablen für Geräte mit hoher DPI-Auflösung skaliert werden müssen, da sie eine Änderungsrate in Pixeln angeben.

### <a name="4-construct-spriteclasses"></a>4. Erstellen von SpriteClasses
Wir erstellen SpriteClass-Objekte in der **LoadContent**-Methode. Fügen Sie dem bereits vorhandenen Code diesen Code hinzu:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

Das Brokkoli-Bild ist viel größer, als es im Spiel angezeigt werden soll. Daher müssen wir es auf das 0,2-Fache seiner Originalgröße skalieren.

### <a name="5-program-obstacle-behaviour"></a>5. Programmieren des Verhaltens des Hindernisses
Der Brokkoli soll über den Bildschirm hinausreichen und in Richtung des Avatars des Spielers wandern, der dem Hindernis ausweichen muss. Dazu fügen Sie der **Game1.cs**-Klasse diese Methode hinzu:

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

Der erste Teil der Methode bestimmt, von welchem Punkt außerhalb des Bildschirms das Brokkoli-Objekt ausgehen soll. Dazu werden zwei Zufallszahlen erzeugt.

Im zweiten Teil wird bestimmt, wie schnell der Brokkoli sich bewegt, wobei die Geschwindigkeit vom aktuellen Spielstand abhängt. Nachdem der Spieler fünf Brokkoli ausgewichen ist, erhöht sich immer die Geschwindigkeit.

Der dritte Teil legt die Richtung der Bewegung des Brokkoli-Sprites fest. Wenn der Brokkoli auftaucht, bewegt er sich in Richtung des Spieler-Avatars (Dino). Er erhält außerdem den **dA**-Wert 7f, was bewirkt, dass der Brokkoli bei der Jagd nach dem Spieler durch die Luft wirbelt.

### <a name="6-program-game-starting-state"></a>6. Programmieren des Anfangszustands des Spiels
Bevor wir mit der Verarbeitung von Tastatureingaben fortfahren, benötigen wir eine Methode, mit der der Anfangszustand der beiden von uns erstellten Objekte festgelegt wird. Das Spiel soll nicht sofort gestartet werden, wenn die App ausgeführt wird. Vielmehr soll es vom Benutzer manuell gestartet werden, indem er die LEERTASTE drückt. Fügen Sie den folgenden Code hinzu, mit dem der Anfangszustand der animierten Objekte festgelegt und der Punktestand zurückgesetzt wird:

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7. Verarbeiten von Tastatureingaben
Als Nächstes benötigen wir eine neue Methode für die Verarbeitung von Benutzereingaben über die Tastatur. Fügen Sie diese Methode zu **Game1.cs** hinzu:

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

Wir sehen oben eine Reihe von vier „if“-Anweisungen:

Die erste beendet das Spiel, wenn die **ESC**-TASTE gedrückt wird.

Die zweite startet das Spiel, wenn die **LEERTASTE** gedrückt wird und das Spiel noch nicht gestartet wurde.

Die dritte lässt den Dino-Avatar springen, wenn die **LEERTASTE** gedrückt wird. Dazu wird die **dY**-Eigenschaft geändert. Beachten Sie, dass der Spieler nur springen kann, wenn er sich auf dem „Boden“ befindet (dino.y = screenHeight * SKYRATIO). Außerdem springt er nur, wenn die LEERTASTE einmal gedrückt und nicht gehalten wird. Dies verhindert, dass der Dino nach Spielstart sofort springt, weil die LEERTASTE auch zum Starten des Spiels gedrückt wird.

Mit der letzten „if/else“-Klausel wird schließlich überprüft, ob die NACH-LINKS- oder NACH-RECHTS-TASTE gedrückt wird. In diesem Fall wird die **dX**-Eigenschaft des Dinos entsprechend geändert.

**Aufgabe:** Schaffen Sie es, dass die obige Tastatureingabemethode mit dem WASD-Eingabeschema genauso wie mit den Pfeiltasten funktioniert?

### <a name="8-add-logic-to-the-update-method"></a>8. Hinzufügen von Logik zur Update-Methode
Als Nächstes müssen wir für alle diese Teile Logik zur **Update**-Methode in **Game1.cs** hinzufügen:

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. Zeichnen von SpriteClass-Objekten
Fügen Sie zum Schluss der **Draw**-Methode in **Game1.cs** direkt nach dem letzten Aufruf von **spriteBatch.Draw** den folgenden Code hinzu:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

In MonoGame werden neue Aufrufe von **spriteBatch.Draw** über alle vorherigen Aufrufe gezeichnet. Dies bedeutet, dass das Brokkoli- und das Dino-Sprite über das vorhandene Gras-Sprite gezeichnet werden, sodass sie unabhängig von ihrer Position niemals dahinter verborgen werden können.

Führen Sie das Spiel nun aus, und bewegen Sie den Dino mit den Pfeiltasten und der LEERTASTE. Wenn Sie die oben beschriebenen Schritte ausgeführt haben, sollten Sie Ihren Avatar innerhalb des Spielfensters bewegen können, und der Brokkoli sollte immer schneller werden.

![Spieler-Avatar und Hindernis](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Rendern von Text mit SpriteFont
Mit dem obigen Code überwachen wir im Hintergrund die Punktzahl des Spielers, aber wir teilen dem Spieler das Ergebnis nicht mit. Außerdem gibt es beim Start der App eine wenig intuitive Einführung: Der Spieler sieht ein blaues und ein grünes Fenster, weiß aber nicht, dass er die LEERTASTE drücken muss, damit es losgeht.

Zur Behebung dieser Probleme nutzen wir eine neue Art von MonoGame-Objekt mit dem Namen **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Erstellen von Beschreibungsdateien mit SpriteFont
Suchen Sie im **Projektmappen-Explorer** den Ordner **Content**. Klicken Sie in diesem Ordner mit der rechten Maustaste auf die Datei **Content.mgcb**, und wählen Sie **Öffnen mit** aus. Wählen Sie aus dem Popupmenü **MonoGame-Pipeline** aus, und drücken Sie dann **OK**. Klicken Sie im neuen Fenster mit der rechten Maustaste auf das **Content**-Element, und wählen Sie **Hinzufügen -> Neues Element** aus. Wählen Sie **SpriteFont-Beschreibung** aus, benennen Sie sie mit „Score“, und drücken Sie **OK**. Dann fügen Sie auf die gleiche Weise eine weitere SpriteFont-Beschreibung mit dem Namen „GameState“ hinzu.

### <a name="2-edit-descriptions"></a>2. Bearbeiten von Beschreibungen
Klicken Sie in der **MonoGame-Pipeline** mit der rechten Maustaste auf den Ordner **Content**, und wählen Sie **Dateispeicherort öffnen** aus. Ein Ordner mit den soeben erstellten SpriteFont-Beschreibungsdateien sollte angezeigt werden. Er sollte zudem alle Bilder enthalten, die Sie dem Ordner „Content“ bisher hinzugefügt haben. Sie können jetzt das Fenster „MonoGame-Pipeline“ speichern und schließen. Öffnen Sie über den **Datei-Explorer** beide Beschreibungsdateien in einem Text-Editor (Visual Studio, Notepad++, Atom usw.).

Jede Beschreibung enthält eine Reihe von Werten, die SpriteFont beschreiben. Wir werden einige Änderungen vornehmen:

Ändern Sie in **Score.spritefont** den **<Size>** -Wert von 12 auf 36.

Ändern Sie in **GameState.spritefont** den **<Size>** -Wert von 12 auf 72 und den **<FontName>** -Wert von Arial in Agency. Agency ist eine weitere Schriftart, die standardmäßig auf Windows 10-Computern bereitgestellt wird und dem Begrüßungsbildschirm mehr Flair verleiht.

### <a name="3-load-spritefonts"></a>3. Laden von SpriteFonts
Zurück in Visual Studio fügen wir zuerst eine neue Textur für den Begrüßungsbildschirm hinzu. [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Fügen Sie wie zuvor die Textur zum Projekt hinzu, indem Sie mit der rechten Maustaste auf „Content“ klicken und **Hinzufügen -> Vorhandenes Element** auswählen. Nennen Sie das neue Element „start-splash.png“.

Als Nächstes fügen Sie **Game1.cs** die folgenden Klassenvariablen hinzu:

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

Dann fügen Sie der **LoadContent**-Methode die folgenden Zeilen hinzu:

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. Zeichnen der Punktezahl
Wechseln Sie zur **Draw**-Methode von **Game1.cs**, und fügen Sie unmittelbar vor **spriteBatch.End();** den folgenden Code hinzu.

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

Der obige Code verwendet die Sprite-Beschreibung, die wir erstellt haben (Arial, Schriftgrad 36), um den aktuellen Punktestand des Spielers in der Nähe der oberen rechten Bildschirmecke zu zeichnen.

### <a name="5-draw-horizontally-centered-text"></a>5. Zeichnen von horizontal zentriertem Text
Wenn Sie ein Spiel entwickeln, möchten Sie wahrscheinlich häufiger Text zeichnen, der entweder horizontal oder vertikal zentriert ist. Wenn Sie den Einführungstext horizontal zentrieren möchten, fügen Sie diesen Code der **Draw**-Methode unmittelbar vor **spriteBatch.End();** hinzu.

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

Zunächst erstellen wir zwei Zeichenfolgen, eine für jede zu zeichnende Textzeile. Als Nächstes messen wird die Breite und Höhe jeder ausgegebenen Zeile mit der **SpriteFont.MeasureString(String)** -Methode. Dadurch können wir die Größe als **Vector2**-Objekt mit der **X**-Eigenschaft für die Breite und der **Y**-Eigenschaft für die Höhe angeben.

Zuletzt zeichnen wir die einzelnen Zeilen. Um den Text horizontal zu zentrieren, legen wir den **X**-Wert des Positionsvektors auf den gleichen Wert wie **screenWidth / 2 - textSize.X / 2** fest.

**Aufgabe:** Wie würden Sie die obige Prozedur ändern, um den Text sowohl vertikal als auch horizontal zu zentrieren?

Starten Sie das Spiel. Sehen Sie den Begrüßungsbildschirm? Wird die Punktezahl mit jedem Ausweichen vor dem Brokkoli erhöht?

![Begrüßungsbildschirm](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Erkennung von Kollisionen
Nun haben wir einen Brokkoli, der den Spieler verfolgt. Außerdem gibt es eine Punktezahl, die sich immer dann erhöht, wenn der Spieler ausweicht. Aber im Moment gibt es noch keinen Weg, das Spiel auch zu verlieren. Wir müssen wissen, ob das Dino-Sprite und das Brokkoli-Sprite kollidieren, und falls ja, wie wir das Spiel für beendet erklären.

### <a name="1-get-the-textures"></a>1. Herunterladen der Texturen
Zuletzt benötigen wir ein Bild für das Spielende. [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/game-over.png).

Genauso wie zuvor bei den Bildern für das grüne Rechteck, Ninja Cat und den Brokkoli fügen Sie dieses Bild über die **MonoGame-Pipeline** der Datei **Content.mgcb** hinzu und nennen es „game-over.png“.

### <a name="2-rectangular-collision"></a>2. Kollision zwischen Rechtecken
Beim Erkennen von Kollisionen in einem Spiel werden Objekte oft vereinfacht, damit die nötigen Berechnungen nicht allzu kompliziert werden. Wir behandeln sowohl den Avatar des Spielers als auch das Brokkoli-Hindernis als Rechtecke, um eine Kollision dieser Objekte erkennen zu können.

Öffnen Sie **SpriteClass.cs**, und fügen Sie eine neue Klassenvariable hinzu:

```CSharp
const float HITBOXSCALE = .5f;
```

Dieser Wert stellt dar, inwieweit dem Spieler eine erkannte Kollision „verziehen“ wird. Bei dem Wert „.5f“ werden die Kanten des Rechtecks, in dem der Dino mit dem Brokkoli kollidieren kann – häufig auch „Hitbox“ genannt – auf die Hälfte der Gesamtgröße der Textur verkleinert. Damit können die Ecken der beiden Texturen in wenigen Fällen kollidieren, ohne dass Teile des Bildes tatsächlich sichtbar kollidieren. Passen Sie diesen Wert nach Bedarf an.

Fügen Sie dann **SpriteClass.cs** eine neue Methode hinzu:

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

Diese Methode bestimmt, ob zwei rechteckige Objekte kollidiert sind. Mit dem Algorithmus wird getestet, ob zwischen den Seiten der Rechtecke eine Lücke besteht. Wenn es eine Lücke gibt, ist keine Kollision erfolgt – wenn keine Lücke vorhanden ist, muss eine Kollision vorliegen.

### <a name="3-load-new-textures"></a>3. Laden neuer Texturen

Öffnen Sie dann **Game1.cs**, und fügen Sie zwei neue Klassenvariablen hinzu: eine zum Speichern der Sprite-Textur für das Spielende und einen booleschen Wert, mit dem Zustand des Spiels überwacht wird:

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

Anschließend initialisieren Sie **gameOver** in der **Initialize**-Methode:

```CSharp
gameOver = false;
```

Laden Sie schließlich die Textur in der **LoadContent**-Methode in **gameOverTexture**:

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="4-implement-game-over-logic"></a>4. Implementieren der Logik für das Spielende
Fügen Sie der **Update**-Methode direkt nach dem Aufruf der **KeyboardHandler**-Methode diesen Code hinzu:

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

Dadurch werden nach Spielende alle Bewegungen beendet und das Dino- und Brokkoli-Sprite in ihrer aktuellen Position fixiert.

Als Nächstes fügen Sie am Ende der **Update**-Methode unmittelbar vor **base.Update(gameTime)** diese Zeile ein:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Dadurch wird die **RectangleCollision**-Methode aufgerufen, die wir in **SpriteClass** erstellt haben, und das Spiel wird als „beendet“ gekennzeichnet, wenn der Wert „true“ zurückgegeben wird.

### <a name="5-add-user-input-for-resetting-the-game"></a>5. Hinzufügen von Benutzereingaben für das Zurücksetzen des Spiels
Fügen Sie der **KeyboardHandler**-Methode diesen Code hinzu, damit der Benutzer das Spiel durch Drücken der EINGABETASTE zurücksetzen kann:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="6-draw-game-over-splash-and-text"></a>6. Zeichnen des Bildschirms und Texts für das Spielende
Abschließend fügen Sie der Draw-Methode direkt nach dem ersten Aufruf von **spriteBatch.Draw** (dies sollte der Aufruf sein, mit dem die Rasen-Textur gezeichnet wird) diesen Code hinzu.

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

Hier verwenden wir dieselbe Methode wie zuvor zum Zeichnen von horizontal zentriertem Text, wobei wir wieder die Schriftart aus dem Begrüßungsbildschirm verwenden. Außerdem wird **gameOverTexture** in der oberen Fensterhälfte zentriert dargestellt.

Nun sind wir fertig! Starten Sie das Spiel erneut. Wenn Sie die oben beschriebenen Schritte ausgeführt haben, sollte das Spiel jetzt beendet werden, wenn der Dino mit dem Brokkoli kollidiert. Dann sollte der Spieler zum Neustarten des Spiels durch Drücken der EINGABETASTE aufgefordert werden.

![Spielende](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Veröffentlichen im Microsoft Store
Da wir dieses Spiel als UWP-App erstellt haben, können Sie dieses Projekt im Microsoft Store veröffentlichen. Dazu müssen Sie einige Voraussetzungen erfüllen.

Sie müssen als Windows-Entwickler [registriert](https://developer.microsoft.com/store/register) sein.

Sie müssen die [Prüfliste für die App-Übermittlung](../publish/app-submissions.md) verwenden.

Die App muss zur [Zertifizierung](../publish/the-app-certification-process.md) eingereicht werden.

Weitere Informationen findest du unter [Veröffentlichen von Windows-Apps und -Spielen](../publish/index.md).