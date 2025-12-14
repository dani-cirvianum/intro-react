# Peticions Http

* S'utilitzen per consumir APIs utilitzant async/await o promeses.
* `Async/await` fa el codi més lineal i llegible que promeses encadenades.

```jsx
import { useState, useEffect } from 'react'

function Usuaris() {
  const [users, setUsers] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    // No fem l'efecte async directament => definim una funció async dins
    async function fetchUsers() {
      try {
        setLoading(true)
        const res = await fetch('https://jsonplaceholder.typicode.com/users')
        if (!res.ok) throw new Error(`HTTP error! status: ${res.status}`)
        const data = await res.json()
        setUsers(data)
      } catch (err) {
        setError(err)
      } finally {
        setLoading(false)
      }
    }

    fetchUsers()
  }, [])

  if (loading) return <p>Carregant...</p>
  if (error) return <p>Error: {error.message}</p>
  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name} ({u.email})</li>)}
    </ul>
  )
}
```

* `useEffect` s'executa al muntatge.
* `fetchUsers` és `async` per usar `await`.
* `await fetch(...)` fa la crida
* comprovem `res.ok` per errors HTTP.
* `res.json()` parseja la resposta.
* `try/catch/finally` gestiona errors i l'estat `loading`.
* **Exercici** Modificar per fer POST d'un nou usuari fictici.

Hook reutilitzable amb async/await (useFetch):

```jsx
import { useState, useEffect } from 'react'

export function useFetch(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    let cancelled = false

    async function doFetch() {
      try {
        setLoading(true)
        const res = await fetch(url)
        if (!res.ok) throw new Error(`HTTP error: ${res.status}`)
        const json = await res.json()
        if (!cancelled) setData(json)
      } catch (err) {
        if (!cancelled) setError(err)
      } finally {
        if (!cancelled) setLoading(false)
      }
    }

    doFetch()
    return () => { cancelled = true } // prevenció de memòria després d'un unmount
  }, [url])

  return { data, loading, error }
}
```

* `cancelled` serveix per evitar actualitzar l'estat després que el component s'hagi desmuntat.
* El hook retorna `data`, `loading`, i `error` per facilitar l'ús en components.

```jsx
function UsersPage() {
  const { data: users, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users')

  if (loading) return <p>Carregant...</p>
  if (error) return <p>Error: {error.message}</p>

  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  )
}
```

* Es pot utilitzar`axios` amb async/await
  * La sintaxi seria `const res = await axios.get(url)` i `res.data` conté la resposta.
* Sempre s'ha de gestionar els errors i l'estat de càrrega per millorar UX (pensar en l'accessibilitat i la usabilitat).
