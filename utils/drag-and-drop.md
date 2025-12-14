# Drag\&Drop

## Drag & Drop en React amb Vite

### Crear el projecte amb Vite

```bash
npm create vite@latest react-dnd-demo
cd react-dnd-demo
npm install
npm install react-dnd react-dnd-html5-backend
```

* `react-dnd`: llibreria principal per drag & drop
* `react-dnd-html5-backend`: backend basat en HTML5 per gestionar els esdeveniments de drag & drop.

### Configuració bàsica

Modificar main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import { DndProvider } from "react-dnd";
import { HTML5Backend } from "react-dnd-html5-backend";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <DndProvider backend={HTML5Backend}>
      <App />
    </DndProvider>
  </React.StrictMode>
);
```

### Crear un element draggable

```jsx
// DraggableBox.jsx
import { useDrag } from "react-dnd";

export default function DraggableBox({ id, text }) {
  const [{ isDragging }, drag] = useDrag(() => ({
    type: "BOX",
    item: { id },
    collect: (monitor) => ({
      isDragging: monitor.isDragging(),
    }),
  }));

  return (
    <div
      ref={drag}
      style={{
        padding: "10px",
        margin: "5px",
        backgroundColor: isDragging ? "lightgreen" : "lightblue",
        cursor: "move",
      }}
    >
      {text}
    </div>
  );
}
```

### Crear una zona de drop

```jsx
// DropZone.jsx
import { useDrop } from "react-dnd";

export default function DropZone({ onDrop }) {
  const [{ isOver }, drop] = useDrop(() => ({
    accept: "BOX",
    drop: (item) => onDrop(item.id),
    collect: (monitor) => ({
      isOver: monitor.isOver(),
    }),
  }));

  return (
    <div
      ref={drop}
      style={{
        minHeight: "150px",
        border: "2px dashed gray",
        backgroundColor: isOver ? "#f0f0f0" : "white",
        padding: "10px",
      }}
    >
      <p>Arrossega aquí</p>
    </div>
  );
}
```

### Integrar-ho a l’App

```jsx
// App.jsx
import { useState } from "react";
import DraggableBox from "./DraggableBox";
import DropZone from "./DropZone";

