# Redux

## Redux i React-Redux a React

### Per què necessitem Redux?

En una aplicació React:

* Cada component pot tenir estat local amb useState.

Quan l’aplicació creix:

* molts components necessiten les mateixes dades (ex.: usuari loguejat, rol, configuració)
* s'han de passar dades entre components amb props (“prop drilling”)
* l’estat queda dispers i difícil de mantenir

**Redux** centralitza l’estat global en un únic lloc (`store`) i permet accedir-hi des de qualsevol component de manera controlada.

### Conceptes bàsics

#### `Store`

És l’objecte que conté tot l’**estat global de l’aplicació**.

#### Action

És un objecte que descriu què ha passat:

```json
{
  type: 'login',
  user: 'Dani'
}
```

* `type`: és obligatori i indica l’acció
* altres propietats (user, logged, etc.) contenen dades per modificar l’estat
* **No modifica** l’estat per ell mateix, només informa què ha passat

#### Reducer

* És una funció que decideix com canvia l’estat.
* No modifica directament l’estat original
* No fa crides externes ni té efectes secundaris
* Sempre retorna un nou objecte d’estat

#### Flux de dades

1. Un component fa dispatch(action)
2. Redux envia l’acció al reducer
3. El reducer calcula el nou estat
4. El store s’actualitza

React es re-renderitza automàticament

### Exemple: changeState reducer

Aquest és un exemple molt simple i flexible que es pot utilitzar per actualitzar qualsevol part de l’estat global:

```jsx
const changeState = (state = initialState, { type, ...rest }) => {
  switch (type) {
    case 'set':
      return { ...state, ...rest }
    default:
      return state
  }
}
```

* `state = initialState`: si és la primera vegada que el reducer s’executa, `state` s’inicialitza amb `initialState`.
* `{ type, ...rest }`: destructura l’objecte action.
  * `type` és el tipus d’acció
  * `rest` són totes les altres propietats de l’acció.
* `switch (type)`
  * Només hi ha el cas 'set':
  * return { ...state, ...rest } Això crea un nou objecte d’estat combinant l’estat actual amb les propietats que arriben a rest.

```jsx
state = { user: null, logged: false }
dispatch({ type: 'set', user: 'Dani', logged: true })
```

Resultat:

```jsx
{ user: 'Dani', logged: true }
```

* Cas per defecte

```jsx
default:
  return state
```

Si el tipus d’acció no és `set`, retorna l’estat tal com està.

### React-Redux: connectant Redux amb React

* Redux no forma React. És javascript "pur"
* Per utilitzar juntament amb react s'utilitza `react-redux`.

#### Funcionalitats principals

* **Provider**: injecta l'`store` a l’arrel de l’aplicació
* **useSelector()**: llegeix dades del store
* **useDispatch()**: envia les accions

### Exemple complet amb changeState

#### store.js

```jsx
// store.js
import { createStore } from 'redux';
import { changeState } from './reducers';

const initialState = { user: null, logged: false };
export const store = createStore(changeState, initialState);
```

#### main.jsx

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { Provider } from 'react-redux'
import { store } from './store'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
)
```

#### Component React

```jsx
import { useSelector, useDispatch } from 'react-redux'

function App() {
  const { user, logged } = useSelector(state => state)
  const dispatch = useDispatch()

  return (
    <>
      <p>Usuari: {user || 'No loguejat'}</p>
      <p>Loguejat: {logged ? 'Sí' : 'No'}</p>

      <button
        onClick={() =>
          dispatch({ type: 'set', user: 'Dani', logged: true })
        }
      >
        Login
      </button>

      <button
        onClick={() => dispatch({ type: 'set', user: null, logged: false })}
      >
        Logout
      </button>
    </>
  )
}

export default App
```

### Instal·lació en un projecte Vite

```bash
npm install redux react-redux
```
