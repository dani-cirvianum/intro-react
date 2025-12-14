# UseRef

### Què és `useRef`⁣?

* Permet guardar valors mutables entre renderitzacions sense provocar una nova renderització quan aquests canvien.
* Permet crear una referència (`Ref`) a un element del DOM o emmagatzemar qualsevol valor que cal mantenir constant durant tot el cicle de vida del component.

### Funcionament i Propòsits

`useRef` retorna un objecte anomenat `Ref Object` que té una única propietat: `.current`.

```javascript
const myRef = useRef(initialValue);
// myRef és { current: initialValue }
```

### Accedir a Elements del DOM

* És la manera més comuna d'utilitzar `useRef`
* Permet interactuar directament amb elements del DOM, cosa que no es pot fer de forma nativa en la lògica de React.

**Exemple**: Enfocar un camp de text quan es carrega el component.

```jsx
import React, { useRef } from 'react';

function CampFocus() {
  // 1. Creem la referència
  const inputRef = useRef(null);

  const handleClick = () => {
    // 3. Accedim al DOM i cridem el mètode .focus()
    inputRef.current.focus(); 
  };

  return (
    <div>
      {/* 2. Enllacem la referència a l'element del DOM */}
      <input type="text" ref={inputRef} /> 
      <button onClick={handleClick}>Enfoca el camp</button>
    </div>
  );
}
```

### Emmagatzemar Valors Mutables

* A diferència de l'estat (`useState`), canviar el valor de `.current` d'un Ref no provoca una nova renderització.
* S'utilitza per guardar valors que necessiten persistència, però que no han d'actualitzar la interfície d'usuari (UI).

**Exemple**: Guardar l'ID d'un temporitzador (`setInterval`) per poder-lo aturar més tard.

```jsx
import React, { useRef, useEffect } from 'react';

function Temporitzador() {
  const intervalRef = useRef(null);
  
  useEffect(() => {
    // Guardem l'ID del temporitzador a .current
    intervalRef.current = setInterval(() => {
      console.log('Tick!');
    }, 1000);

    // La funció de neteja
    return () => {
      // Accedim a l'ID i l'aturarem quan el component es desmunti
      clearInterval(intervalRef.current);
    };
  }, []); // [] Array buit: s'executa només al muntar/desmuntar

  return <h1>Revisa la consola per veure el temporitzador</h1>;
}
```

```jsx
const countRef = useRef(0)
countRef.current++
```
