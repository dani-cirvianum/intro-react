# React Suspense

## `React.Suspense`

* React.Suspense és un component de React que permet gestionar la càrrega de contingut asíncron.
* Mostra una pantalla intermèdia (fallback) mentre s’estan carregant dades o components.
* És molt útil per lazy loading de components i recursos.

## Què fa exactament?

* Encara que React és sincròni per defecte però moltes operacions són asíncrones (importació de components, fetch de dades, etc.).
* Suspense envolcalla aquests components i mostra un fallback mentre no estan llestos.
* Quan el contingut es carrega, React el renderitza automàticament.

## Sintaxi bàsica

```jsx
import React, { Suspense } from 'react';

const ComponentLazy = React.lazy(() => import('./ComponentLazy'));

function App() {
  return (
    <Suspense fallback={<div>Carregant...</div>}>
      <ComponentLazy />
    </Suspense>
  );
}
```

* React.lazy() permet carregar un component només quan es necessita.
* Suspense envolta aquest component i defineix què mostrar mentre es carrega (fallback).
* Quan el component acaba de carregar-se, React el mostra automàticament.

## Exemple

```jsx
const Perfil = React.lazy(() => import('./Perfil'));

function App() {
  return (
    <div>
      <h1>Benvingut</h1>
      <Suspense fallback={<p>Carregant el perfil...</p>}>
        <Perfil />
      </Suspense>
    </div>
  );
}
```

* Sense Suspense, React.lazy donaria error.
* fallback pot ser qualsevol JSX: spinner, missatge, placeholder, etc.

## Quan utilitzar-lo?

* Lazy loading de components grans: millora el temps de càrrega inicial.
* Integració amb fetch de dades: mostra contingut provisional mentre arriben les dades.
* Rutes amb React Router: carregar només el component de la ruta quan s’hi navega
