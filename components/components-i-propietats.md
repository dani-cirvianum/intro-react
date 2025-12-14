# Components i propietats

### Components

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

### Propietats

* Paràmetres que passen dades de pare a fill.

```jsx
function Salutacio({ nom }) {
  return <p>Hola, {nom}!</p>
}

// ús:
<Salutacio nom="Pau" />
```

* `Salutacio` rep `nom` i renderitza el text.
