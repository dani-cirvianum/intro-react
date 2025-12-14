# Renderitzat condicional

## Renderitzat condicional

* Permet mostrar diferents elements o components en funció de l'estat actual de l'aplicació, les props que rep, o qualsevol altra condició lògica.
* El renderitzat condicional és el que fa que lesaplicacions de React siguin dinàmiques i responsives als canvis d'estat!

## Tècniques de Renderitzat Condicional

Hi ha diverses maneres d'implementar la lògica condicional dins del JSX de React.

### Amb l'Operador Ternari (? :)

* És la tècnica més bàsica i clara per mostrar una de dues opcions.
* És ideal quan tens una condició simple i vols triar entre dos blocs de codi (un si és veritat, l'altre si és fals).

```jsx
function MissatgeUsuari({ estaLogat }) {
  return (
    <div>
      {estaLogat ? (
        // Opció 1: Si la condició és TRUE
        <h1>Benvingut/da de nou!</h1>
      ) : (
        // Opció 2: Si la condició és FALSE
        <button>Inicia Sessió</button>
      )}
    </div>
  );
}
```

### Amb l'Operador Lògic && (Short-Circuiting)

* S'utilitza quan només vols renderitzar si la condició és veritable, i no es vol mostrar res si és falsa.
* Si l'expressió de l'esquerra és falsa, React ignora l'expressió de la dreta i no renderitza res.
* Si és certa, renderitza l'element de la dreta.

```jsx
function Notificacions({ count }) {
  return (
    <div>
      <h2>La teva Safata</h2>
      {count > 0 && (
        // Només es mostra aquest <p> si 'count' és major que 0
        <p>Tens {count} notificacions pendents.</p> 
      )}
    </div>
  );
}
```

### Amb Sentències if/else (Fora de JSX)

Quan la lògica condicional es torna massa complexa (amb moltes condicions o blocs de codi grans), és millor moure la lògica condicional fora del bloc return de JSX.

```jsx
function EstatDeCarrega({ dades, carregant }) {
  // 1. Definim la lògica condicional abans del return
  if (carregant) {
    return <div>Carregant dades...</div>;
  }
  
  if (!dades) {
    return <div>No s'han trobat dades.</div>;
  }

  // 2. Si es passen totes les condicions, retornem el renderitzat final
  return (
    <div>
      <h1>Dades rebudes amb èxit:</h1>
      <p>{dades.titol}</p>
    </div>
  );
}
```

### Elements Condicionals o Llistes

Es pot assignar un component sencer a una variable i després incloure aquesta variable dins del JSX.

```jsx
function PanellAdmin({ rol }) {
  let panell; // Declarem la variable on guardarem l'element

  if (rol === 'admin') {
    panell = <PanellCompleta />;
  } else if (rol === 'editor') {
    panell = <PanellEditor />;
  } else {
    panell = <div>Accés Denegat.</div>;
  }

  return (
    <div>
      <h3>Estat del Sistema</h3>
      {panell} {/* Renderitzem l'element que hem assignat a la variable */}
    </div>
  );
}
```

## Resum

* **Ternari (`? :`)**: Perfecte per escollir entre dues opcions simples dins d'una sola línia de JSX.
* **Short-Circuiting (`&&`)**: Ideal quan vols mostrar quelcom només si es compleix la condició.
* **`if/else` fora de JSX**: Més adequat per a lògiques complexes o quan vols retornar components sencers molt diferents.
