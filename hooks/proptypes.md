# PropTypes

## React PropTypes

* `PropTypes` és una llibreria de React que permet definir i validar les propietats que rep un component.
* Serveix per ajudar els desenvolupadors a detectar errors de tipus o propietats obligatòries **durant el desenvolupament**.
* **No és obligatori**, però és **molt útil** per mantenir components clars i segurs.

## Què fa?

* Permet declarar quin tipus de dada s’espera a cada prop d’un component.
* Permet marcar quines propietats són obligatòries.
* Mostra avisos a la consola si les propietats no compleixen les regles (**només en desenvolupament**).

## Instal·lació

Si no està instal·lat, es pot afegir amb `npm`:

```bash
npm install prop-types
```

## Sintaxi bàsica

```jsx
import PropTypes from 'prop-types';

function Salutacio({ nom, edat }) {
  return <p>Hola {nom}, tens {edat} anys</p>;
}

// Declaració de tipus de props
Salutacio.propTypes = {
  nom: PropTypes.string.isRequired, // string i obligatori
  edat: PropTypes.number              // number opcional
};

// Valors per defecte si no es passen
Salutacio.defaultProps = {
  edat: 18
};

```

## Tipus més comuns de `PropTypes`

| Tipus                    | Exemple                          |
| ------------------------ | -------------------------------- |
| `PropTypes.string`       | Text                             |
| `PropTypes.number`       | Números                          |
| `PropTypes.bool`         | Boolean                          |
| `PropTypes.array`        | Arrays                           |
| `PropTypes.object`       | Objetes                          |
| `PropTypes.func`         | Funcions                         |
| `PropTypes.node`         | Qualsevol element de Reac (JSX)  |
| `PropTypes.element`      | Un element React concret         |
| `PropTypes.oneOf([...])` | Una de les opcions especificades |
