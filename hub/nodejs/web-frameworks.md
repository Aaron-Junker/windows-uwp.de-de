---
title: Erste Schritte mit Node.js-Webframeworks unter Windows
description: Ein Leitfaden, der Sie beim Einstieg in Node.js-Webframeworks unter Windows unterstützt.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, Erlernen von Node.js, Node unter Windows, Node im WSL, Node unter Linux unter Windows, Installieren von Node unter Windows, NodeJS mit VS Code, Entwickeln mit Node unter Windows, Entwickeln mit NodeJS unter Windows, Installieren von Node im WSL, NodeJS im Windows-Subsystem für Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517787"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Erste Schritte mit Node.js-Webframeworks unter Windows

Ein Schritt-für-Schritt-Leitfaden für den Einstieg in die Verwendung von Node.js-Webframeworks unter Windows, einschließlich Next.js, Nuxt.js und Gatsby.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass Sie bereits die Schritte zum [Einrichten der Node.js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md) abgeschlossen haben. Dazu gehören:

- Installieren von Windows 10 Insider Preview Build 18932 oder höher
- Aktivieren des WSL 2-Features unter Windows
- Installieren einer Linux-Distribution (Ubuntu 18.04 für unsere Beispiele) Dies können Sie mit `wsl lsb_release -a` überprüfen.
- Achten Sie darauf, dass die Distribution Ubuntu 18.04 im WSL 2-Modus ausgeführt wird. (WSL kann Distributionen im V1- oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`.
- Legen Sie in PowerShell mit `wsl -s ubuntu 18.04` Ubuntu 18.04 als Standarddistribution fest.

## <a name="get-started-with-nextjs"></a>Erste Schritte mit Next.js

Next.js ist ein Framework zum Erstellen von servergerenderten JavaScript-Apps auf der Grundlage von React.js, Node.js, Webpack und Babel.js. Es handelt sich im Grunde um einen Projekt-Codebaustein für React, der mit dem Augenmerk auf bewährte Methoden entwickelt wurde und es Ihnen ermöglicht, „universelle“ Webanwendungen auf einfache und konsistente Weise zu erstellen, wozu kaum Konfiguration erforderlich ist. Diese „universellen“ servergerenderten Web-Apps werden auch manchmal als „isomorph“ bezeichnet, was bedeutet, dass der Code zwischen Client und Server geteilt wird.

So erstellen Sie ein Next.js-Projekt, was die Installation von next, react und react-dom einschließt:

1. Öffnen Sie das WSL-Terminal (d. h. Ubuntu 18.04).

2. Erstellen Sie mit `mkdir NextProjects` einen neuen Projektordner, und wechseln Sie mit `cd NextProjects` in das Verzeichnis.

3. Installieren Sie Next.js, und erstellen Sie ein Projekt (und ersetzen Sie dabei 'my-next-app' durch jeden beliebigen Namen für Ihre App): `npm create next-app my-next-app`.

4. Nachdem das Paket installiert wurde, wechseln Sie in Ihren neuen App-Ordner `cd my-next-app`, und verwenden Sie dann `code .`, um das Next.js-Projekt in VS Code zu öffnen. Auf diese Weise können Sie sich das Next.js-Framework ansehen, das für Ihre App erstellt wurde. Beachten Sie, dass VS Code Ihre App in einer WSL-Remote-Umgebung geöffnet hat (worauf die grüne Registerkarte unten links in Ihrem VS Code-Fenster hinweist). Dies bedeutet, dass Sie zwar VS Code für die Bearbeitung unter dem Windows-Betriebssystem verwenden, Ihre App aber weiterhin unter dem Linux-Betriebssystem ausgeführt wird.

    ![WSL-Remote-Erweiterung](../images/wsl-remote-extension.png)

5. Diese 3 Befehle müssen Sie kennen, sobald Next.js installiert ist:

    - `npm run dev` zum Ausführen einer Entwicklungsinstanz mit Hot Reloading, Dateiüberwachung und erneuter Taskausführung.
    - `npm run build` zum Kompilieren Ihres Projekts.
    - `npm start` zum Starten Ihrer App im Produktionsmodus.

    Öffnen Sie das in VS Code integrierte WSL-Terminal (**Ansicht > Terminal**). Vergewissern Sie sich, dass der Terminalpfad auf das Projektverzeichnis verweist (d. h. `~/NextProjects/my-next-app$`). Versuchen Sie dann, mithilfe von `npm run dev` eine Entwicklungsinstanz Ihrer neuen Next.js-App auszuführen.

6. Der lokale Entwicklungsserver wird gestartet, und sobald die Erstellung Ihrer Projektseiten abgeschlossen wurde, wird in Ihrem Terminal „erfolgreich kompiliert – auf [http://localhost:3000](http://localhost:3000) bereit“ angezeigt. Wählen Sie diesen localhost-Link aus, um Ihre neue Next.js-App in einem Webbrowser zu öffnen.

    ![Ihre Next.js-App bei der Ausführung auf localhost:3000](../images/next-app.png)

7. Öffnen Sie die `pages/index.js`-Datei im VS Code-Editor. Suchen Sie nach dem Seitentitel `<h1 className='title'>Welcome to Next.js!</h1>`, und ändern Sie ihn in `<h1 className='title'>This is my new Next.js app!</h1>`. Speichern Sie bei immer noch in localhost:3000 geöffnetem Webbrowser Ihre Änderung, und beachten Sie, wie das Hot Reloading-Feature Ihre Änderung automatisch kompiliert und Ihre Änderung im Browser aktualisiert.

8. Betrachten wir nun, wie Next.js mit Fehlern umgeht. Entfernen Sie das Endtag `</h1>`, sodass Ihr Titelcode nun so aussieht: `<h1 className='title'>This is my new Next.js app!`. Speichern Sie diese Änderung, und beachten Sie, dass der Fehler „Fehler bei der Kompilierung“ im Browser sowie in Ihrem Terminal angezeigt wird, um Sie zu informieren, dass für `<h1>` ein Endtag erwartet wird. Ersetzen Sie das `</h1>`-Endtag, und speichern Sie, dann wird die Seite erneut geladen.

Sie können den Debugger von VS Code mit Ihrer Next.js-App verwenden, indem Sie die F5-Taste drücken oder in der Menüleiste zu **Ansicht > Debuggen** (STRG+UMSCHALT+D) und **Ansicht > Debugging-Konsole** (STRG+UMSCHALT+Y) wechseln. Wenn Sie das Zahnradsymbol im Debugfenster auswählen, wird eine Startkonfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debugsetup speichern können. Weitere Informationen finden Sie unter [Debuggen in VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![VS Code-Fenster „Debuggen“ und Symbol „launch.json konfigurieren“](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu Next.js finden Sie in der [Next.js-Dokumentation](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Erste Schritte mit Nuxt.js

Nuxt.js ist ein Framework zum Erstellen von servergerenderten JavaScript-Apps auf der Grundlage von Vue.js, Node.js, Webpack und Babel.js. Es wurde von Next.js inspiriert. Es handelt sich im Grunde um einen Projekt-Codebaustein für Vue. Ebenso wie Next.js wurde es mit dem Augenmerk auf bewährte Methoden entwickelt und ermöglicht es Ihnen, „universelle“ Webanwendungen auf einfache und konsistente Weise zu erstellen, wozu kaum Konfiguration erforderlich ist. Diese „universellen“ servergerenderten Web-Apps werden auch manchmal als „isomorph“ bezeichnet, was bedeutet, dass der Code zwischen Client und Server geteilt wird.

Das Erstellen eines Nuxt.js-Projekts beinhaltet die Beantwortung einer Reihe von Fragen zur Art des integrierten serverseitigen Frameworks, dem UI-Framework, dem Testframework, dem Modus, den Modulen und dem Linter, die Sie installieren möchten:

1. Öffnen Sie das WSL-Terminal (d. h. Ubuntu 18.04).

2. Erstellen Sie mit `mkdir NuxtProjects` einen neuen Projektordner, und wechseln Sie mit `cd NuxtProjects` in das Verzeichnis.

3. Installieren Sie Nuxt.js, und erstellen Sie ein Projekt (und ersetzen Sie dabei 'my-nuxt-app' durch jeden beliebigen Namen für Ihre App): `npm create nuxt-app my-nuxt-app`.

4. Das Installationsprogramm von Nuxt.js stellt Ihnen nun die folgenden Fragen:
    - Projektname: my-nuxtjs-app
    - Projektbeschreibung: Beschreibung der Nuxt.js-App.
    - Name des Autors: Ich verwende meinen GitHub-Alias.
    - Auswahl des Paket-Managers: Yarn oder **Npm**: Wir verwenden für die Beispiele NPM.
    - Auswahl des UI-Frameworks: Keins, Ant Design Vue, Bootstrap Vue usw. Wählen Sie für dieses Beispiel **Vuetify**. Die Vue-Community hat außerdem eine gute [Zusammenfassung zum Vergleichen dieser UI-Frameworks erstellt](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr), damit Sie die beste Lösung für Ihr Projekt auswählen können.
    - Auswahl benutzerdefinierter Serverframeworks: Keins, AdonisJs, Express, Fastify usw. Wählen Sie für dieses Beispiel **Keins** aus. Auf der Seite Dev.to finden Sie einen [Vergleich der Serverframeworks 2019–2020](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm).
    - Auswahl von Nuxt.js-Modulen (verwenden Sie die LEERTASTE, um Module auszuwählen, oder EINGABE, wenn Sie keine auswählen möchten): Axios (zur Vereinfachung von HTTP-Anforderungen) oder [PWA-Unterstützung](https://pwa.nuxtjs.org/) (zum Hinzufügen eines Serviceworkers, einer manifest.json-Datei usw). Fügen Sie für dieses Beispiel kein Modul hinzu.
    - Auswählen von Linting-Tools: **ESLint**, Prettier, Lint gestaffelte Dateien. Wählen Sie **ESLint** aus (ein Tool, das Ihren Code analysiert und Sie bei möglichen Fehlern warnt).
    - Auswahl eines Testframeworks: **Keins**, Jest, AVA. Wählen Sie **Keins** aus, da wir Testen im Rahmen dieses Schnellstarts nicht behandeln.
    - Auswahl des Rendermodus: **Universal (SSR)** oder Einzelseiten-App (Single Page App, SPA). Wählen Sie für das Beispiel **Universal (SSR)** aus. Die [Nuxt.js-Dokumentation](https://nuxtjs.org/guide#server-rendered-universal-ssr-) weist auf einige der Unterschiede hin – für SSR ist ein laufender Node.js-Server erforderlich, um Ihre App zu rendern, während sich SPA für statisches Hosting eignet.
    - Auswahl von Entwicklungstools: **jsconfig.json** (für VS Code empfohlen, damit die Intellisense-Codevervollständigung funktioniert)

5. Nachdem Ihr Projekt erstellt wurde, wechseln Sie mit `cd my-nuxtjs-app` in Ihr Nuxt.js-Projektverzeichnis, und geben Sie dann `code .` ein, um das Projekt in der VS Code WSL-Remote-Umgebung zu öffnen.

    ![WSL-Remote-Erweiterung](../images/wsl-remote-extension.png)

6. Diese 3 Befehle müssen Sie kennen, sobald Nuxt.js installiert ist:

    - `npm run dev` zum Ausführen einer Entwicklungsinstanz mit Hot Reloading, Dateiüberwachung und erneuter Taskausführung.
    - `npm run build` zum Kompilieren Ihres Projekts.
    - `npm start` zum Starten Ihrer App im Produktionsmodus.

    Öffnen Sie das in VS Code integrierte WSL-Terminal (**Ansicht > Terminal**). Vergewissern Sie sich, dass der Terminalpfad auf das Projektverzeichnis verweist (d. h. `~/NuxtProjects/my-nuxt-app$`). Versuchen Sie dann, mithilfe von `npm run dev` eine Entwicklungsinstanz Ihrer neuen Nuxt.js-App auszuführen.

6. Der lokale Entwicklungsserver wird gestartet (und zeigt eine Art Statusanzeige für die Client- und Serverkompilierung an). Nachdem das Erstellen Ihres Projekts abgeschlossen ist, zeigt das Terminal „Erfolgreich kompiliert“ zusammen mit der für die Kompilierung benötigten Zeit an. Verweisen Sie im Webbrowser auf [http://localhost:3000](http://localhost:3000), um Ihrer neue Nuxt.js-App zu öffnen.

    ![Ihre Nuxt.js-App bei der Ausführung auf localhost:3000](../images/nuxt-app.png)

7. Öffnen Sie die `pages/index.vue`-Datei im VS Code-Editor. Suchen Sie nach dem Seitentitel `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>`, und ändern Sie ihn in `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Speichern Sie bei immer noch in localhost:3000 geöffnetem Webbrowser Ihre Änderung, und beachten Sie, wie das Hot Reloading-Feature Ihre Änderung automatisch kompiliert und Ihre Änderung im Browser aktualisiert.

8. Betrachten wir nun, wie Nuxt.js mit Fehlern umgeht. Entfernen Sie das Endtag `</v-card-title>`, sodass Ihr Titelcode nun so aussieht: `<v-card-title class="headline">This is my new Nuxt.js app!`. Speichern Sie diese Änderung, und beachten Sie, dass im Browser und in Ihrem Terminal ein Kompilierungsfehler angezeigt wird, der Sie informiert, dass ein Endtag für `<v-card-title>` fehlt, zusammen mit der Zeilennummer, in der Sie den Fehler in Ihrem Code finden. Ersetzen Sie das `</v-card-title>`-Endtag, und speichern Sie, dann wird die Seite erneut geladen.

Sie können den Debugger von VS Code mit Ihrer Nuxt.js-App verwenden, indem Sie die F5-Taste drücken oder in der Menüleiste zu **Ansicht > Debuggen** (STRG+UMSCHALT+D) und **Ansicht > Debugging-Konsole** (STRG+UMSCHALT+Y) wechseln. Wenn Sie das Zahnradsymbol im Debugfenster auswählen, wird eine Startkonfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debugsetup speichern können. Weitere Informationen finden Sie unter [Debuggen in VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![VS Code-Fenster „Debuggen“ und Symbol „launch.json konfigurieren“](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu Nuxt.js finden Sie im [Nuxt.js-Leitfaden](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Erste Schritte mit Gatsby.js

Gatsby.js ist ein statisches Framework zur Websitegenerierung, das auf React.js basiert, im Unterschied zu Lösungen mit serverseitigem Rendering, wie Next.js und Nuxt.js. Ein statischer Websitegenerator erzeugt statischen HTML-Code zur Erstellungszeit. Es ist kein Server erforderlich. Next.js und Nuxt.js generieren HTML-Code zur Laufzeit (immer, wenn eine neue Anforderung eingeht). Für ihre Ausführung ist ein Server erforderlich. Gatsby schreibt außerdem vor, wie Daten in Ihrer App (mit GraphQL) behandelt werden, während „Next.js“ und „Nuxt.js“ Ihnen die Entscheidung überlassen.

So erstellen Sie ein Gatsby.js-Projekt:

1. Öffnen Sie das WSL-Terminal (d. h. Ubuntu 18.04).
2. Erstellen Sie mit `mkdir GatsbyProjects` einen neuen Projektordner, und wechseln Sie mit `cd GatsbyProjects` in das Verzeichnis.
3. Verwenden Sie npm, um die Gatsby-Befehlszeilenschnittstelle zu installieren: `npm install -g gatsby-cli`. Überprüfen Sie nach der Installation die Version mit `gatsby --version`.
4. Erstellen Sie Ihr Gatsby.js-Projekt: `gatsby new my-gatsby-app`
5. Nachdem das Paket installiert wurde, wechseln Sie in Ihren neuen App-Ordner `cd my-gatsby-app`, und verwenden Sie dann `code .`, um das Gatsby-Projekt in VS Code zu öffnen. Auf diese Weise können Sie sich mithilfe des Datei-Explorers von VS Code das Gatsby.js-Framework ansehen, das für Ihre App erstellt wurde. Beachten Sie, dass VS Code Ihre App in einer WSL-Remote-Umgebung geöffnet hat (worauf die grüne Registerkarte unten links in Ihrem VS Code-Fenster hinweist). Dies bedeutet, dass Sie zwar VS Code für die Bearbeitung unter dem Windows-Betriebssystem verwenden, Ihre App aber weiterhin unter dem Linux-Betriebssystem ausgeführt wird.

    ![WSL-Remote-Erweiterung](../images/wsl-remote-extension.png)

6. Nach der Installation von Gatsby müssen Sie drei Befehle kennen:

    - `gatsby develop` zum Ausführen einer Entwicklungsinstanz mit Hot-Reload.
    - `gatsby build` zum Erstellen eines Produktionsbuilds.
    - `gatsby serve` zum Starten Ihrer App im Produktionsmodus.

    Öffnen Sie das in VS Code integrierte WSL-Terminal (**Ansicht > Terminal**). Vergewissern Sie sich, dass der Terminalpfad auf das Projektverzeichnis verweist (d. h. `~/GatsbyProjects/my-gatsby-app$`). Versuchen Sie dann, mithilfe von `gatsby develop` eine Entwicklungsinstanz Ihrer neuen App auszuführen.

7. Sobald die Kompilierung des neuen Gatsby-Projekts abgeschlossen ist, zeigt das Terminal an, dass Sie jetzt gatsby-starter-default im Browser unter [http://localhost:8000/](http://localhost:8000/) anzeigen können. Wählen Sie diesen localhost-Link aus, um Ihr neu erstelltes Projekt in einem Webbrowser anzuzeigen.

> [!NOTE]
> Sie werden bemerken, dass Ihre Terminalausgabe Sie außerdem darüber informiert, dass Sie GraphiQL, eine in den Browser integrierte IDE, anzeigen können, um die Daten und das Schema Ihrer Website zu untersuchen: [http://localhost:8000/___graphql](http://localhost:8000/___graphql). GraphQL konsolidiert Ihre APIs in einer selbstdokumentierenden IDE (GraphiQL), die in Gatsby integriert ist. Über das Untersuchen von Daten und Schema Ihrer Website hinaus können Sie GraphQL-Vorgänge wie Abfragen, Mutationen und Abonnements ausführen. Weitere Informationen finden Sie unter [Einführung in GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Öffnen Sie die `src/pages/index.js`-Datei im VS Code-Editor. Suchen Sie nach dem Seitentitel `<h1 >Hi people</h1>`, und ändern Sie ihn in `<h1 >Hi (Your Name)!</h1>`. Speichern Sie bei immer noch in localhost:8000 geöffnetem Webbrowser Ihre Änderung, und beachten Sie, wie das Hot Reloading-Feature Ihre Änderung automatisch kompiliert und Ihre Änderung im Browser aktualisiert.

    ![Ihre Gatsby.js-App bei der Ausführung auf localhost:3000](../images/gatsby-app.png)

9. Betrachten wir nun, wie Next.js mit Fehlern umgeht. Entfernen Sie das Endtag `</h1>`, sodass Ihr Titelcode nun so aussieht: `<h1>Hi (Your Name)!`. Speichern Sie diese Änderung, und beachten Sie, dass der Fehler „Fehler bei der Kompilierung“ im Browser sowie in Ihrem Terminal angezeigt wird, um Sie zu informieren, dass für `<h1>` ein Endtag erwartet wird. Ersetzen Sie das `</h1>`-Endtag, und speichern Sie, dann wird die Seite erneut geladen.

Sie können den Debugger von VS Code mit Ihrer Next.js-App verwenden, indem Sie die F5-Taste drücken oder in der Menüleiste zu **Ansicht > Debuggen** (STRG+UMSCHALT+D) und **Ansicht > Debugging-Konsole** (STRG+UMSCHALT+Y) wechseln. Wenn Sie das Zahnradsymbol im Debugfenster auswählen, wird eine Startkonfigurationsdatei (`launch.json`) erstellt, in der Sie Details zum Debugsetup speichern können. Weitere Informationen finden Sie unter [Debuggen in VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![VS Code-Fenster „Debuggen“ und Symbol „launch.json konfigurieren“](../images/vscode-debug-launch-configuration.png)

Weitere Informationen zu Gatsby finden Sie in der [Gatsby.js-Dokumentation](https://www.gatsbyjs.org/docs/).
