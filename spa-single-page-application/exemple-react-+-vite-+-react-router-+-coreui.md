# Exemple React + Vite + React-router + CoreUI

## SPA React + Vite + CoreUI+Auth

### Crear projecte amb Vite

```bash
npm create vite@latest react-coreui-auth
cd react-coreui-auth
```

* Haurem d'escollir la opció de react amb javascript

### Instal·lar dependències

```bash
npm install @coreui/coreui @coreui/react @coreui/icons-react react-router-dom
```

### Estructura de carpetes

```
src/
 ├─ main.jsx
 ├─ App.jsx
 ├─ auth/
 │    ├─ AuthContext.jsx
 │    └─ PrivateRoute.jsx
 ├─ layout/
 │    └─ AdminLayout.jsx
 ├─ pages/
 │    ├─ Login.jsx
 │    ├─ Dashboard.jsx
 │    └─ Settings.jsx
 └─ styles.css (opcional)
```

### auth/auth.js

```jsx
import { createContext } from "react";
const AuthContext = createContext();
```

### auth/AuthContext.js

```jsx
import { useState, useEffect } from "react";
import {AuthContext} from "./AuthContext";

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
    // Exemple senzill → substitueix-ho per crida a Laravel
    if (username === "admin" && password === "1234") {
      const userData = { username };
      setUser(userData);
      localStorage.setItem("user", JSON.stringify(userData));
      return true;
    }
    return false;
  };

  const logout = () => {
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
import { useAuth } from "./AuthContext";

export default function PrivateRoute() {
  const { user } = useAuth();

  if (!user) {
    return <Navigate to="/login" replace />;
  }

  return <Outlet />;
}
```

### layout/AdminLayout.jsx

```jsx
import {
  CContainer,
  CHeader,
  CHeaderBrand,
  CSidebar,
  CSidebarNav,
  CNavItem,
  CButton,
} from "@coreui/react";

import { Link, Outlet } from "react-router-dom";
import { useAuth } from "../auth/AuthContext";

export default function AdminLayout() {
  const { logout } = useAuth();

  return (
    <div className="d-flex">
      <CSidebar visible={true}>
        <CSidebarNav>
          <CNavItem>
            <Link to="/">Dashboard</Link>
          </CNavItem>

          <CNavItem>
            <Link to="/settings">Settings</Link>
          </CNavItem>

          <hr />

          <CButton color="danger" className="m-3" onClick={logout}>
            Logout
          </CButton>
        </CSidebarNav>
      </CSidebar>

      <div className="flex-grow-1">
        <CHeader>
          <CHeaderBrand>React Admin</CHeaderBrand>
        </CHeader>

        <CContainer className="mt-4">
          <Outlet />
        </CContainer>
      </div>
    </div>
  );
}
```

### pages/Login.jsx

```jsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { useAuth } from "../auth/AuthContext";

import {
  CCard,
  CCardBody,
  CForm,
  CFormInput,
  CButton,
} from "@coreui/react";

export default function Login() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const navigate = useNavigate();
  const { login } = useAuth();

  const handleSubmit = (e) => {
    e.preventDefault();

    const ok = login(username, password);

    if (!ok) {
      setError("Credencials incorrectes");
      return;
    }

    navigate("/");
  };

  return (
    <div className="d-flex justify-content-center align-items-center vh-100">
      <CCard style={{ width: "350px" }}>
        <CCardBody>
          <h3 className="text-center mb-3">Login</h3>

          {error && <div className="text-danger mb-2">{error}</div>}

          <CForm onSubmit={handleSubmit}>
            <CFormInput
              label="Usuari"
              className="mb-3"
              value={username}
              onChange={(e) => setUsername(e.target.value)}
            />

            <CFormInput
              type="password"
              label="Contrasenya"
              className="mb-3"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
            />

            <CButton type="submit" color="primary" className="w-100">
              Entrar
            </CButton>
          </CForm>
        </CCardBody>
      </CCard>
    </div>
  );
}
```

### pages/Dashboard.jsx

```jsx
import { CCard, CCardBody } from "@coreui/react";

export default function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>

      <CCard>
        <CCardBody>Benvingut al dashboard protegit.</CCardBody>
      </CCard>
    </div>
  );
}
```

### pages/Settings.jsx

```jsx
import { CCard, CCardBody } from "@coreui/react";

export default function Settings() {
  return (
    <div>
      <h2>Settings</h2>

      <CCard>
        <CCardBody>Aquesta és una secció només per usuaris loguejats.</CCardBody>
      </CCard>
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
    </Routes>
  );
}
```

### main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App.jsx";

import "@coreui/coreui/dist/css/coreui.min.css";
import { AuthProvider } from "./auth/AuthContext";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <AuthProvider>
      <App />
    </AuthProvider>
  </BrowserRouter>
);
```

### Actualitzar per utilitzar CRUD d'usuaris

```
src/
 ├─ pages/
 │    ├─ users/
 │    │    ├─ UsersList.jsx
 │    │    └─ UserForm.jsx
