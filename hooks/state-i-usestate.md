# State i useState

### State i useState

* Estat intern d'un component (mutable dins del component).

```jsx
import { useState } from 'react'

function Comptador() {
  const [count, setCount] = useState(0)
  return (
    <div>
      <p>Comptador: {count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementa</button>
    </div>
  )
}
```

* `useState(0)` crea un estat `count` inicialitzat a 0 i la funció `setCount` per actualitzar-lo.
* Al cridar `setCount`, React torna a renderitzar el component (**només el compoment** i no tot el html) amb el nou valor.
