# Llistes i claus

## Llistes a React

Renderitzar una col·lecció de dades (com ara un array d'objectes o strings) es fa mapejant l'array a un array d'elements de React.

### El Mètode `map`()

La manera més comuna de generar llistes de components o elements HTML és utilitzar el mètode `map()` de JavaScript dins del JSX:

```jsx
const productes = [
  { id: 1, nom: 'Poma', preu: 0.5 },
  { id: 2, nom: 'Pera', preu: 0.7 },
  { id: 3, nom: 'Plàtan', preu: 0.6 }
];

function LlistaDeProductes() {
  return (
    <ul>
      {/* Mapegem l'array de productes a un array d'elements <li> */}
      {productes.map((producte) => (
        // Aquí és on afegim la 'key'
        <li key={producte.id}> 
          {producte.nom} - {producte.preu}€
        </li>
      ))}
    </ul>
  );
}
```

## Claus (`Keys`)

* La propietat `key` és un atribut especial que s'ha d'incloure sempre que es crea una llista d'elements.
* React utilitza aquestes claus per **optimitzar** la gestió i l'**actualització** de la llista.

### Per què són importants les Claus?

* Quan una llista canvia (s'afegeix, s'elimina o es reordena un element), React necessita saber exactament quin element de l'arbre del DOM ha canviat, sense haver de redibuixar tota la llista.
* Sense una clau estable, React reconstrueix tota l'estructura o bé actualitza el component incorrecte, la qual cosa pot provocar:
  * **Errors de rendiment**: Redibuixar components innecessàriament.
  * **Errors d'Estat**: L'estat d'un camp d'un formulari pot ser transferit incorrectament a un altre element si la llista es reordena.

### Requisits de les Claus

* **Úniques**: La clau ha de ser única entre els seus germans (dins de la mateixa llista). No cal que sigui única globalment en tota l'aplicació.
* **Estables**: La clau d'un element no ha de canviar entre renderitzacions. És a dir, si un element s'afegeix a l'array, sempre ha de tenir la mateixa clau.

### Ús Recomanat: ID Estables

{% hint style="info" %}
Utilitzar l'ID únic i estable que prové de les dades (normalment d'una base de dades o una API).
{% endhint %}

```jsx
// Correcte: id és estable i únic
<li key={producte.id}>...</li> 
```

### Ús a Evitar: L'Índex de l'Array

* El mètode `map()` proporciona un segon argument, l'índex de l'element.
* Només s'ha d'utilitzar l'índex com a clau si la llista **MAI** canviarà d'ordre, ni s'afegiran o eliminaran elements.
* Si la llista es modifica, l'índex canviarà i React perdrà la pista dels elements, fent que el propòsit de la clau sigui inútil i causant els problemes esmentats:

```jsx
// A EVITAR si la llista pot canviar d'ordre o mida
productes.map((producte, index) => (
  <li key={index}>...</li> 
))
```

## Components i Claus

* En encapsular un element de llista dins d'un component propi, la clau s'ha d'aplicar a l'element de React més extern que es retorna dins de la funció `map()`.

```jsx
// 1. Component LlistItema.jsx
function LlistItema({ nom, preu }) {
  // Aquest component NO necessita saber res de la 'key'
  return (
    <div className="item-card">
      <h3>{nom}</h3>
      <p>{preu}€</p>
    </div>
  );
}
```

```jsx
// 2. Ús al Component Pare (LlistaDeProductes.jsx)
function LlistaDeProductes() {
  // ... array productes ...
  return (
    <div>
      {productes.map((p) => (
        // La 'key' s'aplica al component LlistItema que estem creant
        <LlistItema 
          key={p.id} // Correcte
          nom={p.nom}
          preu={p.preu} 
        />
      ))}
    </div>
  );
}
```
