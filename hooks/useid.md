# UseId

## USEID

### Què és?

* Genera un **ID únic i estable** per a cada renderitzat del component.
* Relaciona elements HTML que necessiten un identificador comú (per exemple, `<label>` i `<input>`).
* Evita conflictes d’IDs quan tens múltiples instàncies del mateix component.

### Per què és útil?

* **Accessibilitat (WCAG/ARIA)**: s'associa correctament label i input.
* **Components reutilitzables**: si hi ha varies instàncies del mateix component, cada una tindrà els seus IDs únics.
* **Evita hardcodejar IDs**: no cal inventar noms com input-1, input-2…

### A tenir en compte

* Els IDs generats per useId són estables entre renders, però no són predictibles (React els crea amb prefixos interns).
* Si es necessita un ID per CSS o JavaScript extern, millor definir-lo manualment.
* `useId()` està pensat sobretot per accessibilitat i relacions semàntiques dins del DOM.

### Exemple bàsic

```jsx
import { useId } from "react";

function NameField() {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Nom:</label>
      <input id={id} type="text" name="name" />
    </div>
  );
}
```

* `useId()` genera un ID únic (ex. `:r0:`).
* El label i l’input comparteixen aquest ID, assegurant que el lector de pantalla entengui la relació.

### Exemple amb múltiples camps

```jsx
function Form() {
  const nameId = useId();
  const emailId = useId();

  return (
    <form>
      <label htmlFor={nameId}>Nom:</label>
      <input id={nameId} type="text" />

      <label htmlFor={emailId}>Email:</label>
      <input id={emailId} type="email" />
    </form>
  );
}
```

* Cada `useId()` retorna un ID diferent, evitant col·lisions.

### Exemple amb `aria-describedby`

```jsx
import { useId, useState } from "react";

function EmailField() {
  const inputId = useId();       // ID per l’input
  const helpId = useId();        // ID per al text d’ajuda
  const [error, setError] = useState("");

  const validate = (e) => {
    const value = e.target.value;
    if (!value.includes("@")) {
      setError("El correu ha de contenir '@'");
    } else {
      setError("");
    }
  };

  return (
    <div>
      <label htmlFor={inputId}>Correu electrònic:</label>
      <input
        id={inputId}
        type="email"
        onBlur={validate}
        aria-describedby={helpId} // connecta amb el text d’ajuda/error
      />
      <p id={helpId} style={{ color: error ? "red" : "gray" }}>
        {error || "Introdueix un correu vàlid"}
      </p>
    </div>
  );
}
```

* `useId()` assegura que cada instància del component té IDs únics.
* `aria-describedby={helpId}` fa que el lector de pantalla llegeixi el text associat al camp.
* Si hi ha error, el missatge es llegeix com a part de l’input; si no, es mostra el text d’ajuda.
