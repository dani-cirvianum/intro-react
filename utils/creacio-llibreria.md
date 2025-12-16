# Creació llibreria

## Creació d'una llibreria i importació utilitzant npm

### Creació del projecte

Es pot utilitzar un projecte que estiguem desenvolupant amb vite+react amb les dependències corresponents. S'executa i es prova utilitzant

```bash
npm run dev
```

### Creació del fixer src/index.js

Aquest fitxer contindrà els components del projecte que es volen exportar i utilitzar en altres projectes

```jsx
// Només posar els components que interessa exportar
export {default as Dashboard} from './pages/Dashboard';
export {default as Settings} from './pages/Settings';
```

### Configurar Vite per contruir a llibreria (vite.config.js)

```jsx
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
    lib: {
      entry: 'src/index.js', // Fitxer que conté els exports
      name: 'my-library', // Nom del component a crear
      fileName: (format) => `my-library.${format}.js` // Adaptar el nom correcte
    }
  }
})

```

### Configurar package.json

```jsx
{
  "name": "my-component-lib",
  "version": "1.0.0",
  "main": "dist/my-component-lib.umd.js",
  "module": "dist/my-component-lib.es.js",
  "files": ["dist"],
  "peerDependencies": {
    "react": "^19.2.0",
    "react-dom": "^19.2.0"
  }
}
```

### Afegir scripts de build (si no estàº creat)

```jsx
"scripts": {
  "build": "vite build"
}
```

### Executar el build

```bash
npm run build
```

Això crearà la carpeta dist/ amb els fitxers de la llibreria

### Consumir la llibreria en un altre projecte

Modificar per posar la ruta corresponent

```bash
npm install ../my-compoentnt
```

### Utilitzar els components

Utilitzar el component com un component de React "normal" important en `npm`

```jsx
import {Component} from "my-component-lib"
```
