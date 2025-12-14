# React Memo

## `React.memo`

* `React.memo` és un **`hook` de React** que serveix per **evitar renderitzats innecessaris** de components funcionals.
* Cada cop que un component pare es renderitza **els components fills també es tornen a renderitzar**, encara que les seves dades no hagin canviat.
* `React.memo` evita aquest comportament quan **les propietats són les mateixes**.

> Es pot entendre com una manera de dir a React: _“Si aquest component rep les mateixes dades que abans, no el tornis a pintar”_.

## Què passa al “renderitzar”?

Renderitzar en React implica:

* **Executar** la funció del component
* **Calcular** el JSX
* **Actualitzar** (si cal) el DOM
* **Renderitzar** massa vegades **no és un error**, però pot afectar el **rendiment** en aplicacions grans.

## Què fa exactament `React.memo`?

Quan un component està embolcallat amb `React.memo`:

* Guarda el resultat del render anterior
* Compara les **propietats anteriors** amb les **propietats noves**
  * Si **no han canviat**, **no es torna a renderitzar**
  * Si **han canviat**, es renderitza amb normalitat

## Sintaxi bàsica

```jsx
function Missatge({ text }) {
  return <p>{text}</p>;
}

export default React.memo(Missatge);
```

* `Missatge` és el component original
* `React.memo(Missatge)` retorna un **nou component optimitzat**

## Exemple amb component pare i fill

```jsx
function Pare() {
  const [contador, setContador] = React.useState(0);

  return (
    <>
      <button onClick={() => setContador(contador + 1)}>
        Incrementar
      </button>
      <Fill text="Hola" />
    </>
  );
}

const Fill = React.memo(({ text }) => {
  console.log('Render del Fill');
  return <p>{text}</p>;
});
```

* Cada cop que es prem el botó, el component `Pare` es renderitza
* El component `Fill` **no es renderitza**, perquè la prop `text` no canvia
* Sense `React.memo`, `Fill` es renderitzaria cada vegada

## Components passats com a props

Un component també es pot passar com a propietat:

```jsx
function Contenidor({ Component }) {
  return <Component />;
}
```

```jsx
function Fill() {
  return <div>Contingut</div>;
}

<Contenidor Component={Fill} />;
```

Si es volen evitar renderitzats innecessaris:

```jsx
const FillMemo = React.memo(Fill);

<Contenidor Component={FillMemo} />;
```

## `React.memo` i `children`

```jsx
const Caixa = React.memo(({ children }) => {
  return <div>{children}</div>;
});
```

Si `children` és un JSX que es crea de nou a cada render, `React.memo` **no evitarà** el render.

## Problema habitual: objectes i arrays

```jsx
<Fill dades={{ nom: 'Dani' }} />
```

Tot i que el contingut és el mateix, l’objecte és **nou a cada render**, i per tant:

* React considera que la prop ha canviat
* El component es torna a renderitzar

## Quan utilitzar `React.memo`?

* **SÍ** és recomanable utilitzar `React.memo`:
  * Components amb render costós
  * Components que reben sovint les mateixes propietats
  * Llistes, taules, dashboards, ...
* **NO** és recomanable utilitzar `React.memo`:
  * Components molt simples
  * Quan les props canvien constantment
  * Afegir-lo sense entendre el problema
