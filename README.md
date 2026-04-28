Die Pakete lassen sich sehr gut in logische Gruppen sortieren. Dann musst du nicht mehr 12 Namen einzeln auswendig lernen.

# 1. React selbst
```bash
npm install react react-dom
```
Gehören zusammen:
```bash
react      = React-Kern
react-dom  = React im Browser anzeigen
```
Merksatz:
```bash
react denkt.
react-dom malt es in die Webseite.
```
# 2. Webpack-Grundsystem
```bash
npm install -D webpack webpack-cli webpack-dev-server
```
Gehören zusammen:
```bash
webpack             = der Bundler
webpack-cli         = damit du webpack per Terminal starten kannst
webpack-dev-server  = Entwicklungsserver mit Live Reload
```
Merksatz:

webpack baut.
webpack-cli startet den Bau.
webpack-dev-server zeigt den Bau live im Browser.
3. TypeScript für React
npm install -D typescript ts-loader @types/react @types/react-dom

Gehören zusammen:

typescript       = eigentlicher TypeScript-Compiler
ts-loader        = verbindet TypeScript mit Webpack
@types/react     = TypeScript-Beschreibungen für React
@types/react-dom = TypeScript-Beschreibungen für React-DOM

Merksatz:

typescript kann TS übersetzen.
ts-loader bringt es in Webpack.
@types/... erklärt TypeScript fremde Bibliotheken.
4. SCSS / CSS-Kette
npm install -D sass sass-loader css-loader style-loader

Gehören zusammen:

sass         = eigentlicher SCSS-Compiler
sass-loader  = verbindet Sass mit Webpack
css-loader   = macht CSS importierbar
style-loader = schreibt CSS in die Webseite

Die Kette:

.scss
  ↓
sass-loader
  ↓
.css
  ↓
css-loader
  ↓
CSS als JS-Modul
  ↓
style-loader
  ↓
<style> im Browser

Merksatz:

sass übersetzt SCSS.
css-loader macht CSS importierbar.
style-loader bringt CSS in die Seite.
5. HTML-Erzeugung
npm install -D html-webpack-plugin

Gehört eher allein:

html-webpack-plugin = erzeugt/verwendet HTML und hängt dein Bundle ein

Merksatz:

Plugin baut die Startseite um das Bundle herum.
Gesamtübersicht
React:
  react
  react-dom

Webpack:
  webpack
  webpack-cli
  webpack-dev-server

TypeScript:
  typescript
  ts-loader
  @types/react
  @types/react-dom

SCSS:
  sass
  sass-loader
  css-loader
  style-loader

HTML:
  html-webpack-plugin
Als Installationsblöcke
# React
npm install react react-dom

# Webpack
npm install -D webpack webpack-cli webpack-dev-server

# TypeScript
npm install -D typescript ts-loader @types/react @types/react-dom

# SCSS
npm install -D sass sass-loader css-loader style-loader

# HTML
npm install -D html-webpack-plugin
Die wichtigste Daumenregel
Bibliothek:
  Das benutzt dein Code direkt.
  Beispiel: react, react-dom

Compiler:
  Der übersetzt eine Sprache.
  Beispiel: typescript, sass

Loader:
  Der verbindet einen Compiler oder Dateityp mit Webpack.
  Beispiel: ts-loader, sass-loader, css-loader, style-loader

Plugin:
  Das erweitert Webpack insgesamt.
  Beispiel: html-webpack-plugin

CLI/Server:
  Damit steuerst du Webpack.
  Beispiel: webpack-cli, webpack-dev-server

Und ganz kurz:

react + react-dom
= App schreiben

webpack + webpack-cli + webpack-dev-server
= App bauen und starten

typescript + ts-loader + @types/...
= TSX verstehen

sass + sass-loader + css-loader + style-loader
= SCSS verstehen

html-webpack-plugin
= HTML automatisch vorbereiten
