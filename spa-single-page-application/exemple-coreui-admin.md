---
hidden: true
---

# Exemple: CoreUI Admin

En aquest exemple es crearà una aplicació basada en CoreuUI Free Template

Podem trobar més informació a:

* [https://coreui.io/react/](https://coreui.io/react/)
* [https://github.com/coreui/coreui-free-react-admin-template](https://github.com/coreui/coreui-free-react-admin-template)

## Creació del projecte

```bash
 npm create vite@latest admin-coreui
 cd admin-coreui
 npm install
```

Durant el procés d'instal·lació s'ha de seleccionar React i Javascript

A continuació instal·larem les dependències addicionals per poder treballar.

```bash
npm install react-router-dom
npm install redux react-redux simplebar-react
npm install @coreui/react @coreui/icons @coreui/icons-react
```

## Modificació de fitxers

### src/store.js

Aquest fixar conté l'[`store`](redux.md#store) de [`Redux`](redux.md#redux-i-react-redux-a-react).

```jsx
import { legacy_createStore as createStore } from 'redux'

const initialState = {
  sidebarShow: true,
  theme: 'light',
}

const changeState = (state = initialState, { type, ...rest }) => {
  switch (type) {
    case 'set':
      return { ...state, ...rest }
    default:
      return state
  }
}

const store = createStore(changeState)
export default store

```

### src/index.css

Definim les classes i estils utilitzats

```css
.wrapper {
  width: 100%;
  padding-left: var(--cui-sidebar-occupy-start, 0);
}

```

### src/main.jsx

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import '@coreui/coreui/dist/css/coreui.min.css'
import './index.css'
import '@coreui/icons/css/all.min.css'
import { Provider } from 'react-redux'
import store from './store'

createRoot(document.getElementById('root')).render(
  <StrictMode>
	  <Provider store={store}>
    	<App />
	  </Provider>
  </StrictMode>,
)
```

### src/\_nav.jsx

* Aquest fitxer conté les rutes de navegació que s'utilitzaran en l'aplicació.
* Es defineixen les rutes, els icones que es mostraran i el component encarregat de tractar-les

```jsx
import React from 'react'
import CIcon from '@coreui/icons-react'
import { cilUser, cilSpeedometer} from '@coreui/icons'
import { CNavItem, CNavTitle } from '@coreui/react'

const _nav = [
  {
    component: CNavItem,
    name: 'Dashboard',
    to: '/dashboard',
    icon: <CIcon icon={cilSpeedometer} customClassName="nav-icon" />,
    badge: {
      color: 'info',
      text: 'Home',
    },
  },
  {
    component: CNavItem,
    name: 'Usuaris',
    to: '/users',
    icon: <CIcon icon={cilUser} customClassName="nav-icon" />,
  }
 
]

export default _nav
```

