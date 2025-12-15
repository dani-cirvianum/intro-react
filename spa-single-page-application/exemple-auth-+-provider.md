# Exemple Auth + Provider

## SPA React + Vite + AuthProvider

### Crear projecte amb Vite

```bash
npm create vite@latest react-auth-layout
cd react-auth-layout
npm install
npm install react-router-dom
```

* Haurem d'escollir la opció de react amb javascript

### Estructura de carpetes

```
src/
 ├─ main.jsx
 ├─ App.jsx
 ├─ context/
 │    ├─ AuthContext.js
 ├─ auth/
 │    ├─ AuthProvider.jsx
 │    └─ PrivateRoute.jsx
 ├─ layout/
 │    └─ AdminLayout.jsx
 ├─ pages/
 │    ├─ Dashboard.jsx
 │    ├─ Login.jsx
 │    ├─ NotFound.jsx
 │    └─ Settings.jsx
 └─ styles.css (opcional)
```

### context/AuthContext.js

```jsx
import { createContext } from "react";
export const AuthContext = createContext();
```

### auth/AuthProvider.jsx

```jsx
import { useState, useEffect } from "react";
import {AuthContext} from "../context/AuthContext";

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
        async function fetchUser() {
          const stored = localStorage.getItem("user");
          if (stored) setUser(JSON.parse(stored)); 
        }
        fetchUser();
    
  }, []);

  const login = (username, password) => {
    /* 
	Exemple senzill:  comprovem admin i 1234
	En un entorn real s'hauria de comprovar a través api per validar 
	*/
    if (username === "admin" && password === "1234") {
      const userData = { username };
      setUser(userData);
      localStorage.setItem("user", JSON.stringify(userData));
      return true;
    }
    return false; // Si no es admin/1234
  };

  const logout = () => {
	/*
	 En aquest exemple senzillament esborrem del localStorage
	 En un entorn real s'hauria de fer la crida al servidor 
	 */
    setUser(null);
    localStorage.removeItem("user");
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}


```

### auth/PrivateRoute.jsx

```jsx
import { Navigate, Outlet } from "react-router-dom";
import { useContext } from "react";
import { AuthContext } from "../context/AuthContext";

export default function PrivateRoute() {
 
  const { user } = useContext(AuthContext);  
  if (!user) {
    return <Navigate to="/login" replace />;
  }

  return <Outlet />;
}
```

###

### pages/Login.jsx

```jsx
import { useState, useContext } from "react";
import { useNavigate } from "react-router-dom";
import { AuthContext } from "../context/AuthContext";

export default function Login() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const navigate = useNavigate();
  const { login } = useContext(AuthContext);

  const handleSubmit = (e) => {
    e.preventDefault();

    const ok = login(username, password);

    if (!ok) {
      setError("Credencials incorrectes");
      return;
    }

    navigate("/"); // Si l'usuari s'ha identificat correctament redirigim a l'arrel
  };

  return (
    <div>
		<form onSubmit={handleSubmit}>
        	<h2>Login</h2>
		  	{error && <div style={{color: "red"}}>{error}</div>}
		  	<input type="text" placeholder="User name" value={username}
        		onChange={(e) => setUsername(e.target.value)}
      		/>
			<br />
			<input type="password" placeholder="Password" value={password}
        		onChange={(e) => setPassword(e.target.value)}
      		/>
			<br/>
			<button type="submit">Entrar</button>
		</form>
    </div>
  );
}
```

### layout/AdminLayout.jsx

```jsx
import { Link, Outlet } from "react-router-dom";
import { useContext } from "react";
import { AuthContext } from "../context/AuthContext";

export default function AdminLayout() {
  const { logout } = useContext(AuthContext);

  return (
    <div>
		<div>
			<nav>
				<Link to="/">Dashboard</Link>
				<Link to="/settings">Settings</Link>	
			</nav>
			<button onClick={logout}>Logout</button>	
		</div>
		<div>
			<Outlet /> {/* Aquí es mostraran els continguts*/ }
		</div>
    </div>
  );
}
```

### pages/Dashboard.jsx

```jsx
export default function Dashboard() {
  return (
    <div>
		<h2>Dashboard</h2>
		<div>
			Benvingut al dashboard
		</div>
    </div>
  );
}
```

### pages/Settings.jsx

```jsx
export default function Settings() {
  return (
    <div>
    	<h2>Settings</h2>
		<div>
			Aquesta és la secció de settings només per usuaris loguejats
		</div>
    </div>
  );
}
```

### pages/NotFound.jsx

```jsx
import { Link } from 'react-router-dom';

export default function NotFound() {
  return (
    <div>
      <h1>404 - Pàgina No Trobada</h1>
      <p>La pàgina que busques no existeix.</p>
      <Link to="/">Torna a la pàgina d'inici</Link>
    </div>
  );
}
```

### App.jsx

```jsx
import { Routes, Route } from "react-router-dom";
import Login from "./pages/Login";
import Dashboard from "./pages/Dashboard";
import Settings from "./pages/Settings";
import AdminLayout from "./layout/AdminLayout";
import PrivateRoute from "./auth/PrivateRoute";
import NotFound from "./pages/NotFound";

export default function App() {
  return (
    <Routes>
      {/* Ruta pública */}
      <Route path="/login" element={<Login />} />

      {/* Rutes privades */}
      <Route path="/" element={<PrivateRoute />}>
        <Route element={<AdminLayout />}>
          <Route index element={<Dashboard />} />
          <Route path="settings" element={<Settings />} />

		  
        </Route>
      </Route>
	  {/* Aquesta és la ruta 404 de comodí per si no troba les altres rutes. Ha d'anar al final. */}
        <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

### main.jsx

```jsx
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";
import { AuthProvider } from "./auth/AuthProvider";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <AuthProvider>
      <App />
    </AuthProvider>
  </BrowserRouter>
);
```
