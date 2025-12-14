# Events

## Què és la Gestió d'Esdeveniments a React?

* És el mecanisme pel qual React respon a les accions de l'usuari (clics de ratolí, escriptura, canvis de formularis, etc.).
* És molt similar a l'HTML estàndard, però amb algunes diferències clau:
  * Noms d'Esdeveniments: Els noms dels esdeveniments es passen en format camelCase (p. ex., onClick, onChange, onSubmit) en lloc de minúscules (onclick).
  * Passar Funcions: Es passa una funció com a handler d'esdeveniment, en lloc d'una cadena de text.

## Sintaxi Bàsica

A HTML, es faria:

HTML

```html
<button onclick="showAlert()">Fes Clic</button>
```

A React:

```javascript
function NomComponent() {
  // 1. Definim la funció (Event Handler)
  const handleClick = (event) => {
    // 'event' és l'objecte d'esdeveniment sintètic de React
    alert('Has fet clic!'); 
    console.log(event);
  };

  return (
    // 2. Passem la funció com a prop
    <button onClick={handleClick}>
      Fes Clic
    </button>
  );
}
```

## Esdeveniment `Sintètic`

* Quan es dispara un esdeveniment (p. ex., un clic), React crea un `SyntheticEvent` (Esdeveniment Sintètic).
* Aquest objecte és un `wrapper` (embolcall) al voltant de l'esdeveniment natiu del navegador. La seva funció principal és:
  * **Normalitzar**: Garantir que les propietats de l'esdeveniment (com ara event.preventDefault(), event.target, etc.) es comportin de manera consistent en tots els navegadors (Chrome, Firefox, Safari, etc.).
  * **Reutilitzar**: React recicla aquests objectes per millorar el rendiment.

❗ Nota: Si necessites accedir a l'esdeveniment natiu del navegador, pots utilitzar event.nativeEvent.

## Passar Arguments als Handlers

* Es poden passar arguments addicionals a la funció handler (a més de l'esdeveniment mateix).
* S'utilitza una funció fletxa anònima o el mètode `bind` (tot i que la funció fletxa és la més utilitzada):

#### Amb Funció Fletxa (Mètode Recomanat)

* Permet cridar la teva funció amb els arguments que es desitgin.

```jsx
function Producte({ nom, id }) {
  const handleAddToCart = (productId) => {
    console.log(`Afegint el producte amb ID: ${productId} al carretó.`);
  };

  return (
    // Passem l'ID del producte directament a la funció
    <button onClick={() => handleAddToCart(id)}>
      Afegir {nom}
    </button>
  );
}
```

## Evitar el comportament per defecte

En esdeveniments de formularis (com onSubmit), es crida `event.preventDefault()` per evitar que el navegador recarregui la pàgina, que és el seu comportament per defecte quan s'envia un formulari HTML.

```jsx
const handleSubmit = (e) => {
  e.preventDefault(); // <-- Atura el recàrrega de la pàgina
  console.log('Formulari enviat, però sense recàrrega');
};

return (
  <form onSubmit={handleSubmit}>
    {/* ... inputs ... */}
    <button type="submit">Enviar</button>
  </form>
);
```

## Gestió d'Esdeveniments en Formularis (onChange)

* `onChange`, s'utilitza per capturar el que l'usuari escriu als camps de formulari i actualitzar l'estat del component.

```jsx
function CampInput() {
  const [valor, setValor] = useState('');

  const handleChange = (e) => {
    // e.target.value conté el text actual del camp
    setValor(e.target.value); 
  };

  return (
    <>
      <input type="text" value={valor} onChange={handleChange} />
      <p>Estàs escrivint: {valor}</p>
    </>
  );
}
```

## **Exemple**: Formulari de Contacte Senzill

* Formulari per capturar el nom i el correu electrònic.

### Preparació de l'Estat

* Utilitzar un sol objecte d'estat per gestionar tots els valors del formulari.

```jsx
import React, { useState } from 'react';

function FormulariInscripcio() {
  // Inicialitzem l'estat amb un objecte que conté els camps del formulari
  const [formData, setFormData] = useState({
    nom: '',
    email: ''
  });
  // ... (continua a la següent secció)
```

### Gestió dels Canvis (`onChange`)

* Crear una única funció handleChange que gestiona l'actualització de qualsevol camp del formulari utilitzant la propietat name de l'element d'entrada (input).

```jsx
// ... (continuació de la secció anterior)
  
  // Funció única per gestionar els canvis en qualsevol input
  const handleChange = (e) => {
    // 1. Obtenim el nom del camp (p. ex., 'nom' o 'email') i el seu nou valor
    const { name, value } = e.target; 

    // 2. Actualitzem l'estat. Utilitzem l'operador spread (...)
    // per mantenir els altres camps i només sobreescriure el camp actualitzat.
    setFormData(prevData => ({
      ...prevData,
      [name]: value // [name] utilitza la clau dinàmica per al camp actual
    }));
  };
// ... (continua a la següent secció)
```

### Gestió de l'Enviament (`onSubmit`)

* La funció handleSubmit s'executarà quan es faci clic al botó d'enviament (o apretar Enter).
* El pas més important és cridar e.preventDefault().

```jsx
// ... (continuació de la secció anterior)

  const handleSubmit = (e) => {
    e.preventDefault(); // Molt important: atura la recàrrega de la pàgina
    
    // Aquí es faria la lògica d'enviament a una API
    console.log('Dades del formulari enviades:', formData);
    
    // Opcional: Netejar el formulari
    setFormData({ nom: '', email: '' });
  };
// ... (continua a la següent secció)
```

### La Renderització del Component

* Enllaçar les funcions i l'estat als elements del formulari:

```jsx
// ... (continuació de la secció anterior)

  return (
    <form onSubmit={handleSubmit}>
      
      {/* Camp de Nom */}
      <div>
        <label htmlFor="nom">Nom:</label>
        <input
          type="text"
          id="nom"
          name="nom" // Important: coincideix amb la clau de l'objecte d'estat
          value={formData.nom} // L'estat controla el valor
          onChange={handleChange} // S'activa a cada pulsació de tecla
        />
      </div>

      {/* Camp d'Email */}
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email" //  Important: coincideix amb la clau de l'objecte d'estat
          value={formData.email}
          onChange={handleChange}
        />
      </div>

      <button type="submit">Inscriu-te</button>
      
      {/* Mostra l'estat actual per a depuració */}
      <p>Estat actual: Nom={formData.nom}, Email={formData.email}</p>
    </form>
  );
}
```
