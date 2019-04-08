---
title: Erstellen eines UWP-Spiels in MonoGame-2D
description: Eine einfache UWP-Spiel für den Microsoft Store, geschrieben C# und "MonoGame"
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: dbd2c6c9f5e3cf2200f9b260687f05718178868a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619155"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Erstellen eines UWP-Spiels in MonoGame-2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Ein einfaches 2D-UWP-Spiel für den Microsoft Store, geschrieben in C# und MonoGame


![Sprite-Blatt für laufenden Dino](images/JS2D_0.png)

## <a name="introduction"></a>Einführung

MonoGame ist ein einfaches Framework für die Spieleentwicklung. In diesem Lernprogramm lernen Sie die Grundlagen der Entwicklung von Spielen in MonoGame kennen und erhalten z. B. Informationen zum Laden von Inhalten, Zeichnen und Animieren von Sprites und Behandeln von Benutzereingaben. Außerdem werden einige erweiterte Konzepte wie die Erkennung von Kollisionen und die Skalierung von Bildschirmen für einen hohen DPI-Wert besprochen. Dieses Lernprogramm dauert 30 bis 60 Minuten.

## <a name="prerequisites"></a>Voraussetzungen
+   Windows 10 und Microsoft Visual Studio 2017  [Klicken Sie hier, um zu erfahren, wie Sie Visual Studio einrichten](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Das .NET-Desktop-Entwicklungs-Framework. Wenn Sie dies noch nicht installiert haben, können Sie es durch das erneute Ausführen des Visual Studio-Installers und das Ändern Ihrer Installation von Visual Studio 2017 erhalten.
+   Sie sollten über Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache verfügen. [Klicken Sie hier, um Informationen zum Einstieg in C# zu erhalten](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   Kenntnisse über die grundlegenden Konzepte der Informatik, wie Klassen, Methoden und Variablen, sind ebenfalls von Vorteil.

## <a name="why-monogame"></a>Warum MonoGame?
In Umgebungen für die Spieleentwicklung gibt es ausreichend Optionen. Und das Angebot ist groß: von Engines mit vollem Funktionsumfang wie Unity bis hin zu umfassenden und komplexen Multimedia-APIs wie DirectX. Womit soll man beginnen? MonoGame ist eine Sammlung von Tools, die in ihrer Komplexität zwischen einer Game-Engine und einer ausgereiften API wie DirectX liegt. Sie bietet eine einfach zu verwendende Content-Pipeline und alle erforderlichen Funktionen, um einfache Spiele zu erstellen, die auf einer Vielzahl von Plattformen ausgeführt werden können. Alle apps geschrieben sind reine MonoGame beste C#, und Sie können sie schnell über den Microsoft Store oder andere ähnliche Verteilung Plattformen verteilen.

## <a name="get-the-code"></a>Code abrufen
Wenn Sie das Lernprogramm nicht schrittweise durchgehen, sondern nur MonoGame in Aktion sehen möchten, [klicken Sie hier, um die fertige App abzurufen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Öffnen Sie das Projekt in Visual Studio 2017, und drücken Sie **F5**, um das Beispiel auszuführen. Wenn Sie das Projekt zum ersten Mal laden, kann dies möglicherweise etwas länger dauern, da Visual Studio alle NuGet-Pakete abrufen muss, die nicht mit installiert wurden.

Wenn Sie dies getan haben, überspringen Sie den nächsten Abschnitt zum Einrichten von MonoGame, und sehen Sie sich eine schrittweise Erläuterung des Codes an.

**Hinweis:** Das Spiel, die in diesem Beispiel erstellten soll nicht abgeschlossen werden (oder einen viel Spaß beim überhaupt). Sein einzige Zweck werden die grundlegenden Konzepte der Direct2D-Entwicklung in MonoGame erläutert. Verwenden Sie diesen Code und verbessern Sie ihn – oder beginnen Sie einfach ganz von vorn, nachdem Sie die Grundlagen kennengelernt haben!

## <a name="set-up-monogame-project"></a>Einrichten des MonoGame-Projekts
1. Installieren Sie **MonoGame 3.6** für Visual Studio aus [MonoGame.net](https://www.monogame.net/).

2. Starten Sie Visual Studio 2017.

3. Wählen Sie **Datei->Neu->Projekt** aus.

4. Wählen Sie unter Visual C#-Projektvorlagen **MonoGame** und **MonoGame Windows 10 Universal-Projekt** aus.

5. Benennen Sie Ihr Projekt mit „MonoGame2D“, und klicken Sie auf OK. Wenn das Projekt erstellt wurde, ist es möglicherweise voller Fehler, die aber nach der ersten Ausführung des Projekts beseitigt sein sollten. Dabei werden alle fehlenden NuGet-Pakete installiert.

6. Stellen Sie sicher, dass **X86** und **lokaler Computer** als Zielplattform angegeben sind, und drücken Sie **F5**, um das leere Projekt zu erstellen und auszuführen. Wenn Sie die oben beschriebenen Schritte ausgeführt haben, sollte ein leeres blaues Fenster angezeigt werden, sobald das Projekt erstellt wurde.

## <a name="method-overview"></a>Übersicht über die Methoden
Nachdem Sie das Projekt erstellt haben, öffnen Sie die **Game1.cs**-Datei aus dem **Projektmappen-Explorer**. Hier wird ein Großteil der Spiellogik abgelegt. Viele wichtige Methoden werden hier automatisch generiert, wenn Sie ein neues MonoGame-Projekt erstellen. Diese Methoden sollen hier kurz erläutert werden:

**public Game1()** – Der Konstruktor. Diese Methode wird in diesem Lernprogramm nicht geändert.

**protected override void Initialize()** – Hier werden alle verwendeten Klassenvariablen initialisiert. Diese Methode wird zu Beginn des Spiels einmal aufgerufen.

**protected override void LoadContent()** – Diese Methode lädt Inhalt (z. B. Audio, Texturen, Schriftarten) in den Arbeitsspeicher, bevor das Spiel gestartet wird. Wie die Initialize-Methode wird sie beim Starten der App einmal aufgerufen.

**protected override void UnloadContent()** – Mit dieser Methode wird Inhalt entfernt, der nicht aus dem Inhalts-Manager stammt. Diese Methode wird hier nicht verwendet.

**geschützte außer Kraft setzen "void" Update (GameTime GameTime)** diese Methode wird einmal für jede Zyklus, der die spielschleife aufgerufen. Hier wird der Status der im Spiel verwendeten Objekte oder Variablen aktualisiert. Dies betrifft auch Position, Geschwindigkeit oder Farbe eines Objekts. Dies ist auch, in denen Benutzereingaben behandelt wird. Kurz gesagt, behandelt diese Methode alle Teile der Spiellogik mit Ausnahme des Zeichnens von Objekten auf den Bildschirm.

**protected override void Draw(GameTime gameTime)** – Hier werden Objekte auf den Bildschirm gezeichnet. Dabei wird die Position verwendet, die von der Update-Methode angegeben wurde.

## <a name="draw-a-sprite"></a>Zeichnen eines Sprite
Sie haben das neue MonoGame-Projekt ausgeführt und sehen einen schöne blauen Himmel. Nun fügen wird Boden hinzu.
In MonoGame werden der App 2D-Zeichnungen in Form von „Sprites“ hinzugefügt. Ein Sprite ist eine Computergrafik, die als eine Einheit bearbeitet wird. Sprites können verschoben, skaliert, geformt, animiert und kombiniert werden, sodass alles erstellt werden kann, was Sie sich im 2D-Raum vorstellen können.

### <a name="1-download-a-texture"></a>1. Laden Sie eine Textur
Für unsere Zwecke soll das erste Sprite äußerst langweilig sein. [Klicken Sie hier, um dieses einfache grüne Rechteck herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Fügen Sie die Textur, in den Inhaltsordner
- Öffnen Sie den **Projektmappen-Explorer**.
- Klicken Sie mit der rechten Maustaste im Ordner **Content** auf **Content.mgcb**, und wählen Sie **Öffnen mit** aus. Wählen Sie aus dem Popupmenü **Monogame-Pipeline** aus, und wählen Sie **OK**.
- Klicken Sie im neuen Fenster mit der rechten Maustaste auf das **Content**-Element, und wählen Sie **Hinzufügen -> Vorhandenes Element** aus.
- Suchen Sie und markieren Sie das grüne Rechteck, in dem Datei-Browser.
- Nennen Sie das Element „grass.png“, und wählen Sie **Hinzufügen** aus.

### <a name="3-add-class-variables"></a>3. Fügen Sie Klassenvariablen hinzu
Öffnen Sie zum Laden dieses Bilds als Sprite-Textur **Game1.cs**, und fügen Sie die folgenden Klassenvariablen hinzu.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

Die Variable SKYRATIO gibt an, wie viel der Szene Himmel und wie viel Gras sein soll – in diesem Fall zwei Drittel zu einem Drittel. Die Variablen **screenWidth** und **screenHeight** überwachen die Größe des App-Fensters, und in **grass** speichern wir das grüne Rechteck.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Initialisieren Sie Klassenvariablen zu, und legen Sie die Größe des Fensters
Die Variablen **screenWidth** und **screenHeight** müssen noch initialisiert werden. Fügen Sie daher der **Initialize**-Methode diesen Code hinzu:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Wir haben nun die Höhe und Breite des Bildschirms und den Ansichtsmodus der App auf **Vollbild**festgelegt. Außerdem bleibt die Maus unsichtbar.

### <a name="5-load-the-texture"></a>5. Laden Sie die Textur
Fügen Sie der **LoadContent**-Methode Folgendes hinzu, um die Textur in die grass-Variable zu laden:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Das Sprite zeichnen
Fügen Sie der **Draw**-Methode die folgenden Zeilen hinzu, um das Rechteck zu zeichnen:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Hier verwenden wir die **spriteBatch.Draw**-Methode, um die angegebene Textur innerhalb der Ränder eines Rechteckobjekts zu platzieren. Ein **Rechteck** setzt sich aus den x- und y-Koordinaten seiner oberen linken und unteren rechte Ecke zusammen. Mit den zuvor definierten Variablen **screenWidth**, **screenHeight** und **SKYRATIO** zeichnen wir die Textur des grünen Rechtecks entlang des unteren Drittels des Bildschirms. Wenn Sie das Programm ausführen, sehen Sie jetzt den blauen Hintergrund, der teilweise durch das grüne Rechteck abgedeckt ist.

![Grünes Rechteck](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Skalierung auf Bildschirme mit hohen DPI-Werten
Wenn Sie Visual Studio auf einem Monitor mit hoher Pixeldichte ausführen, wie beispielsweise auf einem Surface Pro oder Surface Studio, könnte es sein, dass das in den obigen Schritten erstellte grüne Rechteck das untere Drittel des Bildschirms nicht vollständig abdeckt. Es ist wahrscheinlich nicht an der unteren linken Ecke des Bildschirms verankert. Um dieses Problem beheben und die Oberfläche des Spiels auf allen Geräten zu vereinheitlichen, müssen wir eine Methode erstellen, mit der bestimmte Werte in Bezug zur Pixeldichte des Bildschirms skaliert werden können:

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

Als Nächstes ersetzen Sie die Initialisierung der Variablen **screenHeight** und **screenWidth"** in der **Initialize**-Methode mit Folgendem:

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

Wenn Sie einen Bildschirm mit hohen DPI-Werten verwenden und die App jetzt ausführen, sollte das grüne Rechteck das untere Drittel wie gewünscht abdecken.

## <a name="build-the-spriteclass"></a>Erstellen der SpriteClass
Bevor wir mit der Animation von Sprites beginnen, werden wir eine neue Klasse namens „SpriteClass“ erstellen. Mit ihr können wir die Komplexität der Oberflächenebene der Sprite-Manipulation reduzieren.

### <a name="1-create-a-new-class"></a>1. Erstellen Sie eine neue Klasse
Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **MonoGame2D (Universal Windows)**, und wählen Sie **Hinzufügen -> Klasse** aus. Nennen Sie die Klasse „SpriteClass.cs“, und wählen Sie dann **Hinzufügen** aus.

### <a name="2-add-class-variables"></a>2. Fügen Sie Klassenvariablen hinzu
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

Hier richten wir die Klassenvariablen ein, die wir zum Zeichnen und Animieren eines Sprite benötigen. Die Variablen **x** und **y** stellen die aktuelle Position des Sprite auf der Ebene dar, während die Variable **angle** der aktuelle Winkel des Sprite in Grad ist (0 – aufrecht, 90 – um 90 Grad im Uhrzeigersinn geneigt). Beachten Sie unbedingt, dass **x** und **y** für diese Klasse die Koordinaten des **Mittelpunkts** des Sprite darstellen (der Standardursprung befindet sich in der oberen linken Ecke). Damit wird es einfacher, Sprites zu drehen, da sie sich um einen beliebigen angegebenen Ursprung drehen. Durch die Drehung um den Mittelpunkt erhalten wir eine einheitliche Drehbewegung.

Danach haben wir **dX**, **dY** und **dA**, die die Änderungsraten der Variablen **x**, **y** und **angle** pro Sekunde darstellen.

### <a name="3-create-a-constructor"></a>3. Erstellen Sie einen Konstruktor
Beim Erstellen einer Instanz der **SpriteClass** statten wir den Konstruktor mit dem Grafikgerät aus **Game1.cs**, dem Projektordnerpfad zu der Textur und der gewünschten Skalierung der Textur in Bezug zu ihrer Originalgröße aus. Wir richten die übrigen Klassenvariablen in der Update-Methode ein, nachdem wir das Spiel gestartet haben.

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

### <a name="4-update-and-draw"></a>4. Update- und Draw
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

Die **Update** SpriteClass-Methode wird in der **Update**-Methode von Game1.cs aufgerufen. Mit ihr werden die Werte **x**, **y** und **angle** der Sprites auf der Grundlage ihrer jeweiligen Änderungsraten aktualisiert.

Die **Draw** -Methode wird in der **Draw**-Methode von Game1.cs aufgerufen. Mit ihr wird das Sprite im Fenster des Spiels gezeichnet.

## <a name="user-input-and-animation"></a>Benutzereingaben und Animation
Wir haben die SpriteClass erstellt und nutzen diese nun, um zwei neue Spielobjekte zu erstellen: zunächst einen Avatar, den der Spieler mit den Pfeiltasten und der LEERTASTE steuern kann. Die zweite ist ein Objekt, das der Spieler vermeiden muss.

### <a name="1-get-the-textures"></a>1. Abrufen von Texturen
Für den Avatar des Spielers verwenden wir die Ninja-Katze von Microsoft, die auf einem riesigen Tyrannosaurus Rex reitet. [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Nun geht es um das Hindernis, dem der Spieler ausweichen muss. Was hassen eine Ninja-Katze und ein fleischfressender Dinosaurier am meisten? Gemüse! [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Genauso wie zuvor bei dem grünen Rechteck fügen Sie diese Bilder nun über die **MonoGame-Pipeline** in **Content.mgcb** hinzu. Nennen Sie sie „ninja-cat-dino.png“ und „broccoli.png“.

### <a name="2-add-class-variables"></a>2. Fügen Sie Klassenvariablen hinzu
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
score verfolgt, wie vielen Hindernissen der Spieler erfolgreich ausgewichen ist.

Schließlich wird mit **random** dem Verhalten des Brokkoli-Hindernisses ein Grad an Zufälligkeit hinzugefügt.

### <a name="3-initialize-variables"></a>3. Initialisieren von Variablen
Als Nächstes müssen diese Variablen initialisiert werden. Fügen Sie in der Initialize-Methode den folgenden Code hinzu:

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

Beachten Sie, dass die letzten drei Variablen für Geräte mit hohen DPI-Werten skaliert werden müssen, da sie eine Änderungsrate in Pixel angegeben.

### <a name="4-construct-spriteclasses"></a>4. Erstellen von SpriteClasses
Wir erstellen SpriteClass-Objekte in der **LoadContent**-Methode. Fügen Sie dem vorhandenen Code diesen Code hinzu:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

Das Brokkoli-Bild ist viel größer, als es im Spiel angezeigt werden soll. Daher müssen wir es auf das 0,2-Fache seiner ursprünglichen Größe skalieren.

### <a name="5-program-obstacle-behaviour"></a>5. Programm Hindernis Verhalten
Der Brokkoli soll über den Bildschirm hinausreichen und in Richtung des Avatars des Spielers wandern, der dem Hindernis ausweichen muss. Um dies zu erreichen, fügen Sie diese Methode, um die **Game1.cs** Klasse:

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

Im zweiten Teil wird bestimmt, wie schnell der Brokkoli sich bewegt, wobei die Geschwindigkeit vom aktuellen Spielstand abhängt. Wenn der Spieler fünf Brokkoli ausgewichen ist, erhöht sich die Geschwindigkeit.

Der dritte Teil legt die Richtung der Bewegung des Brokkoli-Sprite fest. Wenn der Brokkoli auftaucht, bewegt er sich in Richtung des Spielers-Avatars (Dino). Er erhält außerdem den **dA**-Wert 7f, was bewirkt, dass der Brokkoli bei der Jagd nach dem Spieler durch die Luft wirbelt.

### <a name="6-program-game-starting-state"></a>6. Programmstatus Spiel ab
Bevor wir mit der Tastatureingabe fortfahren, müssen wir eine Methode angeben, mit der der Anfangszustand der beiden von uns erstellten Objekte festgelegt wird. Das Spiel soll nicht sofort gestartet werden, wenn die App ausgeführt wird. Vielmehr soll der Benutzer es manuell starten, indem er die LEERTASTE drückt. Fügen Sie den folgenden Code hinzu, mit dem der ursprüngliche Zustand der animierten Objekte festgelegt und der Punktestand zurückgesetzt wird:

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

### <a name="7-handle-keyboard-input"></a>7. Behandeln von Tastatureingaben
Als Nächstes benötigen wir eine neue Methode für den Umgang mit Benutzereingaben über die Tastatur. Fügen Sie diese Methode zur **Game1.cs**:

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

Wir sehen oben eine Reihe von vier „Wenn“-Anweisungen:

Die erste beendet das Spiel, wenn die **Escape**-Taste gedrückt wird.

Die zweite startet das Spiel, wenn die **LEERTASTE** gedrückt wird, und das Spiel ist noch nicht gestartet wurde.

Die dritte lässt den Dino-Avatar springen, wenn die **LEERTASTE** gedrückt wird. Dazu wird die **dY**-Eigenschaft geändert. Beachten Sie, die der Spieler springen kann nicht, es sei denn, sie auf "Grund"auf (dino.y "ScreenHeight" = * SKYRATIO), und wird auch nicht wechseln, wenn die LEERTASTE statt gedrückten einmal nach unten gehalten wird. Dies verhindert, dass der Dino nach Spielstart sofort springt, weil die LEERTASTE auch zum Starten des Spiels gedrückt wird.

Mit der letzten „Wenn“-Anweisung wird überprüft, ob die Pfeile nach links oder rechts gedrückt werden. Wenn dies der Fall ist, wird die **dX** -Eigenschaft des Dinos entsprechend geändert.

**Aufgabe:** Schaffen Sie es, dass die obige Tastatureingabemethode mit dem WASD-Eingabeschema genauso wie mit den Pfeiltasten funktioniert?

### <a name="8-add-logic-to-the-update-method"></a>8. Fügen Sie Logik hinzu, um die Update-Methode
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

### <a name="9-draw-spriteclass-objects"></a>9. SpriteClass Objekte zeichnen
Fügen Sie zum Schluss der **Draw**-Methode in **Game1.cs** direkt nach dem letzten Aufruf von **spriteBatch.Draw** den folgenden Code hinzu:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

In MonoGame werden neue Aufrufe von **spriteBatch.Draw** über alle vorherigen Aufrufe gezeichnet. Dies bedeutet, dass sowohl die Broccoli und das Sprite Dino über das vorhandene Gras Sprite gezeichnet werden, damit sie nie dahinter unabhängig von ihrer Position ausgeblendet werden können.

Führen Sie das Spiel nun aus, und bewegen Sie den Dino mit den Pfeiltasten und der LEERTASTE. Wenn Sie die obigen Schritte ausgeführt haben, sollten Sie Ihren Avatar, verschieben Sie im Fenster Spiels vornehmen, und die Broccoli sollten mit einer Geschwindigkeit ständig wachsenden erzeugen.

![Spieler-Avatar und Hindernis](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Rendern von Text mit SpriteFont
Mit dem obigen Code überwachen wir im Hintergrund die Punktzahl des Spielers, aber wir teilen dem Spieler das Ergebnis nicht mit. Außerdem gibt es beim Start der App eine wenig intuitive Einführung: Der Spieler sieht ein blaues und ein grünes Fenster, weiß aber nicht, dass er die LEERTASTE drücken muss, damit es losgeht.

Zur Behebung dieser Probleme nutzen wir eine neue Art von MonoGame-Objekt mit dem Namen **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Erstellen von SpriteFont Description-Dateien
Suchen Sie im **Projektmappen-Explorer** den Ordner **Content**. Klicken Sie in diesem Ordner mit der rechten Maustaste auf die Datei **Content.mgcb**, und wählen Sie **Öffnen mit** aus. Wählen Sie aus dem Popupmenü **MonoGame-Pipeline** aus, und drücken Sie dann auf **OK**. Klicken Sie im neuen Fenster mit der rechten Maustaste auf das **Content**-Element, und wählen Sie **Hinzufügen -> Neues Element** aus. Wählen Sie **SpriteFont-Beschreibung** aus, benennen Sie sie mit „Score“, und drücken Sie **OK**. Dann fügen Sie auf die gleiche Weise eine andere SpriteFont-Beschreibung mit dem Namen „GameState“ hinzu.

### <a name="2-edit-descriptions"></a>2. Eine Beschreibung bearbeiten
Klicken Sie mit der rechten Maustaste auf den **Content**-Ordner in der **MonoGame-Pipeline**, und wählen Sie **Dateispeicherort öffnen** aus. Ein Ordner mit der soeben erstellten SpriteFont-Beschreibung sollte angezeigt werden. Er sollte zudem alle Bilder enthalten, die Sie dem Content-Ordner bisher hinzugefügt haben. Sie können jetzt das Fenster „MonoGame-Pipeline“ speichern und schließen. Öffnen Sie aus dem **Datei-Explorer** heraus beide Beschreibungsdateien in einem Text-Editor (Visual Studio, Notepad++, Atom usw.).

Jede Beschreibung enthält eine Reihe von Werten, die die SpriteFont beschreiben. Wir werden einige Änderungen vornehmen:

Ändern Sie in **Score.spritefont** den **<Size>**-Wert von 12 auf 36.

Ändern Sie in **GameState.spritefont** den **<Size>**-Wert von 12 auf 72 und den **<FontName>**-Wert von Arial in Agency. Agency ist eine weitere Schriftart, die standardmäßig auf Windows 10-Computern bereitgestellt wird und dem Begrüßungsbildschirm mehr Flair verleiht.

### <a name="3-load-spritefonts"></a>3. Laden Sie SpriteFonts
Zurück in Visual Studio fügen wir zuerst eine neue Textur für den Begrüßungsbildschirm hinzu. [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Fügen Sie wie zuvor die Textur zum Projekt hinzu, indem Sie mit der rechten Maustaste auf „Content“ klicken und **Hinzufügen -> Vorhandenes Element** auswählen. Nennen Sie das neue Element „start-splash.png“.

Als Nächstes fügen Sie die folgenden Klassenvariablen in **Game1.cs** hinzu:

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

### <a name="4-draw-the-score"></a>4. Zeichnen Sie die Bewertung
Wechseln Sie in die **Draw**-Methode von **Game1.cs**, und fügen Sie unmittelbar vor **spriteBatch.End();** den folgenden Code hinzu.

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

Der obige Code verwendet die Sprite-Beschreibung, die wir erstellt haben (Arial Größe 36), um den Punktestand des Spielers in der Nähe der oberen rechten Ecke des Bildschirms zu zeichnen.

### <a name="5-draw-horizontally-centered-text"></a>5. Horizontal zentriert Zeichnen von text
Wenn Sie ein Spiel erstellen, möchten Sie häufig Text zeichnen, die entweder horizontal oder vertikal zentriert ist. Wenn Sie den Einführungstext horizontal zentrieren möchten, fügen Sie der **Draw**-Methode unmittelbar vor dem **spriteBatch.End();** diesen Code hinzu.

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

Zunächst erstellen wir zwei Zeichenfolgen, eine für jede zu zeichnende Textzeile. Als Nächstes messen wird die Breite und Höhe jeder Zeile beim Drucken mit der **SpriteFont.MeasureString(String)**-Methode. Dadurch können wir die Größe als **Vector2**-Objekt mit der **X**-Eigenschaft für die Breite und der **Y**-Eigenschaft für die Höhe angeben.

Zuletzt zeichnen wir die einzelnen Zeilen. Um den Text Horizontal zentrieren, stellen wir die **X** Wert, der dessen Position Vektor gleich **"ScreenWidth" / 2 - textSize.X / 2**.

**Aufgabe:** Was müssen Sie tun, damit der obige Text vertikal und horizontal zentriert wird?

Starten Sie das Spiel. Sehen Sie den Begrüßungsbildschirm? Wird die Punktezahl mit jedem Ausweichen vor einem Brokkoli erhöht?

![Begrüßungsbildschirm](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Erkennung von Kollisionen
Nun haben wir einen Brokkoli, der den Spieler verfolgt. Außerdem gibt es eine Punktzahl, die sich immer dann erhöht, wenn der Spieler ausweicht. Aber im Moment gibt es noch keinen Weg, das Spiel auch zu verlieren. Wir müssen wissen, ob das Dino-Sprite und das Brokkoli-Sprite kollidieren, und falls sie dies tun, muss das Spiel als beendet erklärt werden.

### <a name="1-get-the-textures"></a>1. Abrufen von Texturen
Das letzte Abbild, die wir benötigen, ist eine für "game Over". [Klicken Sie hier, um das Bild herunterzuladen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/game-over.png).

Wie zuvor mit dem grünen Rechteck "Ninja"-Cat und Broccoli, Hinzufügen von Bildern dieses Image in **Content.mgcb** über die **MonoGame Pipeline**, nennen Sie es "Game-over.png".

### <a name="2-rectangular-collision"></a>2. Rechteckige Konflikt
Beim Erkennen von Kollisionen in einem Spiel werden Objekte oft vereinfacht, damit die nötigen Berechnungen nicht allzu kompliziert werden. Wir behandeln sowohl den Avatar des Spielers als auch das Brokkoli-Hindernis als Rechtecke, um eine Kollision dieser Objekte erkennen zu können.

Öffnen Sie **SpriteClass.cs**, und fügen Sie eine neue Klassenvariable hinzu:

```CSharp
const float HITBOXSCALE = .5f;
```

Dieser Wert stellt dar, inwieweit dem Spieler eine erkannte Kollision „verziehen“ wird. Mit dem Wert .5f werden die Kanten des Rechtecks, in dem der Dino mit dem Brokkoli kollidieren kann – häufig auch „Hitbox“ genannt – auf die Hälfte der Gesamtgröße der Textur verkleinert. Damit können die Ecken der beiden Texturen in wenigen Fällen kollidieren, ohne dass Teile des Bildes tatsächlich sichtbar kollidieren. Passen Sie diesen Wert nach Bedarf an.

Fügen Sie dann eine neue Methode zu **SpriteClass.cs** hinzu:

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

Diese Methode bestimmt, ob zwei rechteckige Objekte kollidiert sind. Der Algorithmus funktioniert, indem Sie testen, um festzustellen, ob eine Lücke zwischen den Seiten der Rechtecke vorhanden ist. Wenn es eine Lücke gibt, ist keine Kollision erfolgt – wenn keine Lücke vorhanden ist, muss eine Kollision vorliegen.

### <a name="3-load-new-textures"></a>3. Laden Sie neue Strukturen

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

### <a name="4-implement-game-over-logic"></a>4. Implementieren von "über" Spiellogik
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

Dadurch werden nach Spielende alle Bewegungen beendet, und das Dino- und das Brokkoli-Sprite werden in ihrer aktuellen Position fixiert.

Als Nächstes fügen Sie am Ende der **Update**-Methode unmittelbar vor **base.Update(gameTime)** diese Zeile ein:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Dadurch wird die **RectangleCollision**-Methode aufgerufen, die wir in **SpriteClass** erstellt haben, und das Spiel als „beendet“ markiert, wenn der Wert „true“ zurückgegeben wird.

### <a name="5-add-user-input-for-resetting-the-game"></a>5. Hinzufügen von Benutzereingaben für das Zurücksetzen des Spiels
Fügen Sie folgenden Code, der **KeyboardHandler** Methode, die der Benutzer das Spiel zurücksetzen, wenn sie die EINGABETASTE drücken:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="6-draw-game-over-splash-and-text"></a>6. Zeichnen-Befehl das Spiel ist beendet Begrüßungsbildschirm und text
Abschließend fügen Sie der Draw-Methode direkt nach dem ersten Aufruf von **spriteBatch.Draw** (dies sollte der Aufruf sein, mit dem die Gras-Textur gezeichnet wird) diesen Code hinzu.

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

Hier verwenden wir dieselbe Methode wie zuvor zum Zeichnen von horizontal zentriertem Text, wobei wir wieder die Schriftart aus dem Begrüßungsbildschirm verwenden. Außerdem wird **gameOverTexture** in der oberen Hälfte des Fensters zentriert.

Nun sind Sie fertig. Starten Sie das Spiel erneut. Wenn Sie die oben beschriebenen Schritte ausgeführt haben, sollte das Spiel jetzt beendet werden, wenn der Dino mit dem Brokkoli kollidiert. Dann sollte der Spieler zum Neustarten des Spiels durch Drücken der EINGABETASTE aufgefordert werden.

![Spielende](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Veröffentlichen im Microsoft Store
Da wir das Spiel als UWP-app erstellt haben, ist es möglich, dieses Projekt in der Microsoft Store zu veröffentlichen. Dazu müssen Sie einige Schritte durchführen.

Sie müssen als Windows-Entwickler [registriert](https://developer.microsoft.com/en-us/store/register) sein.

Sie müssen die [Prüfliste für App-Übermittlung](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions) verwenden.

Die App muss zur [Zertifizierung](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process) eingereicht werden.

Weitere Informationen finden Sie unter [Veröffentlichen Ihrer UWP-app](https://developer.microsoft.com/en-us/store/publish-apps).