```

### Afegir la ruta al layout

* `AdminLayout.jsx` (només afegeix el link Users)
* Buscar la secció del menú i afegir:

```jsx
<CNavItem>
  <Link to="/users">Users</Link>
</CNavItem>
```

### Afegir la ruta al router

* Afegir a `App.jsx`

```jsx
import UsersList from "./pages/users/UsersList";

...

<Route path="users" element={<UsersList />} />
```

### pages/users/UsersList.jsx

* Llista + botó nou + botó editar + eliminar + modal per formulari.

```jsx
import { useState } from "react";
import {
  CCard,
  CCardBody,
  CButton,
  CTable,
  CTableBody,
  CTableHead,
  CTableHeaderCell,
  CTableRow,
  CTableDataCell,
  CModal,
  CModalBody,
  CModalHeader,
  CModalTitle,
} from "@coreui/react";

import UserForm from "./UserForm";

export default function UsersList() {
  const [users, setUsers] = useState([
    { id: 1, name: "Joan", email: "joan@example.com" },
    { id: 2, name: "Marta", email: "marta@example.com" },
  ]);

  const [visible, setVisible] = useState(false);
  const [editingUser, setEditingUser] = useState(null);

  const handleCreate = () => {
    setEditingUser(null);
    setVisible(true);
  };

  const handleEdit = (user) => {
    setEditingUser(user);
    setVisible(true);
  };

  const handleDelete = (id) => {
    if (!confirm("Vols eliminar aquest usuari?")) return;
    setUsers(users.filter((u) => u.id !== id));
  };

  const handleSubmit = (formData) => {
    if (editingUser) {
      // Editar
      setUsers(
        users.map((u) => (u.id === editingUser.id ? { ...u, ...formData } : u))
      );
    } else {
      // Crear
      const newUser = {
        id: Math.max(...users.map((u) => u.id), 0) + 1,
        ...formData,
      };
      setUsers([...users, newUser]);
    }

    setVisible(false);
  };

  return (
    <div>
      <h2>Gestió d'usuaris</h2>

      <CCard>
        <CCardBody>
          <CButton color="primary" onClick={handleCreate} className="mb-3">
            Nou usuari
          </CButton>

          <CTable bordered hover responsive>
            <CTableHead color="dark">
              <CTableRow>
                <CTableHeaderCell>ID</CTableHeaderCell>
                <CTableHeaderCell>Nom</CTableHeaderCell>
                <CTableHeaderCell>Email</CTableHeaderCell>
                <CTableHeaderCell>Accions</CTableHeaderCell>
              </CTableRow>
            </CTableHead>

            <CTableBody>
              {users.map((user) => (
                <CTableRow key={user.id}>
                  <CTableDataCell>{user.id}</CTableDataCell>
                  <CTableDataCell>{user.name}</CTableDataCell>
                  <CTableDataCell>{user.email}</CTableDataCell>
                  <CTableDataCell>
                    <CButton
                      color="warning"
                      size="sm"
                      onClick={() => handleEdit(user)}
                      className="me-2"
                    >
                      Editar
                    </CButton>

                    <CButton
                      color="danger"
                      size="sm"
                      onClick={() => handleDelete(user.id)}
                    >
                      Esborrar
                    </CButton>
                  </CTableDataCell>
                </CTableRow>
              ))}
            </CTableBody>
          </CTable>
        </CCardBody>
      </CCard>

      {/* Modal del formulari */}
      <CModal visible={visible} onClose={() => setVisible(false)}>
        <CModalHeader>
          <CModalTitle>
            {editingUser ? "Editar usuari" : "Nou usuari"}
          </CModalTitle>
        </CModalHeader>

        <CModalBody>
          <UserForm
            user={editingUser}
            onSubmit={handleSubmit}
            onCancel={() => setVisible(false)}
          />
        </CModalBody>
      </CModal>
    </div>
  );
}
```

### pages/users/UserForm.jsx

* Formulari reutilitzable per crear i editar.

```jsx
import { useState, useEffect } from "react";
import { CForm, CFormInput, CButton } from "@coreui/react";

export default function UserForm({ user, onSubmit, onCancel }) {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  useEffect(() => {
    if (user) {
      setName(user.name);
      setEmail(user.email);
    } else {
      setName("");
      setEmail("");
    }
  }, [user]);

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit({ name, email });
  };

  return (
    <CForm onSubmit={handleSubmit}>
      <CFormInput
        label="Nom"
        value={name}
        onChange={(e) => setName(e.target.value)}
        className="mb-3"
        required
      />

      <CFormInput
        label="Email"
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        className="mb-3"
        required
      />

      <div className="text-end">
        <CButton color="secondary" className="me-2" onClick={onCancel}>
          Cancel·lar
        </CButton>

        <CButton type="submit" color="primary">
          Guardar
        </CButton>
      </div>
    </CForm>
  );
}
```
