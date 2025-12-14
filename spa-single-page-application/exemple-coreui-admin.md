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

## Fitxers de configuració

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

## Components principals

### views/Dashboard.jsx

Component que mostra el `dashboard` (pàgina per defecte)

```jsx
import { CRow, CCol, CWidgetStatsA } from '@coreui/react'

const Dashboard = () => {
  return (
    <CRow>
      <CCol sm={6} lg={3}>
        <CWidgetStatsA
          className="mb-4"
          value="10"
          title="Usuaris"
        />
      </CCol>

      <CCol sm={6} lg={3}>
        <CWidgetStatsA
          className="mb-4"
          value="5"
          title="Posts"
        />
      </CCol>
    </CRow>
  )
}

export default Dashboard


```

### views/Users.jsx

```jsx
const Users = () => {
  return (
    <>
      <h1>Gestió d’usuaris</h1>
      <p>Aquí hi anirà el CRUD</p>
    </>
  )
}

export default Users

```

## Definició de les rutes

### src/routes.jsx

```jsx
import Dashboard from './views/Dashboard'
import Users from './views/Users'

const routes = [
  { path: '/dashboard', element: Dashboard },
  { path: '/users', element: Users },
]

export default routes

```

## Disseny de la plantilla (CoreUI)

### layout/DefaultLayout.jsx

* És el fitxer principal de la plantilla
* Defineix l'estructura, opcions de navegació i els continguts

```jsx
import AppSidebar from '../components/AppSidebar'
import AppHeader from '../components/AppHeader'
import AppFooter from '../components/AppFooter'
import AppContent from '../components/AppContent'

const DefaultLayout = () => {
  return (
    <div>
      <AppSidebar />

      <div className="wrapper d-flex flex-column min-vh-100 bg-light">
        <AppHeader />
        <div className="body flex-grow-1 px-3">
          <AppContent />
        </div>
        <AppFooter />
      </div>
    </div>
  )
}

export default DefaultLayout

```

### components/AppContent.jsx

```jsx
import React, { Suspense } from 'react'
import { Navigate, Route, Routes } from 'react-router-dom'
import { CContainer, CSpinner } from '@coreui/react'

// routes config
import routes from '../routes'

const AppContent = () => {
  return (
    <CContainer className="px-4" lg>
      <Suspense fallback={<CSpinner color="primary" />}>
        <Routes>
          {routes.map((route, idx) => {
            return (
              route.element && (
                <Route
                  key={idx}
                  path={route.path}
                  exact={route.exact}
                  name={route.name}
                  element={<route.element />}
                />
              )
            )
          })}
          <Route path="/" element={<Navigate to="dashboard" replace />} />
        </Routes>
      </Suspense>
    </CContainer>
  )
}

export default React.memo(AppContent)

```

### components/AppFooter.jsx

```jsx
import React from 'react'
import { CFooter } from '@coreui/react'

const AppFooter = () => {
  return (
    <CFooter className="px-4">
      <div>
        <a href="#" target="_blank" rel="noopener noreferrer">
          2DAW
        </a>
        <span className="ms-1">&copy; 2025 2DAW.</span>
      </div>
      <div className="ms-auto">
        <span className="me-1">Powered by</span>
        <a href="https://coreui.io/react" target="_blank" rel="noopener noreferrer">
          CoreUI React Admin &amp; Dashboard Template
        </a>
      </div>
    </CFooter>
  )
}

export default React.memo(AppFooter)
```

### components/AppHeader.jsx

```jsx
import { useEffect, useRef } from 'react'
import { NavLink } from 'react-router-dom'
import { useSelector, useDispatch } from 'react-redux'
import { CContainer, CHeader, CHeaderNav, CHeaderToggler, CNavLink, CNavItem} from '@coreui/react'
import CIcon from '@coreui/icons-react'
import {cilMenu} from '@coreui/icons'
//import { AppBreadcrumb } from './index'

const AppHeader = () => {
  const headerRef = useRef()
 
  const dispatch = useDispatch()
  const sidebarShow = useSelector((state) => state.sidebarShow)

  useEffect(() => {
    const handleScroll = () => {
      headerRef.current &&
        headerRef.current.classList.toggle('shadow-sm', document.documentElement.scrollTop > 0)
    }

    document.addEventListener('scroll', handleScroll)
    return () => document.removeEventListener('scroll', handleScroll)
  }, [])

  return (
    <CHeader position="sticky" className="mb-4 p-0" ref={headerRef}>
      <CContainer className="border-bottom px-4" fluid>
        <CHeaderToggler
          onClick={() => dispatch({ type: 'set', sidebarShow: !sidebarShow })}
          style={{ marginInlineStart: '-14px' }}
        >
          <CIcon icon={cilMenu} size="lg" />
        </CHeaderToggler>
        <CHeaderNav className="d-none d-md-flex">
          <CNavItem>
            <CNavLink to="/dashboard" as={NavLink}>
              Dashboard
            </CNavLink>
          </CNavItem>
          <CNavItem>
            <CNavLink to="/users" as={NavLink}>Users</CNavLink>
          </CNavItem>
        </CHeaderNav>
        
      </CContainer>
      <CContainer className="px-4" fluid>
  {/*      <AppBreadcrumb />
  */}
      </CContainer>
    </CHeader>
  )
}

export default AppHeader

```

