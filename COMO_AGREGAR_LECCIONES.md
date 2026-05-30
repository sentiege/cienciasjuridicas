# 📚 Cómo usar esta plantilla

## Estructura del archivo

Todo vive en un único archivo `template.html`. Los únicos lugares que necesitás editar están marcados con `✏️` o comentarios en MAYÚSCULAS.

---

## PASO 1 — Configurar datos básicos (en el `<head>` y hero)

```html
<title>🎓 Nombre de la Materia — Lecciones</title>
...
<h1>NOMBRE DE LA MATERIA</h1>
<p class="universidad">Universidad Nacional de Asunción</p>
<p class="profesor">Prof. Nombre del Profesor</p>
<div class="badge-row">
  <span class="badge">10 Lecciones</span>   ← cambiá el número
</div>
```

---

## PASO 2 — Configurar Firebase (para ranking y login)

1. Entrá a https://console.firebase.google.com
2. Creá un proyecto nuevo
3. Activá **Firestore Database** en modo producción
4. En Configuración del proyecto → Tus apps → Web, copiá el config
5. Reemplazá en el JS:

```js
const firebaseConfig = {
  apiKey:            "TU_API_KEY",        ← reemplazá
  authDomain:        "TU_PROYECTO...",    ← reemplazá
  ...
};
```

6. En Firestore → Reglas, pegá estas reglas:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{ci} {
      allow read: if true;
      allow create: if request.resource.data.ci == ci;
      allow update: if request.resource.data.ci == ci && resource.data.ci == ci;
      allow delete: if false;
    }
  }
}
```

---

## PASO 3 — Definir las lecciones (en el JS)

### 3a. TOTAL — cantidad de slides por lección
```js
const TOTAL = {
  "1": 5,   // lección 1 tiene 5 slides (el último siempre es ejercicios)
  "2": 6,   // lección 2 tiene 6 slides
  ...
};
```

### 3b. BLOCKS — agrupar lecciones por tema
```js
const BLOCKS = {
  "📖 Primer Bloque": [1,2,3,4,5],
  "📚 Segundo Bloque": [6,7,8,9,10],
};
const BLOCK_NAMES = {
  1:'Primer Bloque', 2:'Primer Bloque', ...
};
const EXAM_LESSONS = [1,2,3,4,5,6,7,8,9,10];
```

---

## PASO 4 — Contenido de cada lección (en el HTML)

Buscá `id="lesson-1"` y editá cada slide:

```html
<h2 class="slide-title">Tu Título Aquí</h2>
<div class="slide-content">
  Tu contenido acá. Podés usar:<br><br>
  <strong>Negrita</strong> para términos clave<br>
  <em>Itálica</em> para conceptos o citas<br>
  • Listas con punto · separadas por <br>
</div>
```

Si querés **más slides**, copiá un bloque `<div class="slide " data-index="X">` 
y aumentá el data-index. Acordate de actualizar `TOTAL` y agregar un `<button class="dot">` extra.

Si querés **menos slides**, borrá un bloque slide y actualizá `TOTAL` y los dots.

---

## PASO 5 — Quiz por lección (en el JS)

```js
const EXAM_DATA = [
  // Lección 1
  {l:1, q:"¿Qué es el concepto X?",
   opts:["Definición correcta","Definición incorrecta A","Incorrecta B","Incorrecta C"],
   a:0},   // ← a: es el índice de la respuesta correcta (0=primera opción)

  // Lección 2
  {l:2, q:"Otra pregunta...", opts:["A","B","C","D"], a:2},

  // Agregá mínimo 4-5 preguntas por lección para el simulacro
];
```

---

## PASO 6 — Ejercicios fill-in-the-blank (en el JS)

```js
const FILL_DATA_JS = {
  "1": [  // lección 1
    {s:"La ___ es el conjunto de normas que regulan la conducta.", 
     a:"ley",          // respuesta correcta (flexible, ignora tildes/mayúsculas)
     hint:"Norma obligatoria"},
  ],
  "2": [  // lección 2
    {s:"El ___ es el encargado de aplicar las normas.", a:"juez", hint:"Pista"},
  ],
};
```

---

## PASO 7 — Nombre de las lecciones en el home

Buscá los `<div class="lesson-card">` y cambiá el texto:
```html
<span class="card-title">Tu Título de Lección Acá</span>
```

Y en el header de cada lección:
```html
<h1 class="lesson-main-title" id="lesson-title-1">Título de la Lección</h1>
<p class="lesson-subtitle" id="lesson-subtitle-1">Subtítulo opcional</p>
```

---

## PASO 8 — Subir a GitHub Pages

1. Creá un repo en GitHub
2. Subí `template.html` renombrándolo a `index.html` (o como prefieras)
3. Settings → Pages → Branch: main → Save
4. En ~2 minutos tu sitio está en `https://TU_USUARIO.github.io/TU_REPO`

---

## Agregar más de 10 lecciones

1. Copiá un bloque `<section class="lesson-page" id="lesson-X">` del HTML
2. Cambiá todos los `X` por el número nuevo
3. Agregá la entrada en `TOTAL`, `BLOCKS`, `BLOCK_NAMES`, `EXAM_LESSONS`, `ALL_LESSONS`
4. Agregá una `lesson-card` en el home
5. Agregá preguntas en `EXAM_DATA` y ejercicios en `FILL_DATA_JS`

---

## Cheatsheet de formato HTML para slides

| Querés | Código |
|--------|--------|
| **Negrita** | `<strong>texto</strong>` |
| *Itálica* | `<em>texto</em>` |
| Salto de línea | `<br>` |
| Párrafo separado | `<br><br>` |
| Lista con puntos | `• Item 1<br>• Item 2` |
| Resaltado naranja (examen) | Agregá clase `exam-card` a la lesson-card |

---

*Plantilla generada desde el proyecto cienciasjuridicas · sentiege/cienciasjuridicas*
