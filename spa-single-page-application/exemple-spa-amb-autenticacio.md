# Exemple SPA amb Autenticació

## Exemple SPA amb React, Vite, JSON-Server i Autenticació JWT

### Configuració del backend (Fake Api)

S'ha de crear un nou porjecte i configurar el json

```bash
mkdir json-server
cd json-server
npm install  json-server@0.17 json-server-auth
```

Fitxer db.json

```json
{
  "users": [ 
    {
      "email": "test@test.com",
      "password": "$2a$10$Uo0aWFmVDHL00VF0qR7BD.dug9wdl0dKCODF4NtAJCaoqfomeMC4u",
      "id": 1
    }
  ],
  "books": [
    {
      "id": 1,
      "title": "El Quixot",
      "author": "Cervantes"
    },
    {
      "id": 2,
      "title": "1984",
      "author": "George Orwell"
    }
  ]
}
```

Arrencar servidor

```bash
npx json-server-auth db.json --port 3001
```

Amb el json anterior podem utilitzar l'email `test@test.com` i el password `123456`

Les rutes protegides requereixen JWT al header Authorization: Bearer .

### Configuració del frontend amb Vite + React

```bash
npm create vite@latest react-books-auth --- --template react -y
cd react-books-auth
npm install
npm install axios react-router-dom
```

### Estructura de carpetes

```
src/
api/
    axios.js
  components/
    Login.jsx
    BookList.jsx
    BookForm.jsx
  pages/
    Home.jsx
    Books.jsx
  App.jsx
  main.jsx
```

### Configuració d’axios amb token

```jsx
// src/api/axios.js
import axios from "axios";

const api = axios.create({
  baseURL: "http://localhost:3001",
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

### Component de Login

```jsx
// src/components/Login.jsx
import { useState } from "react";
import api from "../api/axios";

export default function Login({ onLogin }) {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const res = await api.post("/login", { email, password });
      localStorage.setItem("token", res.data.accessToken);
      onLogin();
    } catch (err) {
      alert("Error d'autenticació");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      /><br/>
      <input
        type="password"
        placeholder="Contrasenya"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      /><br/>
      <button type="submit">Entrar</button>
    </form>
  );
}
```

### CRUD de llibres

Llista de llibres

```jsx
// src/components/BookList.jsx
import { useEffect, useState } from "react";
import api from "../api/axios";

export default function BookList() {
  const [books, setBooks] = useState([]);

  useEffect(() => {
    api.get("/books").then((res) => setBooks(res.data));
  }, []);

  const deleteBook = async (id) => {
    await api.delete(`/books/${id}`);
    setBooks(books.filter((b) => b.id !== id));
  };

  return (
    <div>
      <h2>Llibres</h2>
      <ul>
        {books.map((b) => (
          <li key={b.id}>
            {b.title} - {b.author}
            <button onClick={() => deleteBook(b.id)}>Eliminar</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

Formulari per afegir llibres

```jsx
// src/components/BookForm.jsx
import { useState } from "react";
import api from "../api/axios";

export default function BookForm({ onAdd }) {
  const [title, setTitle] = useState("");
  const [author, setAuthor] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();
    const res = await api.post("/books", { title, author });
    onAdd(res.data);
    setTitle("");
    setAuthor("");
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Afegir llibre</h3>
      <input
        placeholder="Títol"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      /><br/>
      <input
        placeholder="Autor"
        value={author}
        onChange={(e) => setAuthor(e.target.value)}
      /><br/>
      <button type="submit">Afegir</button>
    </form>
  );
}
```

### Pàgina principal amb rutes

```jsx
// src/App.jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { useState } from "react";
import Login from "./components/Login";
import BookList from "./components/BookList";
import BookForm from "./components/BookForm";

export default function App() {
  const [isAuth, setIsAuth] = useState(!!localStorage.getItem("token"));

  return (
    <BrowserRouter>
      <Routes>
        <Route
          path="/"
          element={
            isAuth ? (
              <div>
                <BookForm onAdd={() => {}} />
                <BookList />
              </div>
            ) : (
              <Login onLogin={() => setIsAuth(true)} />
            )
          }
        />
      </Routes>
    </BrowserRouter>
  );
}
```

* Usuari fa login i json-server-auth retorna un JWT.
* El token es guarda a localStorage.
* Axios afegeix el token a cada petició.
* Només els usuaris autenticats poden fer CRUD sobre /books.

**En aquest punt ja tenim l'autenticació bàsica que només permet realitzar el CRUD de llibres si l'usuari s'ha autenticat prèviament.**

En els passos següents ampliarem el mateix projecte.

## Protecció de rutes amb React Router

### Crear un component PrivateRoute

```jsx
// src/components/PrivateRoute.jsx
import { Navigate } from "react-router-dom";

export default function PrivateRoute({ children }) {
  const token = localStorage.getItem("token");
  return token ? children : <Navigate to="/login" />;
}
```

En aquest exemple simularem que l'usuari està autenticat o no en funció de si hi ha un tolen o no guardat a `localStorage`. **Això només serveix per a aquest exemple educatiu i s'ha d'evitar en entorns reals o en producció**.

* **Si hi ha token** (usuari prèviament loguejat) : s'ha de renderitzar el contingut. Mostra els continguts dels fills amb el paràmetre `children`.
* **Si no hi ha token** (usuari no loguejat):  es redirigeix a `/login`.

### Modifiquem el fitxer de Login.jsx

```jsx
// src/components/Login.jsx
import { useState } from "react";
import api from "../api/axios";
import {  useNavigate } from "react-router-dom";

export default function Login() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
	const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const res = await api.post("/login", { email, password });
      localStorage.setItem("token", res.data.accessToken);
      navigate("/")
	  
    } catch (err) {
      alert("Error d'autenticació");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      /><br/>
      <input
        type="password"
        placeholder="Contrasenya"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      /><br/>
      <button type="submit">Entrar</button>
    </form>
  );
}
```

### Actualitzar les rutes a App.js &#x20;

```jsx
// src/App.jsx
import { BrowserRouter, Routes, Route, Navigate } from "react-router-dom";
import Login from "./components/Login";
import BookList from "./components/BookList";
import BookForm from "./components/BookForm";
import PrivateRoute from "./components/PrivateRoute";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<Login  />} />

        <Route
          path="/books"
          element={
            <PrivateRoute>
              <div>
                <BookForm onAdd={() => {}} />
                <BookList />
              </div>
            </PrivateRoute>
          }
        />

        {/* Exemple per autors */}
        <Route
          path="/authors"
          element={
            <PrivateRoute>
              <div>
                <h2>CRUD d’autors</h2>
                {/* Aquí podries afegir AuthorForm i AuthorList */}
              </div>
            </PrivateRoute>
          }
        />

        {/* Ruta per defecte */}
        <Route path="/" element={<Navigate to="/books" />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Consideracions

Lògicament, aquest és un exemple acadèmic molt simple i que s'hauria d'acabar d'implementar el cruds, altres opcions, logout, etc.

### Flux complet

* L’usuari entra a `/login` i fa login: es guarda el token.
* Si intenta accedir a `/books` o `/authors` sense token: redirigit a `/login`.
* Amb token: pot fer CRUD sobre llibres i autors.