### components/AppSidebar.jsx

```jsx
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import {CCloseButton,CSidebar,CSidebarFooter,CSidebarHeader,CSidebarToggler,} from '@coreui/react'
import { AppSidebarNav } from './AppSidebarNav'
import navigation from '../_nav'

const AppSidebar = () => {
  const dispatch = useDispatch()
  const unfoldable = useSelector((state) => state.sidebarUnfoldable)
  const sidebarShow = useSelector((state) => state.sidebarShow)

  return (
    <CSidebar className="border-end" colorScheme="dark" position="fixed" unfoldable={unfoldable}
      visible={sidebarShow} onVisibleChange={(visible) => {
        dispatch({ type: 'set', sidebarShow: visible })
      }}>
    	<CSidebarHeader className="border-bottom">
        	<CCloseButton className="d-lg-none" onClick={() => dispatch({ type: 'set', sidebarShow: false })} />
      	</CSidebarHeader>
      	<AppSidebarNav items={navigation} />
      	<CSidebarFooter className="border-top d-none d-lg-flex">
        	<CSidebarToggler onClick={() => dispatch({ type: 'set', sidebarUnfoldable: !unfoldable })} />
      </CSidebarFooter>
    </CSidebar>
  )
}

export default React.memo(AppSidebar)

```

### components/AppSidebarNav.jsx

```jsx
import React from 'react'
import { NavLink } from 'react-router-dom'
import PropTypes from 'prop-types'

import SimpleBar from 'simplebar-react'
import 'simplebar-react/dist/simplebar.min.css'

import { CBadge, CNavLink, CSidebarNav } from '@coreui/react'

export const AppSidebarNav = ({ items }) => {
  const navLink = (name, icon, badge, indent = false) => {
    return (
      <>
        {icon
          ? icon
          : indent && (
              <span className="nav-icon">
                <span className="nav-icon-bullet"></span>
              </span>
            )}
        {name && name}
        {badge && (
          <CBadge color={badge.color} className="ms-auto" size="sm">
            {badge.text}
          </CBadge>
        )}
      </>
    )
  }

  const navItem = (item, index, indent = false) => {
    const { component, name, badge, icon, ...rest } = item
    const Component = component
    return (
      <Component as="div" key={index}>
        {rest.to || rest.href ? (
          <CNavLink
            {...(rest.to && { as: NavLink })}
            {...(rest.href && { target: '_blank', rel: 'noopener noreferrer' })}
            {...rest}
          >
            {navLink(name, icon, badge, indent)}
          </CNavLink>
        ) : (
          navLink(name, icon, badge, indent)
        )}
      </Component>
    )
  }

  const navGroup = (item, index) => {
    const { component, name, icon, items, to, ...rest } = item
    const Component = component
    return (
      <Component compact as="div" key={index} toggler={navLink(name, icon)} {...rest}>
        {items?.map((item, index) =>
          item.items ? navGroup(item, index) : navItem(item, index, true),
        )}
      </Component>
    )
  }

  return (
    <CSidebarNav as={SimpleBar}>
      {items &&
        items.map((item, index) => (item.items ? navGroup(item, index) : navItem(item, index)))}
    </CSidebarNav>
  )
}

AppSidebarNav.propTypes = {
  items: PropTypes.arrayOf(PropTypes.any).isRequired,
}

```

## Component Principal

### src/App.jsx

```jsx
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom'
import DefaultLayout from './layout/DefaultLayout'
import routes from './routes'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<DefaultLayout />}>
          <Route index element={<Navigate to="/dashboard" replace />} />

          {routes.map((route, idx) => (
            <Route
              key={idx}
              path={route.path}
              element={<route.element />}
            />
          ))}
        </Route>
      </Routes>
    </BrowserRouter>
  )
}

export default App

```

## Executar l'aplicació

```bash
npm run dev
```

