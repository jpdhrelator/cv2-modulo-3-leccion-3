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
Todos los detalles están en [aqui](https://jpdhrelator.github.io/cv2-modulo-3-leccion-3/).