export default function App() {
  const [droppedItems, setDroppedItems] = useState([]);

  const handleDrop = (id) => {
    setDroppedItems((prev) => [...prev, id]);
  };

  return (
    <div style={{ display: "flex", gap: "20px" }}>
      <div>
        <h3>Elements disponibles</h3>
        <DraggableBox id="1" text="Element 1" />
        <DraggableBox id="2" text="Element 2" />
        <DraggableBox id="3" text="Element 3" />
      </div>

      <div>
        <h3>Zona de drop</h3>
        <DropZone onDrop={handleDrop} />
        <ul>
          {droppedItems.map((item, index) => (
            <li key={index}>Has deixat: {item}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}
```

### Resultat esperat

* Es Pot arrossegar els elements cap a la zona de drop.
* En deixar un element, apareix a la llista de `"Has deixat: ..."`.

## Exemple amb imatges

### Element draggable amb imatge

```jsx
// DraggableImage.jsx
import { useDrag } from "react-dnd";

export default function DraggableImage({ id, src, alt }) {
  const [{ isDragging }, drag] = useDrag(() => ({
    type: "IMAGE",
    item: { id },
    collect: (monitor) => ({
      isDragging: monitor.isDragging(),
    }),
  }));

  return (
    <img
      ref={drag}
      src={src}
      alt={alt}
      style={{
        width: "100px",
        margin: "10px",
        opacity: isDragging ? 0.5 : 1,
        cursor: "grab",
        border: "2px solid #ccc",
        borderRadius: "8px",
      }}
    />
  );
}
```

### Zona de drop per imatges

```jsx
// ImageDropZone.jsx
import { useDrop } from "react-dnd";

export default function ImageDropZone({ onDrop }) {
  const [{ isOver }, drop] = useDrop(() => ({
    accept: "IMAGE",
    drop: (item) => onDrop(item.id),
    collect: (monitor) => ({
      isOver: monitor.isOver(),
    }),
  }));

  return (
    <div
      ref={drop}
      style={{
        minHeight: "200px",
        width: "300px",
        border: "3px dashed gray",
        backgroundColor: isOver ? "#f9f9f9" : "white",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
      }}
    >
      <p>Deixa aquí la imatge</p>
    </div>
  );
}
```

### Integració a l’App

```jsx
// App.jsx
import { useState } from "react";
import DraggableImage from "./DraggableImage";
import ImageDropZone from "./ImageDropZone";

export default function App() {
  const [droppedImages, setDroppedImages] = useState([]);

  const handleDrop = (id) => {
    setDroppedImages((prev) => [...prev, id]);
  };

  return (
    <div style={{ display: "flex", gap: "20px" }}>
      <div>
        <h3>Imatges disponibles</h3>
        <DraggableImage id="cat" src="/cat.jpg" alt="Gat" />
        <DraggableImage id="dog" src="/dog.jpg" alt="Gos" />
        <DraggableImage id="bird" src="/bird.jpg" alt="Ocell" />
      </div>

      <div>
        <h3>Zona de drop</h3>
        <ImageDropZone onDrop={handleDrop} />
        <div style={{ marginTop: "10px" }}>
          {droppedImages.map((img, index) => (
            <p key={index}>Has deixat: {img}</p>
          ))}
        </div>
      </div>
    </div>
  );
}
```

### Notes

* Les imatges (cat.jpg, dog.jpg, etc.) s'han de posar dins la carpeta `public/` de Vite.

## Carregar imatges del client i fer drag & drop

### Input per pujar imatges

```jsx
// ImageUploader.jsx
import { useState } from "react";

export default function ImageUploader({ onAddImage }) {
  const handleChange = (e) => {
    const files = e.target.files;
    if (files.length > 0) {
      const newImages = Array.from(files).map((file) => ({
        id: file.name,
        src: URL.createObjectURL(file),
        alt: file.name,
      }));
      onAddImage(newImages);
    }
  };

  return (
    <div>
      <input type="file" accept="image/*" multiple onChange={handleChange} />
    </div>
  );
}
```

* `URL.createObjectURL(file)` crea una URL temporal per mostrar la imatge carregada.
* `multiple` permet pujar més d’una imatge.

### Adaptar el component draggable

```jsx
// DraggableImage.jsx
import { useDrag } from "react-dnd";

export default function DraggableImage({ id, src, alt }) {
  const [{ isDragging }, drag] = useDrag(() => ({
    type: "IMAGE",
    item: { id, src },
    collect: (monitor) => ({
      isDragging: monitor.isDragging(),
    }),
  }));

  return (
    <img
      ref={drag}
      src={src}
      alt={alt}
      style={{
        width: "100px",
        margin: "10px",
        opacity: isDragging ? 0.5 : 1,
        cursor: "grab",
        border: "2px solid #ccc",
        borderRadius: "8px",
      }}
    />
  );
}
```

### Integració a l’App

```jsx
// App.jsx
import { useState } from "react";
import ImageUploader from "./ImageUploader";
import DraggableImage from "./DraggableImage";
import ImageDropZone from "./ImageDropZone";

export default function App() {
  const [images, setImages] = useState([]);
  const [droppedImages, setDroppedImages] = useState([]);

  const handleAddImage = (newImages) => {
    setImages((prev) => [...prev, ...newImages]);
  };

  const handleDrop = (id) => {
    const img = images.find((i) => i.id === id);
    if (img) setDroppedImages((prev) => [...prev, img]);
  };

  return (
    <div style={{ display: "flex", gap: "20px" }}>
      <div>
        <h3>Puja imatges</h3>
        <ImageUploader onAddImage={handleAddImage} />
        <div style={{ display: "flex", flexWrap: "wrap" }}>
          {images.map((img) => (
            <DraggableImage key={img.id} {...img} />
          ))}
        </div>
      </div>

      <div>
        <h3>Zona de drop</h3>
        <ImageDropZone onDrop={handleDrop} />
        <div style={{ marginTop: "10px" }}>
          {droppedImages.map((img, index) => (
            <img
              key={index}
              src={img.src}
              alt={img.alt}
              style={{ width: "80px", margin: "5px" }}
            />
          ))}
        </div>
      </div>
    </div>
  );
}
```

### Notes importants

* Les imatges carregades amb `URL.createObjectURL` només existeixen mentre la pàgina estigui oberta. Si es refresca, desapareixen.
* Per persistir-les, caldria pujar-les a un servidor o guardar-les en localStorage/base de dades.
