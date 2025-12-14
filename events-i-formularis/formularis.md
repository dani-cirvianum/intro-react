# Formularis

### Formularis

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

```jsx
import { useState } from 'react'

function FormulariUsuari() {
  const [form, setForm] = useState({ nom: '', email: '' })

  const handleChange = e => {
    setForm({ ...form, [e.target.name]: e.target.value })
  }

  const handleSubmit = e => {
    e.preventDefault()
    console.log('Dades enviades:', form)
  }

  return (
    <form onSubmit={handleSubmit}>
      <input 
        name="nom" 
        value={form.nom} 
        onChange={handleChange} 
        placeholder="Nom" 
      />
      <input 
        name="email" 
        value={form.email} 
        onChange={handleChange} 
        placeholder="Email" 
      />
      <button>Enviar</button>
    </form>
  )
}
```

* Cada input té un `name` que coincideix amb la clau de l'estat
* `handleChange` actualitza nomnés el camp correspoentn utilitzant `...form`
