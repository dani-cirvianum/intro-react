# Hooks personalitzats

## Hooks personalitzats

* Un `Custom Hook` és una funció JavaScript normal que comença amb el prefix use (per exemple, `useFetch`, `useLocalStorage`, `useToggle`).

### Per què utilitzar Hooks Personalitzats?

* Extreure la lògica d'estat (`stateful logic`) d'un component per poder-la reutilitzar en altres llocs, sense haver de copiar i enganxar codi.
* Permeten compartir:
  * **Lògica d'Estat**: Variables d'estat creades amb `useState`.
  * **Lògica d'Efectes**: Efectes secundaris creats amb `useEffect`.
  * **Referències**: Referències creades amb `useRef`.

### Regles Bàsiques

* **Prefix use**:
  * El nom de la funció ha de començar amb use (p. ex., `useAuth`, `useCounter`).
  * Aquesta convenció és **obligatòria** perquè React pugui identificar-la com un Hook i aplicar correctament les Regles dels Hooks
  * Només es criden al nivell superior dels components de funció o altres Hooks.
* **Utilitzar Hooks Interns**: Un Custom Hook gairebé sempre cridarà a un o més Hooks interns de React (useState, useEffect, useRef, etc.).
* **Retornar Valors**: Normalment, el Hook personalitzat retornarà un valor (una variable, un objecte o una funció) que el component que el crida pot utilitzar.

### Exemple Pràctic: useToggle

Es una lògica senzilla per canviar un valor booleà (true/false) en qualsevol component amb un sol clic.

#### Definició del Hook (useToggle.js)

```jsx
import { useState } from 'react';

// El Custom Hook comença amb 'use'
function useToggle(initialValue = false) {
  // 1. Utilitzem l'hook useState per gestionar l'estat booleà intern
  const [isToggled, setIsToggled] = useState(initialValue);

  // 2. Definim una funció per canviar l'estat
  const toggle = () => {
    setIsToggled(prev => !prev);
  };

  // 3. Retornem l'estat actual i la funció per canviar-lo
  // Podem retornar un array (com useState) o un objecte
  return [isToggled, toggle];
}

export default useToggle;
```

#### Utilització del Hook en un Component

Ara es pot utilitzar aquesta lògica d'activació/desactivació a qualsevol lloc sense reescriure l'estat i la funció toggle.

```jsx
import React from 'react';
import useToggle from './useToggle'; // Importem el nostre Hook

function ComponenteTema() {
  // 1. Cridem el Hook i obtenim l'estat i la funció
  const [isDark, toggleTheme] = useToggle(false); // S'inicialitza a false

  // El Hook conté la lògica d'estat, el component només la consumeix.

  const themeStyle = {
    backgroundColor: isDark ? '#333' : '#FFF',
    color: isDark ? '#FFF' : '#333',
    padding: '20px',
    borderRadius: '8px'
  };

  return (
    <div style={themeStyle}>
      <h2>Tema Actual: {isDark ? 'Fosc' : 'Clar'}</h2>
      
      {/* 2. Utilitzem la funció 'toggleTheme' retornada pel Hook */}
      <button onClick={toggleTheme}>
        Canviar Tema
      </button>
    </div>
  );
}

```

### Exemple amb `useFetch`

Encapsular la lògica de petició de dades (fetch) per poder-la reutilitzar en diversos components que consulten diferents APIs.

```jsx
// useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setError(null);
      })
      .catch(err => {
        setError(err);
        setData(null);
      })
      .finally(() => setLoading(false));
  }, [url]); // L'efecte es reactiva si la URL canvia

  return { data, loading, error };
}

export default useFetch;
```

Qualsevol component només ha de cridar `const { data, loading, error } = useFetch('/api/usuaris');` i ja té tota la lògica de càrrega i gestió d'errors implementada.
