# Helmet

## React Hemlet

* És una llibreria que permet gestionar fàcilment el contingut de l'etiqueta `<head>` dins d’una aplicació React.
* És especialment útil en SPAs (Single Page Applications), on cada vista pot necessitar un títol, metadades o enllaços diferents per a SEO i xarxes socials.

### Què fa React Helmet?

* Gestiona el `<head>`: permet afegir o modificar `<title>`, `<meta>`, `<link>`, `<script>`, etc.
* SEO i xarxes socials: permet definir metadades dinàmiques.
* Evita duplicació: cada component pot tenir el seu Helmet i el més profund sobreescriu el pare.
* Funciona amb CSR i SSR: tant en renderitzat client-side com server-side.

### Instal·lació

```bash
npm install react-helmet-async --legacy-peer-deps
```

### Incloure el HelmetProvider

Això normalment es realitza al fitxer `main.jsx`.

```jsx
// main.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { HelmetProvider } from 'react-helmet-async'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <HelmetProvider>
      <App />
    </HelmetProvider>
  </React.StrictMode>
)
```

### Utilitzar el Helment a cada component/ruta

```jsx
<Helmet>
      <title>Inici | La meva web</title>
</Helmet>
```

Cada component defineix el seu propi `Helmet` i React s'encarrega d'aplicar el canvi.

### Exemple bàsic

```jsx
import React from "react";
import { Helmet } from "react-helmet";

function AboutPage() {
  return (
    <div>
      <Helmet>
        <title>Sobre nosaltres</title>
        <meta name="description" content="Informació sobre la nostra empresa" />
        <link rel="canonical" href="https://example.com/about" />
      </Helmet>

      <h1>About Us</h1>
      <p>Benvingut a la pàgina sobre nosaltres.</p>
    </div>
  );
}

export default AboutPage;
```

* Quan es visita `/about`, el `<head>` del document tindrà:

```html
<title>Sobre nosaltres</title>
<meta name="description" content="Informació sobre la nostra empresa">
<link rel="canonical" href="https://example.com/about">
```

### Exemple amb rutes niuades

* Cada component pot definir el seu Helmet:

```jsx
function Post({ post }) {
  return (
    <div>
      <Helmet>
        <title>{post.title}</title>
        <meta name="description" content={post.body.slice(0, 100)} />
      </Helmet>
      <h2>{post.title}</h2>
      <p>{post.body}</p>
    </div>
  );
}
```

D'aquesta manera cada post té títol i metadades dinàmiques.

### Patró per agrupar diferents components

```jsx
function Page({ title, children }) {
  return (
    <>
      <Helmet>
        <title>{title}</title>
      </Helmet>
      {children}
    </>
  )
}
```

```jsx
function Page({ title, children }) {
  return (
    <>
      <Helmet>
        <title>{title}</title>
      </Helmet>
      {children}
    </>
  )
}
```

