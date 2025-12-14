# SPA

## SPA (Sinle Page Application)

Una SPA amb React és una aplicació web que carrega **una sola pàgina HTML** i canvia el contingut mitjançant components de React, sense recarregar la pàgina, comunicant-se amb un backend mitjançant APIs REST que retornen JSON.

### Què vol dir SPA?

Una SPA (Single Page Application) és una aplicació web que:

* Carrega una sola pàgina HTML
* No recarrega la pàgina quan l’usuari navega
* Canvia el contingut utilitzant JavaScript

Exemples d’SPAs:

* Gmail
* Google Drive
* Instagram
* Panells d’administració

Tot funciona dins la **mateixa pàgina**, però l’usuari té la **sensació que navega per diferents pantalles**.

## Diferència entre web tradicional i SPA

Les Webs tradicionals:

* Cada enllaç carrega un HTML nou
* El backend (PHP, Laravel, ...) genera les vistes
* La pàgina es recarrega contínuament

Una SPA amb React:

* Només hi ha un HTML
* React decideix què es mostra
* El backend només envia i rep dades (normalment JSON)

## Què fa React dins d’una SPA?

React s’encarrega de:

* Mostrar la interfície
* Canviar entre vistes
* Respondre a accions de l’usuari
* Actualitzar només el que cal

React **treballa amb components**.

Exemple:

```jsx
function Header() {
  return <h1>Gestió d’alumnes</h1>;
}
```

Cada part de la pantalla és un component independent.

## Com funciona una SPA en React (pas a pas)

### Càrrega inicial

El navegador carrega:

* index.html
* Els fitxers JavaScript de React

Exemple:

```html
<div id="root"></div>
```

React “entra” dins aquest `div` i "controla" tota la web.

### Components

La web es construeix amb components:

```jsx
function App() {
  return (
    <>
      <Menu />
      <Contingut />
    </>
  );
}
```

Si alguna dada canvia, React actualitza només aquella part.

### Navegació sense recàrrega (React Router)

Encara que vegem URLs diferents, no es carrega cap pàgina nova.

```jsx
<Route path="/login" element={<Login />} />
<Route path="/alumnes" element={<Alumnes />} />
```

Quan anem a `/alumnes`:

* React mostra el component `<Alumnes />`
* La pàgina **no es recarrega**

## D’on provenen les dades?

En una SPA:

* El backend (ex. Laravel) actua en forma de API
* Retorna dades en JSON
* **No retorna HTML**

Exemple:

```json
[
  { "id": 1, "nom": "Anna" },
  { "id": 2, "nom": "Marc" }
]
```

React "demana" aquestes dades i les mostra.

## Què és l’estat (`state`)?

L’estat són dades que **poden canviar** i afecten la pantalla.

Exemple:

```jsx
const [comptador, setComptador] = useState(0);
```

Si l’estat canvia:

* React actualitza la pantalla automàticament
* Això **evita haver de manipular el DOM manualment**.

## Exemple procés de Login i control d’accés

En una SPA:

* Es fa login
* El backend retorna un token
* El frontend guarda el token
* Només es pot accedir a certes pantalles si s’està loguejat

Exemple conceptual:

```jsx
if (!loguejat) {
  return <Login />;
}
```

## Alguns avantatges

* Aplicacions actuals
* Separació frontend / backend
* Ideal per CRUDs
* Reutilització de components

## Alguns inconvenients

* Depèn molt de JavaScript
* SEO més complicat
* Càrrega inicial una mica més lenta
* Cal organitzar bé el codi
