# Efectes i UseEffect

### Efectes i useEffect

* S'utilitzen per **efectes secundaris**: fetch, timers, subscripcions
* `useEffect` substitueix els mètodes de cicle de vida de les classes
* La sintaxis de `useEffect` pot tenir **2 arguments**
  * Una funció de configuració (l'efecte)
  * Un array de dependències (opcional)

```javascript
useEffect(() => {
  // 1. Codi de l'efecte (es executa després del render)

  return () => {
    // Funció de "neteja" (cleanup, opcional)
    // Es executa abans que el component es desmunti 
    // o abans de la següent execució de l'efecte.
  };
}, [dependència1, dependència2]); // 2. Array de dependències
```

* **La funció d'efecte**: s'executarà després de que el component s'hagi renderitzat a la pantalla. D'aquesta manera es garanteix que el DOM estigui actualitzat.
* &#x4C;**'array de dependències**: controla quan es torna a executar la funció d'efecte
  * **Sense array**: S'executa després de cada renderització del component.
  * **Array buit(\[])**: S'executa només una vegada (després del primer render)
  * **Amb valors(\[a,b])**: S'executa després del primer render i quan canvia el valor d'algun dels elements (a o b).
* **La funció de neteja**: Aquesta funció s'executa abans que la funció s'executi de nou (si alguna de les dependències es modifica) o bé quan el component es desmunta (per alliberar memòria).

```jsx
import { useState, useEffect } from 'react'

function Rellotge() {
  const [time, setTime] = useState(new Date())

  useEffect(() => {
    const id = setInterval(() => setTime(new Date()), 1000)
    return () => clearInterval(id)
  }, [])
  return <div>{time.toLocaleTimeString()}</div>
}
```

* L'efecte s'executa una vegada al muntatge (`[]`).
* S'inicia un interval que actualitza `time` cada segon.
* Es retorna una funció de neteja per aturar l'interval quan el component es desmunta.

```jsx
import { useState, useEffect } from 'react'

function Usuari({ id }) {
  const [usuari, setUsuari] = useState(null)

  useEffect(() => {
    fetch(`https://api.example.com/users/${id}`)
      .then(res => res.json())
      .then(data => setUsuari(data))
  }, [id]) // Depèn de l'id: es fa un refetch si canvia

  if (!usuari) return <div>Carregant...</div>
  return <div>{usuari.name}</div>
}
```

**Nota sobre useEffect + async**: no fem la funció del efecte `async` directament sinó que definim una funció `async` dins l'efecte (veure apartat fetch)

```jsx
import { useState, useEffect } from 'react'

function Usuari({ id }) {
  const [usuari, setUsuari] = useState(null)

  useEffect(() => {
    async function fetchUser() {
      const res = await fetch(`https://api.example.com/users/${id}`);
      const data = await res.json();
      setUsuari(data);
    }
    fetchUser()
  }, [id]) // Depèn de l'id: es fa un refetch si canvia

  if (!usuari) return <div>Carregant...</div>
  return <div>{usuari.name}</div>
}
```

* L'efecte s'executa cada vegada que `id` es modifica

```jsx
import { useEffect } from 'react'

function ScrollLogger() {
  useEffect(() => {
    const handleScroll = () => console.log(window.scrollY)
    window.addEventListener('scroll', handleScroll)

    return () => window.removeEventListener('scroll', handleScroll) // neteja
  }, []) // només al muntatge/desmuntatge

  return <div style={{ height: '200vh' }}>Scroll per veure la consola</div>
}
```
