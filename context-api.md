# Context Api

## React Context API

* El Context API és un hook de React per compartir estat entre components sense haver de passar propietats manualment.
* En lloc d’això:
  * Creem un context
  * Proveïm dades amb un `Provider`
  * Les consumin des de qualsevol component amb `useContext`
* És ideal per a:
  * Usuari loguejat
  * Tema (dark/light)
  * Configuracions globals
  * Carret de compra
* No és ideal quan es tracta d'una aplicació altament mutable (que canvia constantment) i que requereix molta optimització.

## Exemples bàsics

### Crear un context

```jsx
// context/ThemeContext.js
import { createContext } from "react";

export const ThemeContext = createContext();
```

### Afegir el Provider

```jsx
// context/ThemeProvider.jsx
import { useState } from "react";
import { ThemeContext } from "./ThemeContext";

export default function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () =>
    setTheme((prev) => (prev === "light" ? "dark" : "light"));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

### Consumir el context

```jsx
// components/Header.jsx
import { useContext } from "react";
import { ThemeContext } from "../context/ThemeContext";

export default function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <header>
      <p>Theme actual: {theme}</p>
      <button onClick={toggleTheme}>Canviar tema</button>
    </header>
  );
}
```

### Envoltar/encapsular la app

```jsx
// main.jsx
import ThemeProvider from "./context/ThemeProvider";

ReactDOM.createRoot(document.getElementById("root")).render(
  <ThemeProvider>
    <App />
  </ThemeProvider>
);
```

## Composició de múltiples contexts

* Pot haver-hi múltiples contexts:

```jsx
ReactDOM.createRoot(document.getElementById("root")).render(
  <AuthProvider>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </AuthProvider>
);
```

## Exemple amb autenticació

### Crear el context

```jsx
// context/AuthContext.js
import { createContext } from "react";

export const AuthContext = createContext();
```

### Provider amb user i login/logout

```jsx
// context/AuthProvider.jsx
import { useState } from "react";
import { AuthContext } from "./AuthContext";

export default function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (name) => setUser({ name });
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

### Com utilitzar-lo

```jsx
// components/Dashboard.jsx
import { useContext } from "react";
import { AuthContext } from "../context/AuthContext";

export default function Dashboard() {
  const { user, logout } = useContext(AuthContext);

  if (!user) return <p>No estàs loguejat</p>;

  return (
    <>
      <p>Hola {user.name}</p>
      <button onClick={logout}>Tanca sessió</button>
    </>
  );
}
```

## Estructura de fitxers recomanada

```
src/
  context/
    ThemeContext.js
    ThemeProvider.jsx
    AuthContext.js
    AuthProvider.jsx
    index.js
```

```jsx
// index.js centralitza exports
export { ThemeContext } from "./ThemeContext";
export { default as ThemeProvider } from "./ThemeProvider";

export { AuthContext } from "./AuthContext";
export { default as AuthProvider } from "./AuthProvider";
```

## Context + Fetch API

```jsx
// context/UserProvider.jsx
import { createContext, useContext, useEffect, useState } from "react";

export const UserContext = createContext();

export default function UserProvider({ children }) {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("/api/users").then(r => r.json()).then(setUsers);
  }, []);

  return (
    <UserContext.Provider value={{ users }}>
      {children}
    </UserContext.Provider>
  );
}
```

```jsx
import { useContext } from "react";
import { UserContext } from "../context/UserProvider";

function UserList() {
  const { users } = useContext(UserContext);
  return users.map((u) => <li key={u.id}>{u.name}</li>);
}
```

## Custom Hook Pattern

* Evita haver d’importar el context + useContext cada cop:

```jsx
// context/useAuth.js
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

export default function useAuth() {
  return useContext(AuthContext);
}
```

```jsx
const { user, login } = useAuth();
```

## Quan NO utilitzar Context

* No s'ha d'utilitzar quan:
  * L'estat canvia molt sovint
  * Es necessita caching, persistència, middlewares
  * Dades massives (arrays enormes)
  * Dades locals d’un sol component
