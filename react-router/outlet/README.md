# Outlet

## Què és `<Outlet />`

* És un placeholder que indica on s’han de renderitzar les rutes fills dins d’una ruta pare.
* Permet definir un layout comú (header, sidebar, footer, etc.) i que les rutes anidades apareguin en un punt concret del component.
* Sense `<Outlet />`, les rutes fills no es mostraran.

### Exemple bàsic

* Definició de rutes

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./Layout";
import Home from "./Home";
import About from "./About";
import Contact from "./Contact";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Ruta pare amb layout */}
        <Route path="/" element={<Layout />}>
          {/* Rutes fills */}
          <Route index element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

* Layout amb `<Outlet />`

```jsx
import { Outlet, Link } from "react-router-dom";

function Layout() {
  return (
    <div>
      <header>
        <nav>
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
          <Link to="/contact">Contact</Link>
        </nav>
      </header>

      {/* Aquí es renderitzen les rutes fills */}
      <main>
        <Outlet />
      </main>

      <footer>© 2025 - Exemple SPA</footer>
    </div>
  );
}

export default Layout;
```

* Quan es va a `/about`, React Router renderitza el component About dins del `<Outlet />` del Layout.
* És la base per fer dashboards, panells d’administració o qualsevol aplicació amb seccions.

### Exemple: un Dashboard amb subrutes:

```jsx
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route index element={<Overview />} />
  <Route path="settings" element={<Settings />} />
  <Route path="profile" element={<Profile />} />
</Route>
I dins DashboardLayout.jsx:

jsx
function DashboardLayout() {
  return (
    <div>
      <aside>Menú lateral</aside>
      <section>
        <Outlet /> {/* Aquí apareixen Overview, Settings o Profile */}
      </section>
    </div>
  );
}
```

* `<Outlet />` és el punt d’injecció de les rutes fills dins d’un layout pare. Sense ell, les rutes anidades no es renderitzen.
