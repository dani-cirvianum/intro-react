# JSX

## JSX

* És una extensió de la sintaxi de JavaScript que permet escriure codi amb l'aparença d'HTML/XML dins dels arxius de JavaScript.
* El JSX serveix com una forma declarativa de descriure com hauria de ser la interfície d'usuari (UI).
* Permet utilitzar tota la potència de JavaScript (variables, funcions, bucles) per construir elements d'interfície complexos de manera familiar (utilitzant etiquetes com `<div>` i `<h1>`).
* El JSX es compila finalment a crides de funció de JavaScript.
* Un navegador web no pot llegir directament JSX.
* Quan s'escriu codi JSX, eines de compilació el tradueixen a crides a la funció `React.createElement()` que els navegadors sí que poden processar.

## Regles i característiques clau del JSX

### Incrustar Expressions JavaScript

Es pots incrustar qualsevol expressió de JavaScript dins del JSX utilitzant claus `{}`:

```jsx
const nom = "Dani";

const element = (
  <h1>Hola, {nom}!</h1> // S'injecta el valor de la variable 'nom'
);
```

Dins de les claus, es poden fer:

* càlculs
* cridar funcions
* utilitzar variables

### Classes CSS a JSX

* No es pot utilitzar l'atribut `class` com a HTML.
* S'ha d'utilitzar `className` perquè `class` és una paraula reservada a JavaScript.

```jsx
<div className="contenidor-principal">
  {/* ... */}
</div>
```

### Tancament d'etiquetes

* Totes les etiquetes han de ser tancades, incloses les etiquetes autotancables (com `<br>` o `<img>`).
* Correcte: `<br />` o `<img src="..." alt="..." />`
* Incorrecte (HTML): `<br>` o `<img>`

### Només un element arrel

* Cada bloc de JSX (per exemple, dins d'un `return` de component) només pot retornar un únic element arrel.
* Si cal agrupar múltiples elements sense afegir un `div` extra al DOM, es pot utilitzar un Fragment (`<>...</>`)

```jsx
// Correcte: utilitzant un Fragment
return (
  <>
    <h1>Títol</h1>
    <p>Paràgraf</p>
  </>
);
```

### Atributs en `camelCase`

* La majoria dels atributs del DOM (excepte `aria-*` i `data-*`) es passen en format `camelCase`.
