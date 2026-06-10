# Resumen de clase — Sass (Lección 3)
**Fecha:** lunes 8 de junio de 2026
**Asignatura:** Desarrollo de la Interfaz de Usuario Web

Este resumen acompaña al código fuente que les comparto. Úsenlo para repasar lo
visto y como apoyo para la actividad.

---

## ¿Qué es Sass?

Sass es un **preprocesador de CSS**: un lenguaje con superpoderes (variables,
anidamiento, cálculos…) que luego se **traduce a CSS normal**, que es lo único que
entiende el navegador.

- Escribimos en archivos `.scss`.
- Lo **compilamos** y se genera un `.css`.
- El HTML enlaza el `.css`, nunca el `.scss`.

> Regla clave: **el `.css` es generado. Nunca lo edites a mano; edita el `.scss` y recompila.**

---

## DÍA 1 — lunes 8 de junio de 2026
### Fundamentos de Sass

## 1. Variables

Guardan un valor reutilizable. Se declaran con `$`. Cambias una línea y se actualiza
todo el sitio.

```scss
$color-primario: hsl(204, 86%, 53%);
$espaciado: 1rem;
$radio: 0.5rem;

.boton {
  background-color: $color-primario;
  padding: $espaciado ($espaciado * 2);  // los cálculos van entre paréntesis
  border-radius: $radio;
}
```

---

## 2. Nesting (anidamiento)

Escribimos selectores dentro de otros para reflejar la jerarquía del HTML.
El símbolo **`&`** significa "el selector padre".

```scss
.nav {
  a {
    color: white;
    &:hover { text-decoration: underline; }   // -> .nav a:hover
  }
}

.boton {
  &--primario { background: blue; }            // -> .boton--primario
}
```

⚠️ No anides más de **3 niveles**: genera selectores enormes y difíciles de mantener.

---

## 3. Compilar

Traducir `.scss` → `.css`. Dos formas:

**A) VS Code (sin instalar nada):** extensión **Live Sass Compiler** → botón
**Watch Sass** en la barra inferior. Al guardar, se regenera el `.css`.

**B) Terminal (CLI de Sass):**
```bash
npm install -g sass                 # una sola vez
sass scss/estilos.scss css/estilos.css      # compilar una vez
sass --watch scss:css               # modo watch (recompila al guardar)
```

---

## 4. Minificar

Para **producción** queremos el CSS más liviano posible (carga más rápido). Misma
apariencia, menos peso.

```bash
sass scss/estilos.scss css/estilos.min.css --style=compressed
```

El resultado queda en una sola línea, sin espacios ni saltos. Comparen el peso del
`.css` normal vs el `.min.css`.

## DÍA 2 — martes 9 de junio de 2026
### Organizar y reutilizar el código Sass

## 5. Scoping de variables

CSS no tiene ámbitos reales; Sass sí. Una variable dentro de un bloque es **local**.
Si ya existe una global con el mismo nombre, la local la "sombrea" (shadowing) solo ahí.

```scss
$color: blue;          // global
.caja  { $color: red;  // local: solo vale aquí
         background: $color; }   // red
.otra  { background: $color; }   // blue (la global)
```

- `!global` cambia la variable global desde dentro de un bloque.
- `!default` asigna un valor **solo si la variable no existe** (útil en configuraciones).

---

## 6. Mixins (@mixin / @include)

Un **mixin** agrupa propiedades que se repiten para reutilizarlas (principio DRY).
Pueden recibir **argumentos** (con valores por defecto) y un bloque con `@content`.

```scss
@mixin boton($fondo) {
  background: $fondo;
  color: white;
  border-radius: 8px;
}
.btn-primario { @include boton(blue); }
.btn-peligro  { @include boton(red); }
```

`@content` es ideal para media queries responsive:

```scss
@mixin responsive($bp) {
  @media (min-width: $bp) { @content; }
}
.caja { @include responsive(768px) { padding: 2rem; } }
```

---

## 7. Partials y @use

Dividimos el código en **partials**: archivos que empiezan con guion bajo
(`_archivo.scss`) y que no se compilan solos. Se cargan con **`@use`**.

```scss
// _config.scss  (partial)
$primario: blue;

// estilos.scss
@use 'config';            // sin _ ni extensión
.barra { background: config.$primario; }   // namespace: config.
```

- `@use 'config' as cfg;` le da un alias corto (`cfg.$primario`).
- **`@import` está desaconsejado** (vuelve todo global y se va a eliminar). Usa `@use`.

---

## 8. Patrón 7-1

Forma estándar de organizar proyectos grandes: varias carpetas + un archivo raíz
(`main.scss`) que **solo** reúne todo con `@use`, sin reglas propias.

```
scss/
├── abstracts/   (variables, mixins, funciones — no generan CSS)
├── base/        (reset, tipografía)
├── components/  (botones, cards, badges...)
├── layout/      (header, footer, grid...)
└── main.scss    (solo @use de todo lo anterior)
```

Recuerda: compila **solo `main.scss`**; los `_partials` no se compilan por separado.

---

## 9. Funciones y estructuras de control

Sass tiene lógica de programación para generar CSS automáticamente.

```scss
@use 'sass:math';
@function rem($px) { @return math.div($px, 16) * 1rem; }  // rem(24) -> 1.5rem

// @each recorre una lista o mapa
$estados: ('ok': green, 'error': red);
@each $nombre, $color in $estados {
  .texto-#{$nombre} { color: $color; }
}

// @for repite N veces
@for $i from 1 through 3 { .mt-#{$i} { margin-top: #{$i * 0.25}rem; } }

// @if / @else: lógica condicional
@mixin texto($oscuro: false) {
  @if $oscuro { color: white; } @else { color: black; }
}
```

De pocas líneas Sass genera muchas clases: ahí está su verdadero poder.

---

---

## Buenas prácticas vistas

- Colores en `hsl()` o `rgb()` (evitar hexadecimales).
- Strings entre comillas simples.
- Cero a la izquierda en decimales (`0.5rem`, no `.5rem`) y `0` sin unidad.
- Indentación de 2 espacios y líneas ~80 caracteres.
- Cálculos numéricos entre paréntesis.

---

## Su tarea

Estilizar la **landing page** entregada usando Sass (variables, nesting, compilar y
minificar), personalizarla, subirla a un **repositorio de GitHub** y publicarla en
**GitHub Pages**. Entregan dos enlaces: el del repositorio y el de la página publicada.
Todos los detalles están en [tarea dia 1](https://jpdhrelator.github.io/cv2-modulo-3-leccion-3/),[tarea dia 2](https://jpdhrelator.github.io/cv2-modulo-3-leccion-3/dia-2/) .


