# Introducció React

### Requisits previs

* Llibreries i coneixements mínims
  * HTML
  * CSS
  * JS ES6
  * consola
  * let/const
  * arrow functions
  * promeses/async
  * etc.

{% hint style="info" %}
Podrem considerar que React és un "JavaScript modern", tot i que aquesta definicio no és del tot correcte.
{% endhint %}

### Què és React?

* És una biblioteca de Javascript utilitzada per crear interfícies d'usuari interactives i declaratives.
* Els seus principals punts clau són:
  * **Components**: La UI es divideix en components reutilitzables i independents.
  * **Declaracions**: React s'encarrega d'actualitzar la vista quan l'estat de l'aplicació canvia, sense necessitat de manipular directament el DOM.
  * **DOM Virtual**: S'utilitza un **DOM Virtual** per fer actualitzacions de forma eficient, minimitzant renderitzacions innecessàries.
  * **Unidireccional**: El flux de dades és de **pare a fill**, fent previsible l'estat de l'aplicació.
  * **Extensible**: Pot combinar-se amb altres biblioteques o frameworks per gestionar l'estat, rutes o crides a API.

React permet crear aplicacions web **ràpides, escalables i fàcilment mantenibles**, centrant-se en la UI i la reutilització de components.

### Entorn de treball

* Per aquesta introducció utilitzarem `Node` i `Vite`.

```bash
npm create vite@latest my-react-app --template react
cd my-react-app
npm install
npm run dev
```

### Estructura d'un projecte React

* Organització típica
  * src
  * components
  * pages
  * assets
* Mantenir el codi llegible i escalable.

```jsx
import React from 'react'
import { createRoot } from 'react-dom/client'
import App from './App'
import './index.css'

createRoot(document.getElementById('root')).render(<App />)
```

* Importem `React` i la funció per crear el root.
* Importem `App` (component principal) i CSS global.
* Creem el root i renderitzem `<App />` dins l'element amb id `root` de l'index.html.

### [Components](components/components-i-propietats.md#components)

Un **component** de React és la unitat bàsica de construcció d'una aplicació React.

Representa una **part reutilitzable de la interfície d'usuari UI**.

Enlloc de crear una pàgina HTML sencera, React divideix la interfície en **peces petites i independents** (components), cadascuna amb la seva pròpia lògica i representació visual.

{% hint style="info" %}
En React, **tot és un component**: botons, formularis, capçaleres, pàgines senceres, etc.
{% endhint %}

#### Per què utilitzar components?

Els components permeten:

* Reutilitzar codi
* Separar responsabilitats
* Mantenir i escalar millor l'aplicació
* Facilitar proves i depuració
* Organitzar la UI de forma modular

### Hooks

* Són **funcions especials** de React que permeten als components:
  * tenir **estat**
  * executar **efectes secundaris**
  * accedir a **funcionalitats internes** de React

Abans de l'aparició dels Hooks, aquestes funcions només existien als **components**.

Els Hooks permeten "enganxar" funcionalitats de React dintre de funcions o altres components.

#### Per què existeixen?

* Permeten reutilitzar lògica entre components.
* Fan que els components siguin més petits i llegibles
* Milloren la separació de responsabilitats

#### A tenir en compte

* Només es poden cridar
  * dins de components funcionals
  * dins de "`Custom Hooks`"
* Sempre s'han de cridar
  * al **nivell superior** del component
  * No es poden cridar dintre `If`, `for`, `while`.

#### Alguns tipus de Hooks

* Hi ha diferents tipus de `Hooks` alguns són:  &#x20;
  * [`State`i `useState`](hooks/state-i-usestate.md). **Estat local**. Permeten gestionar l'estat dintre d'un component funcional. Quan es modifica l'estat, el component es torna a renderitzar.
  * [`effect`i `useEffect`](hooks/efectes-i-useeffect.md). Serveixen per executar codi després d'un render: fetch, intervals, subscripcions, etc.
  * [`useRef`](hooks/useref.md). **Referències mutables**. Enmagatzemem valors persistents que **causen no-render**.

### [React Router](react-router/react-router.md)

* És una llibreria per a React que permet gestionar la navegació dins d'una **SPA** (Single Page Application) sense recarregar la pàgina.&#x20;
* En funció de la URL, React Router decideix **quin component s'ha de renderitzar**.

### Bones pràctiques, testing i depuració

* Bones pràctiques:
  * Components petits i amb una sola responsabilitat.
  * Separar lògica (hooks) de la UI.
  * Evitar efectes laterals no controlats.
* Testing:
  * React Testing Library per proves d'integració d'UI.

```jsx
// Button.test.jsx
import { render, screen, fireEvent } from '@testing-library/react'
import Button from './Button'

test('crida el onClick quan es fa clic', () => {
  const onClick = jest.fn()
  render(<Button onClick={onClick}>Clic</Button>)
  fireEvent.click(screen.getByText('Clic'))
  expect(onClick).toHaveBeenCalled()
})
```

* Depuració:
  * React DevTools, console.log estratègic, inspeccionar props i estat.

### Deploy

* Com pujar l'app a Internet.
  * Connecta el repo i configura build command (`npm run build` per Vite) i output (`dist` per Vite).

### Recursos

* Documentació oficial React: https://reactjs.org
* React Router docs: https://reactrouter.com
* Vite: https://vitejs.dev
* Tutorials: FreeCodeCamp, MDN, vídeos de Traversy Media i Fireship.
