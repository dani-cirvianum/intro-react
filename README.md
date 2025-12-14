# Introducció React

## Introducció a React

### Objectius

* Aprendre a construir aplicacions React
* Gestionar l'estat
* Fer peticions a APIs
* Components
* Hooks
* Peticions
* Rutes

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
* React és "JavaScript modern"

### Entorn de treball

* Instal·lar Node i crear un projecte amb Vite.

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

### JSX

* Sintaxi que permet escriure HTML dins JS

```jsx
function Hola() {
  const nom = 'Dani'
  return <h1>Hola, {nom}!</h1>
}
```

[Accedeix a secció JSX](components/jsx.md)

### [Components](components/components-i-propietats.md#components)

* Són blocs reutilitzables
* Permeten separar la lògica de la presentació
* S'utilitzen per crear components funcionals

```jsx
// components/Header.jsx
export default function Header({ title }) {
  return <header><h1>{title}</h1></header>
}
```

* `Header` és una funció que rep `props` (destructurat a `{ title }`) i retorna JSX.

```jsx
import Header from './components/Header'

function App() {
  return (
    <div>
      <Header title="Benvinguts!" />
      <Header title="Notícies" />
    </div>
  )
}
```

### [Propietats](components/components-i-propietats.md#propietats)

* Paràmetres que passen dades de pare a fill.

```jsx
function Salutacio({ nom }) {
  return <p>Hola, {nom}!</p>
}

// ús:
<Salutacio nom="Pau" />
```

* `Salutacio` rep `nom` i renderitza el text.
* **Exercici** Crear `ProductCard` que rebi un `product` amb `name`, `price` i els mostri

### Hooks

* Són funcions especials que permete utilitzar `estat`, `efectes` i altres funcionalitats avançades de React dins de components funcionals.
* Hi ha diferents tipus de `Hooks`
  * [`State`i `useState`](hooks/state-i-usestate.md). **Estat local**. Permeten gestionar l'estat dintre d'un component funcional. Quan es modifica l'estat, el component es torna a renderitzar.
  * [`effect`i `useEffect`](hooks/efectes-i-useeffect.md). Serveixen per executar codi després d'un render: fetch, intervals, subscripcions, etc.
  * [`useRef`](hooks/useref.md). **Referències mutables**. Enmagatzemem valors persistents que **causen no-render**.

### [Manipulació d'esdeveniments](events-i-formularis/events.md)

* Permeten gestionar clics, submits, etc.
* Aquestes funcions reben l'event DOM

```jsx
function FormButton() {
  function handleClick(ev) {
    ev.preventDefault()
    alert('Has fet clic!')
  }
  return <button onClick={handleClick}>Clica'm</button>
}
```

* `onClick` rep la referència a la funció
* L'event es pot cancel·lar amb `preventDefault()`.

### [Formularis](events-i-formularis/formularis.md)

* Inputs controlats són aquells que el seu valor prové de l'estat del component
  * Permet tenir control total sobre el valor del formulari
  * Permet validar l'input abans d'enviar-lo
  * Permet modificar el valor a través de codi
* Els Formularis controlats tenen alguns avantatges
  * Permet **validació en temps real** abans de l'enviament
  * Facilita **resets** i **actualtizacions**
  * Manté l'estat del formulari dins de React, sense necessitat de modificar directament

```jsx
import { useState } from 'react'

function Formulari() {
  const [text, setText] = useState('')

  function handleSubmit(e) {
    e.preventDefault()
    console.log('Enviat:', text)
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button>Enviar</button>
    </form>
  )
}
```

* L'input obté el seu valor de l'estat `text`.
* `onChange` actualitza l'estat cada vegada que l'usuari escriu.

### [Peticions HTTP](consultes-http-i-api/peticions-http.md)

* Com consumir APIs utilitzant async/await.
* Async/await fa el codi més lineal i llegible que promeses encadenades.

```jsx
import { useState, useEffect } from 'react'

function Usuaris() {
  const [users, setUsers] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    // No fem l'efecte async directament => definim una funció async dins
    async function fetchUsers() {
      try {
        setLoading(true)
        const res = await fetch('https://jsonplaceholder.typicode.com/users')
        if (!res.ok) throw new Error(`HTTP error! status: ${res.status}`)
        const data = await res.json()
        setUsers(data)
      } catch (err) {
        setError(err)
      } finally {
        setLoading(false)
      }
    }

    fetchUsers()
  }, [])

  if (loading) return <p>Carregant...</p>
  if (error) return <p>Error: {error.message}</p>
  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name} ({u.email})</li>)}
    </ul>
  )
}
```



* Es pot utilitzar[`axios` ](consultes-http-i-api/axios-amb-react.md)amb async/await
  * La sintaxi seria `const res = await axios.get(url)` i `res.data` conté la resposta.
* Sempre s'ha de gestionar els errors i l'estat de càrrega per millorar UX (pensar en l'accessibilitat i la usabilitat).

### [React Router](react-router/react-router.md)

* És una llibreria per a React que permet gestionar la navegació dins d'una **SPA** (Single Page Application) sense recarregar la pàgina.&#x20;
* En funció de la URL, React Router decideix **quin component s'ha de renderitzar**.

```jsx
// main.jsx
import { BrowserRouter } from 'react-router-dom'
createRoot(...).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
)

// App.jsx
import { Routes, Route, Link } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'

function App() {
  return (
    <>
      <nav>
        <Link to="/">Inici</Link> | <Link to="/about">Sobre</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </>
  )
}
```

* `BrowserRouter` envolta l'app i gestiona l'historial.
* `Routes` conté `Route` amb `path` i `element`.

[Veure l'enllaç de React Router](react-router/react-router.md)

### [Context API](context-api.md)

* Compartir estat entre components sense passar props.

```jsx
import { createContext, useContext, useState } from 'react'

const ThemeContext = createContext()

export function ThemeProvider({ children }) {
  const [dark, setDark] = useState(false)
  return (
    <ThemeContext.Provider value={{ dark, setDark }}>
      {children}
    </ThemeContext.Provider>
  )
}

export function useTheme() {
  return useContext(ThemeContext)
}
```

* `ThemeProvider` embolica l'app i fa disponible `dark` i `setDark`.
* `useTheme` és un hook helper per consumir el context fàcilment.

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
