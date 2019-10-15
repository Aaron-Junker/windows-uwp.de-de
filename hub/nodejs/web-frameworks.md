---
title: Einstieg in Node. js-Webframe Works unter Windows
description: Eine Anleitung für den Einstieg in Node. js-Webframe Works unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, Node on Windows, Node on WSL, Node on Linux on Windows, install Node on Windows, NodeJS with vs Code, Develop with Node on Windows, Develop with NodeJS on Windows, install Node on WSL, NodeJS on Windows Subsystem für Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a3c1cd980884dc50107c05207665d0c1ef88938e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314954"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Einstieg in Node. js-Webframe Works unter Windows

Eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Node. js-Webframe Works unter Windows, einschließlich Next. js, nuxt. js und Gatsby.

## <a name="prerequisites"></a>Erforderliche Komponenten

In dieser Anleitung wird davon ausgegangen, dass Sie bereits die Schritte zum [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md)abgeschlossen haben, einschließlich:

- Installieren Sie Windows 10 Insider Preview Build 18932 oder höher.
- Aktivieren Sie das WSL 2-Feature unter Windows.
- Installieren Sie eine Linux-Distribution (Ubuntu 18,04 für unsere Beispiele). Sie können Folgendes überprüfen: `wsl lsb_release -a`
- Stellen Sie sicher, dass die Ubuntu 18,04-Distribution im WSL 2-Modus ausgeführt wird. (WSL kann Verteilungen im v1-oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`
- Legen Sie mithilfe von PowerShell Ubuntu 18,04 als Standardverteilung fest, mit: `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Beginnen Sie mit "Next. js"

Next. js ist ein Framework zum Erstellen von Server gerenderten JavaScript-apps auf der Grundlage von " Es handelt sich im Grunde um ein Projekt Bausteine für die Reaktion, das mit Aufmerksamkeit auf bewährte Methoden entwickelt wurde und es Ihnen ermöglicht, universelle Webanwendungen auf einfache und konsistente Weise zu erstellen, ohne dass eine Konfiguration möglich ist. Diese "universellen" Server gerenderten Web-Apps werden manchmal auch als "isomorph" bezeichnet, was bedeutet, dass der Code zwischen Client und Server freigegeben wird.

So erstellen Sie ein "Next. js"-Projekt, das die Installation von "Next", "Reaktions" und "" "

1. Öffnen Sie das WSL-Terminal (d.h. Ubuntu 18,04).

2. Erstellen Sie einen neuen Projektordner: `mkdir NextProjects`, und geben Sie das Verzeichnis ein: `cd NextProjects`.

3. Installieren Sie Next. js, und erstellen Sie ein Projekt (ersetzen Sie "My-Next-app" durch das, was Sie für Ihre APP wünschen): `npm create next-app my-next-app`.

4. Nachdem das Paket installiert wurde, ändern Sie die Verzeichnisse in ihren neuen App-Ordner, `cd my-next-app`, und verwenden Sie dann `code .`, um das nächste. js-Projekt in vs Code zu öffnen. Auf diese Weise können Sie sich das nächste js-Framework ansehen, das für Ihre APP erstellt wurde. Beachten Sie, dass vs Code Ihre APP in einer WSL-Remote Umgebung geöffnet hat (wie auf der Registerkarte "grün" unten links im Fenster "vs Code" angezeigt). Dies bedeutet, dass während der Verwendung von vs Code zur Bearbeitung im Windows-Betriebssystem die APP weiterhin unter dem Linux-Betriebssystem ausgeführt wird.

    ![WSL-Remote Erweiterung](../images/wsl-remote-extension.png)

5. Es gibt drei Befehle, die Sie nach der Installation von "Next. js" kennen müssen:

    - `npm run dev` für das Ausführen einer Entwicklungs Instanz mit einem aktiven Reload, der Dateiüberwachung und der erneuten Ausführung der Aufgabe.
    - `npm run build` zum Kompilieren des Projekts.
    - `npm start` zum Starten der APP im Produktionsmodus.

    Öffnen Sie das in vs Code integrierte WSL-Terminal ( **> Terminal anzeigen**). Stellen Sie sicher, dass der Terminal Pfad auf das Projektverzeichnis verweist (IE. `~/NextProjects/my-next-app$`). Führen Sie dann eine Entwicklungs Instanz Ihrer neuen Next. js-App mit folgenden Aktionen aus: `npm run dev`

6. Der lokale Entwicklungs Server wird gestartet, und sobald Ihre Projektseiten erstellt wurden, wird in Ihrem Terminal "kompiliert erfolgreich auf [http://localhost:3000](http://localhost:3000)" angezeigt. Wählen Sie diesen localhost-Link aus, um die neue Next. js-app in einem Webbrowser zu öffnen.

    ![Ihre nächste js-APP wird auf "localhost: 3000" ausgeführt.](../images/next-app.png)

7. Öffnen Sie die Datei `pages/index.js` in Ihrem vs Code-Editor. Suchen Sie den Seitentitel `<h1 className='title'>Welcome to Next.js!</h1>`, und ändern Sie ihn in `<h1 className='title'>This is my new Next.js app!</h1>`. Wenn Ihr Webbrowser weiterhin für "localhost: 3000" geöffnet ist, speichern Sie die Änderung, und beachten Sie, dass das Feature "Hot-Reload" automatisch kompiliert wird und die Änderung im Browser aktualisiert.

8. Sehen wir uns an, wie "Next. js" Fehler behandelt. Entfernen Sie das schließende Tag `</h1>`, sodass der Titel Code nun wie folgt aussieht: `<h1 className='title'>This is my new Next.js app!`. Speichern Sie diese Änderung, und beachten Sie, dass der Fehler "Fehler bei der Kompilierung" in Ihrem Browser angezeigt wird, und in Ihrem Terminal, dass Sie wissen, dass ein Endtag für `<h1>` erwartet wird. Ersetzen Sie das schließende Tag `</h1>` und speichern, und die Seite wird erneut geladen.

Sie können den Debugger vs Code mit ihrer Next. js-App verwenden, indem Sie die F5-Taste drücken, oder indem Sie in der Menüleiste zum **anzeigen > Debuggen** (STRG + UMSCHALT + D) und zum **anzeigen > Debugging-Konsole** (STRG + UMSCHALT + Y) navigieren. Wenn Sie das Zahnrad Symbol im Debugfenster auswählen, wird eine Start Konfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debuggen speichern können. Weitere Informationen finden Sie unter [vs Code Debuggen](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code Fenster "Debuggen" und "Launch. JSON config"](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu "Next. js" finden Sie in der " [Next. js](https://nextjs.org/docs)"-Dokumentation.

## <a name="get-started-with-nuxtjs"></a>Beginnen Sie mit "nuxt. js"

Nuxt. js ist ein Framework zum Erstellen von Server gerenderten JavaScript-apps auf der Grundlage von Vue. js, Node. js, WebPack und Babel. js. Es wurde von "Next. js" inspiriert. Es handelt sich im Grunde um ein Projekt Bausteine für Vue. Ebenso wie "Next. js" ist es mit Aufmerksamkeit auf bewährte Methoden konzipiert und ermöglicht es Ihnen, "universelle" Web-Apps auf einfache und konsistente Weise zu erstellen, ohne dass eine Konfiguration möglich ist. Diese "universellen" Server gerenderten Web-Apps werden manchmal auch als "isomorph" bezeichnet, was bedeutet, dass der Code zwischen Client und Server freigegeben wird.

Zum Erstellen eines nuxt. js-Projekts, das eine Reihe von Fragen zu der Art des integrierten serverseitigen Frameworks, des Benutzeroberflächen-Frameworks, des Test-Frameworks, des Modus, der Module und der Linter enthält, die Sie installieren möchten:

1. Öffnen Sie das WSL-Terminal (d.h. Ubuntu 18,04).

2. Erstellen Sie einen neuen Projektordner: `mkdir NuxtProjects`, und geben Sie das Verzeichnis ein: `cd NuxtProjects`.

3. Installieren Sie nuxt. js, und erstellen Sie ein Projekt (ersetzen Sie "My-nuxt-app" durch das, was Sie für Ihre APP aufrufen möchten): `npm create nuxt-app my-nuxt-app`

4. Der Installer von nuxt. js stellt Ihnen nun die folgenden Fragen:
    - Projekt Name: My-nuxtjs-App
    - Projektbeschreibung: Beschreibung der nuxt. js-app.
    - Name des Autors: Ich verwende meinen GitHub-Alias.
    - Wählen Sie den Paket-Manager aus: Yarn oder **NPM** : Wir verwenden NPM als Beispiele.
    - UI-Framework auswählen: None, Ant Design Vue, Bootstrap Vue usw. Wählen Sie für dieses Beispiel " **vuetify** ", aber die Vue-Community hat eine gute [Zusammenfassung zum Vergleichen dieser Benutzeroberflächen-Frameworks](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) erstellt, damit Sie die beste Lösung für Ihr Projekt auswählen können.
    - Benutzerdefinierte Server-Frameworks auswählen: Keine, adonisjs, Express, fastify usw. Wählen Sie für dieses Beispiel **keine** aus, aber Sie finden einen [2019-2020-Server Framework-Vergleich](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) auf der dev.to-Website.
    - Wählen Sie nuxt. js-Module (Leertaste verwenden, um Module auszuwählen, oder geben Sie einfach ein, wenn Sie keine wünschen): Axios (zur Vereinfachung von HTTP-Anforderungen) oder [PWA-Unterstützung](https://pwa.nuxtjs.org/) (zum Hinzufügen einer Service-Worker-, Manifest. JSON-Datei usw.). Fügen Sie für dieses Beispiel kein Modul hinzu.
    - Auswählen von linting-Tools: **Eslint**, Prettier, bereitgestellte Dateien. Wir wählen **eslint** (ein Tool zum Analysieren Ihres Codes und warnen Sie von möglichen Fehlern).
    - Wählen Sie ein Test Framework aus: **None**, jest, AVA. Wählen Sie **keine** aus, da die Tests in diesem Schnellstart nicht behandelt werden.
    - Renderingmodus auswählen: **Universal (SSR)** oder Single Page app (Spa). Wählen Sie für unser Beispiel **Universal (SSR)** aus, aber die [nuxt. js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) -Dokumentation zeigt einige der Unterschiede an: die-SSR, die erfordert, dass ein Node. js-Server auf dem Server ausgeführt wird, und Renderen Ihre APP und Spa für statisches Hosting.
    - Wählen Sie Entwicklungs Tools: **jsconfig. JSON** (empfohlen für vs Code, damit die IntelliSense-Code Vervollständigung funktioniert)

5. Nachdem das Projekt erstellt wurde, `cd my-nuxtjs-app`, um Ihr nuxt. js-Projektverzeichnis einzugeben, und geben Sie dann `code .` ein, um das Projekt in der vs Code WSL-Remote Umgebung zu öffnen.

    ![WSL-Remote Erweiterung](../images/wsl-remote-extension.png)

6. Es gibt drei Befehle, die Sie nach der Installation von "nuxt. js" wissen müssen:

    - `npm run dev` für das Ausführen einer Entwicklungs Instanz mit einem aktiven Reload, der Dateiüberwachung und der erneuten Ausführung der Aufgabe.
    - `npm run build` zum Kompilieren des Projekts.
    - `npm start` zum Starten der APP im Produktionsmodus.

    Öffnen Sie das in vs Code integrierte WSL-Terminal ( **> Terminal anzeigen**). Stellen Sie sicher, dass der Terminal Pfad auf das Projektverzeichnis verweist (IE. `~/NuxtProjects/my-nuxt-app$`). Führen Sie dann eine Entwicklungs Instanz Ihrer neuen nuxt. js-App mit folgenden Aktionen aus: `npm run dev`

6. Der lokale Entwicklungs Server wird gestartet (es werden einige kalte Status leisten für die Client-und Server Kompilierungen angezeigt). Nachdem das Projekt erstellt wurde, wird in Ihrem Terminal die Kompilierung erfolgreich angezeigt, und es wird gezeigt, wie lange die Kompilierung gedauert hat. Zeigen Sie im Webbrowser auf [http://localhost:3000](http://localhost:3000) , um die neue nuxt. js-APP zu öffnen.

    ![Ihre nuxt. js-APP, die auf "localhost: 3000" ausgeführt wird](../images/nuxt-app.png)

7. Öffnen Sie die Datei `pages/index.vue` in Ihrem vs Code-Editor. Suchen Sie den Seitentitel `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>`, und ändern Sie ihn in `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Wenn Ihr Webbrowser weiterhin für "localhost: 3000" geöffnet ist, speichern Sie die Änderung, und beachten Sie, dass das Feature "Hot-Reload" automatisch kompiliert wird und die Änderung im Browser aktualisiert.

8. Sehen wir uns an, wie "nuxt. js" Fehler behandelt. Entfernen Sie das schließende Tag `</v-card-title>`, sodass der Titel Code nun wie folgt aussieht: `<v-card-title class="headline">This is my new Nuxt.js app!`. Speichern Sie diese Änderung, und beachten Sie, dass ein Kompilierungsfehler in Ihrem Browser angezeigt wird, und in Ihrem Terminal, dass Sie wissen, dass ein Endtag für `<v-card-title>` fehlt, zusammen mit den Zeilennummern, in denen der Fehler im Code zu finden ist. Ersetzen Sie das schließende Tag `</v-card-title>` und speichern, und die Seite wird erneut geladen.

Sie können den Debugger vs Code mit ihrer nuxt. js-App verwenden, indem Sie die F5-Taste drücken, oder indem Sie in der Menüleiste zum **anzeigen > Debuggen** (STRG + UMSCHALT + D) und zum **anzeigen > Debugging-Konsole** (STRG + UMSCHALT + Y) navigieren. Wenn Sie das Zahnrad Symbol im Debugfenster auswählen, wird eine Start Konfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debuggen speichern können. Weitere Informationen finden Sie unter [vs Code Debuggen](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code Fenster "Debuggen" und "Launch. JSON config"](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu nuxt. js finden Sie im [Handbuch zu nuxt. js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Beginnen Sie mit "Gatsby. js"

"Gatsby. js" ist ein statisches Site Generator-Framework, das auf "" "" "" "" "" Ein statischer Site Generator generiert eine statische HTML-Datei zur Buildzeit. Es ist kein Server erforderlich. Next. js und nuxt. js generieren HTML zur Laufzeit (bei jedem Eintreffen einer neuen Anforderung). Sie müssen einen Server ausführen. Gatsby legt außerdem fest, wie Daten in Ihrer APP (mit graphql) behandelt werden, während "Next. js" und "nuxt. js" die Entscheidung überlassen.

So erstellen Sie ein Gatsby. js-Projekt:

1. Öffnen Sie das WSL-Terminal (d.h. Ubuntu 18,04).
2. Erstellen Sie einen neuen Projektordner: `mkdir GatsbyProjects`, und geben Sie das Verzeichnis ein: `cd GatsbyProjects`
3. Verwenden Sie NPM, um die Gatsby CLI zu installieren: `npm install -g gatsby-cli`. Überprüfen Sie nach der Installation die Version mit `gatsby --version`.
4. Erstellen Sie das Gatsby. js-Projekt: `gatsby new my-gatsby-app`
5. Nachdem das Paket installiert wurde, ändern Sie die Verzeichnisse in ihren neuen App-Ordner, `cd my-gatsby-app`, und verwenden Sie dann `code .`, um das Gatsby-Projekt in vs Code zu öffnen. Auf diese Weise können Sie das "Gatsby. js"-Framework überprüfen, das mit dem Datei-Explorer von vs Code für Ihre APP erstellt wurde. Beachten Sie, dass vs Code Ihre APP in einer WSL-Remote Umgebung geöffnet hat (wie auf der Registerkarte "grün" unten links im Fenster "vs Code" angezeigt). Dies bedeutet, dass während der Verwendung von vs Code zur Bearbeitung im Windows-Betriebssystem die APP weiterhin unter dem Linux-Betriebssystem ausgeführt wird.

    ![WSL-Remote Erweiterung](../images/wsl-remote-extension.png)

6. Es gibt drei Befehle, die Sie nach der Installation von Gatsby wissen müssen:

    - `gatsby develop` für das Ausführen einer Entwicklungs Instanz mit einem aktiven erneuten Laden.
    - `gatsby build` zum Erstellen eines Produktionsbuilds.
    - `gatsby serve` zum Starten der APP im Produktionsmodus.

    Öffnen Sie das in vs Code integrierte WSL-Terminal ( **> Terminal anzeigen**). Stellen Sie sicher, dass der Terminal Pfad auf das Projektverzeichnis verweist (IE. `~/GatsbyProjects/my-gatsby-app$`). Führen Sie dann eine Entwicklungs Instanz Ihrer neuen App mit folgenden Aktionen aus: `gatsby develop`

7. Sobald die Kompilierung des neuen Gatsby-Projekts abgeschlossen ist, zeigt das Terminal an, dass Sie jetzt Gatsby-Starter-default im Browser anzeigen können. [http://localhost:8000/](http://localhost:8000/). " Wählen Sie diesen localhost-Link aus, um das neue Projekt anzuzeigen, das in einem Webbrowser erstellt wurde.

> [!NOTE]
> Sie werden feststellen, dass Sie in ihrer Terminal Ausgabe auch wissen, dass Sie graphiql, eine in-Browser-IDE anzeigen können, um die Daten und das Schema Ihrer Website zu untersuchen: [http://localhost:8000/___graphql](http://localhost:8000/___graphql). " Graphql konsolidiert Ihre APIs in eine selbst dokumentierende IDE (graphiql), die in Gatsby integriert ist. Zusätzlich zum Untersuchen der Daten und des Schemas Ihrer Site können Sie graphql-Vorgänge wie Abfragen, Mutationen und Abonnements ausführen. Weitere Informationen finden Sie unter [Einführung von graphiql](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Öffnen Sie die Datei `src/pages/index.js` in Ihrem vs Code-Editor. Suchen Sie den Seitentitel `<h1 >Hi people</h1>`, und ändern Sie ihn in `<h1 >Hi (Your Name)!</h1>`. Wenn Ihr Webbrowser weiterhin für localhost: 8000 geöffnet ist, speichern Sie die Änderung, und beachten Sie, dass das Feature zum automatischen erneuten Laden automatisch kompiliert wird und die Änderung im Browser aktualisiert.

    ![Ihre Gatsby. js-APP wird auf "localhost: 3000" ausgeführt.](../images/gatsby-app.png)

9. Sehen wir uns an, wie "Next. js" Fehler behandelt. Entfernen Sie das schließende Tag `</h1>`, sodass der Titel Code nun wie folgt aussieht: `<h1>Hi (Your Name)!`. Speichern Sie diese Änderung, und beachten Sie, dass der Fehler "Fehler bei der Kompilierung" in Ihrem Browser angezeigt wird, und in Ihrem Terminal, dass Sie wissen, dass ein Endtag für `<h1>` erwartet wird. Ersetzen Sie das schließende Tag `</h1>` und speichern, und die Seite wird erneut geladen.

Sie können den Debugger vs Code mit ihrer Next. js-App verwenden, indem Sie die F5-Taste drücken, oder indem Sie in der Menüleiste zum **anzeigen > Debuggen** (STRG + UMSCHALT + D) und zum **anzeigen > Debugging-Konsole** (STRG + UMSCHALT + Y) navigieren. Wenn Sie das Zahnrad Symbol im Debugfenster auswählen, wird eine Start Konfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debuggen speichern können. Weitere Informationen finden Sie unter [vs Code Debuggen](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code Fenster "Debuggen" und "Launch. JSON config"](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu Gatsby finden Sie in der [Tabelle "Gatsby. js](https://www.gatsbyjs.org/docs/)".
