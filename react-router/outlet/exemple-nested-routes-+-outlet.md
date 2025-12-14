# Exemple nested routes + outlet

## Exemple rutes simples + nested routes

### Estructura de fitxers

```
src/
 ├─ main.jsx
 ├─ App.jsx
 └─ pages/
     ├─ Home.jsx
     ├─ About.jsx
     ├─ Books.jsx
     ├─ BookList.jsx
     └─ BookForm.jsx
```

### main.jsx – Router global

```jsx
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

### App.jsx – Rutes principals + navegació

```jsx
import { Routes, Route, Link } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";
import Books from "./pages/Books";
import BookList from "./pages/BookList";
import BookForm from "./pages/BookForm";

export default function App() {
  return (
    <>
      <nav>
        <Link to="/">Home</Link> |{" "}
        <Link to="/about">About</Link> |{" "}
        <Link to="/books">Books</Link>
      </nav>

      <hr />

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />

        {/* Ruta pare */}
        <Route path="/books" element={<Books />}>
          {/* Nested routes */}
          <Route index element={<BookList />} />
          <Route path="new" element={<BookForm />} />
        </Route>
      </Routes>
    </>
  );
}
```

### Component pare amb nested routes (Books.jsx)

```jsx
import { Outlet, Link } from "react-router-dom";

export default function Books() {
  return (
    <div>
      <h2>Books</h2>

      <nav>
        <Link to="">Llista</Link> |{" "}
        <Link to="new">Nou llibre</Link>
      </nav>

      <hr />

      {/* Aquí es renderitzen les rutes filles */}
      <Outlet />
    </div>
  );
}
```

**L'Outlet és imprescindible**. Sense ell no es podrien renderitzar correctament

### Rutes filles - BookList.jsx

```jsx
export default function BookList() {
  return <p>Llista de llibres</p>;
}
```

### Rutes filles - BookForm.jsx

```jsx
export default function BookForm() {
  return <p>Formulari de llibre nou</p>;
}
```

### Rutes simples - Home.jsx

```jsx
export default function Home() {
  return <h2>Home</h2>;
}
```

### Rutes simples - About.jsx

```jsx
export default function About() {
  return <h2>About</h2>;
}
```

### Què passa en executar-ho?

* **`/`**: Mostra el component `Home`
* **`/about`**: Mostra el component `About`
* **`/books`**: Mostra els components `Books` + `BookList`
* **`/books/new`**: Mostra els components `Books` + `BookForm`
