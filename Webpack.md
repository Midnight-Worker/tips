# Hier ist die große Sammelinstallation für das komplette Setup:
```bash
npm install react react-dom jquery
npm install -D webpack webpack-cli webpack-dev-server html-webpack-plugin
npm install -D typescript ts-loader @types/react @types/react-dom @types/jquery
npm install -D sass sass-loader css-loader style-loader
npm install -D babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript
npm install -D concurrently
```
Oder als ein einziger großer Befehl:
```bash
npm install react react-dom jquery && npm install -D webpack webpack-cli webpack-dev-server html-webpack-plugin typescript ts-loader @types/react @types/react-dom @types/jquery sass sass-loader css-loader style-loader babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript concurrently
```
Wichtig: Es heißt nicht babel-core, sondern heute:
```
@babel/core
```
Und nicht babel-cli, sondern falls du die Babel-CLI irgendwann brauchst:
```
@babel/cli
```
Für Webpack brauchst du aber normalerweise nicht @babel/cli, weil Webpack Babel über diesen Loader benutzt:

babel-loader
Logisch gruppiert
```bash
React:
  react
  react-dom

Beispiel-Bibliothek:
  jquery
  @types/jquery

Webpack:
  webpack
  webpack-cli
  webpack-dev-server
  html-webpack-plugin

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

Babel:
  babel-loader
  @babel/core
  @babel/preset-env
  @babel/preset-react
  @babel/preset-typescript
```
Gemeinsam starten:
  `concurrently`




