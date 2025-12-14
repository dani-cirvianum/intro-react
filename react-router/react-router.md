# React Router

## React Router: Gestió de Rutes

### Què és React Router?

* És una llibreria per a React que permet gestionar la navegació dins d'una **SPA** (Single Page Application) sense recarregar la pàgina.&#x20;
* En funció de la URL, React Router decideix **quin component s'ha de renderitzar**.

Els avantatges principals són:

* Experiència d'usuari fluida (sense recarregar)
* Manteniment de l’estat global de l’app
* Navegació ràpida
* Arquitectura modular i escalable

### Instal·lació

```bash
npm install react-router-dom
```

### Configuració bàsica

```jsx
// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

* `BrowserRouter` ha d’envoltar tota l’aplicació.
* Només n'hi ha d’haver un a l’app.
* Gestiona l’historial i les URL del navegador.

### Declaració de rutes

```jsx
//App.jsx
import { Routes, Route, Link } from "react-router-dom";
import Home from "./Home";
import About from "./About";

export default function App() {
  return (
    <div>
      <nav>
        <Link to="/">Inici</Link> | 
        <Link to="/about">Sobre nosaltres</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </div>
  );
}
```

* `Routes` conté totes les rutes.
* `Route` associa un path a un component.
* `React Router` no recarrega la pàgina, només canvia el component mostrat.

### Rutes dinàmiques

```jsx
<Route path="/users/:id" element={<User />} />
```

```jsx
import { useParams } from "react-router-dom";

export default function User() {
  const { id } = useParams();
  return <h2>Usuari amb ID: {id}</h2>;
}
```

* `useParams()` llegeix variables a la URL.
* Exemples:
  * /users/1
  * /users/42

### Navegació a través de codi

```jsx
import { useNavigate } from "react-router-dom";

export default function Login() {
  const navigate = useNavigate();

  function login() {
    navigate("/dashboard");
  }

  return <button onClick={login}>Entrar</button>
}
```

* Permet navegar des de codi.
* Útil després de login, logout, formularis, etc.

### Nested Routes

* Les nested routes permeten tenir rutes "dins" d'altres rutes.
* Això és molt útil per dashboards o seccions que comparteixen layout.

```jsx
//App.jsx
import DashboardRoutes from "./DashboardRoutes";

<Routes>
  <Route path="/dashboard/*" element={<DashboardRoutes />} />
</Routes>
```

```jsx
//DashboardRoutes.jsx
import { Routes, Route, Link } from "react-router-dom";
import Overview from "./Overview";
import Settings from "./Settings";

export default function DashboardRoutes() {
  return (
    <div>
      <h2>Dashboard</h2>

      <nav>
        <Link to="/dashboard/overview">Overview</Link> | 
        <Link to="/dashboard/settings">Configuració</Link>
      </nav>

      <Routes>
        <Route path="overview" element={<Overview />} />
        <Route path="settings" element={<Settings />} />
      </Routes>
    </div>
  );
}
```

* Només canvia la part interna del dashboard.
* La resta del layout es manté.
* Rutes resultants
  * /dashboard/overview
  * /dashboard/settings

### Rutes protegides

```jsx
//ProtectedRoute.jsx
import { Navigate } from "react-router-dom";
const isLoggedIn = false;

export default function ProtectedRoute({ children }) {
  if (!isLoggedIn) return <Navigate to="/" replace />;
  return children;
}
```

```jsx
<Route path="/dashboard" element={
  <ProtectedRoute>
    <Dashboard />
  </ProtectedRoute>
} />
```

* Si l’usuari no està loguejat: redirecció
* Si l'usuari sí està loguejat: renderitza el component

### Utilització de constants de rutes (útil per manteniment)

```jsx
//routes.js
export const ROUTES = {
  HOME: "/",
  ABOUT: "/about",
  DASHBOARD: "/dashboard",
  USER: (id) => `/users/${id}`
};
```

```jsx
<Link to={ROUTES.DASHBOARD}>Dashboard</Link>
<Link to={ROUTES.USER(42)}>Usuari 42</Link>
```

* Avantatges
  * Si canviem `/dashboard`: només ho canvies a un lloc
  * Evitem errors per `typos`
  * Creem rutes més clares i reutilitzables

### Per què s'utilitza?

* Construir SPAs sense recàrrega de pàgina
* Gestionar rutes dinàmiques
* Fer nested routes modulars
* Protegir rutes
* Escalar aplicacions
* Mantenir el codi net amb constants
* És essencial per qualsevol app React "actual".
