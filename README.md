# maximilianoide.github.io

Página personal de **Maximiliano Ide**, Software Engineer. Sitio 100 % estático con estética de
menú de pausa de JRPG: la navegación, el cambio de idioma y todas las animaciones son CSS.

- `index.html`: todo el contenido (EN/ES) escrito directamente en el HTML
- `styles.css`: estilos, navegación con `:target`/`:has()` y animaciones
- `404.html`: página de error con la misma estética (autocontenida)
- `favicon.svg`: favicon original
- `nav.js`: **opcional** (<1 KB). Añade navegación con ↑/↓ y sincroniza `aria-current`. El sitio funciona completo sin él.

## Verlo en local

Abre `index.html` directamente en el navegador, o levanta un servidor:

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

> `python3 -m http.server` no sirve `404.html` para rutas inexistentes (GitHub Pages sí).
> Para verla, abre `http://localhost:8000/404.html`.

## Editar los datos

Todo el contenido está en `index.html`, una `<section class="panel">` por entrada del menú:

| Menú         | `id`            | Qué contiene                                  |
|--------------|-----------------|-----------------------------------------------|
| STATUS       | `#status`       | Avatar, rol, ubicación, resumen, idiomas      |
| SKILLS       | `#skills`       | Grupos `.skill-group` con tarjetas `.skill`   |
| QUESTS       | `#quests`       | Repos públicos (`.quest`)                     |
| SOCIAL LINK  | `#social-link`  | Experiencia actual y educación                |
| ACHIEVEMENTS | `#achievements` | Certificaciones (`.trophy`)                   |
| CONTACT      | `#contact`      | Email, GitHub, LinkedIn, X                    |

**Bilingüe:** cada texto traducible existe dos veces, con `lang="en"` y `lang="es"`:

```html
<span lang="en">Full-time</span><span lang="es">Jornada completa</span>
```

El CSS oculta el idioma no seleccionado (radios `#lang-en` / `#lang-es` + `:has()`).
No pongas `lang="es"` en elementos que deban verse siempre, porque se ocultarán en inglés.

**Nueva sección:** duplica una `<section class="panel" id="…">`, añade su `<li class="menu__item">`
en el `<nav>` (con `style="--i:N"`) y agrega el par `body:has(#nuevo:target) .menu__link[href="#nuevo"]`
a la lista de selectores del ítem seleccionado en `styles.css`.

**Repos / estrellas / seguidores:** están escritos a mano. Para actualizarlos:

```sh
curl -s https://api.github.com/users/maximilianoide
curl -s https://api.github.com/users/maximilianoide/repos
```

**Colores y tipografías:** custom properties en `:root` al inicio de `styles.css`.

## Publicar en GitHub Pages

1. Crea en GitHub un repositorio público llamado exactamente **`maximilianoide.github.io`**.
2. En esta carpeta:
   ```sh
   git init
   git add index.html styles.css 404.html favicon.svg nav.js README.md
   git commit -m "Personal page"
   git branch -M main
   git remote add origin https://github.com/maximilianoide/maximilianoide.github.io.git
   git push -u origin main
   ```
3. En el repo: **Settings → Pages → Build and deployment → Source: "Deploy from a branch"**,
   rama **`main`**, carpeta **`/ (root)`** → *Save*.
4. En uno o dos minutos queda en **https://maximilianoide.github.io/**.
   Cada `git push` a `main` vuelve a publicar.

## Compatibilidad

Todo lo moderno viene con fallback (`@supports`). Sin `:has()` el sitio se muestra como una sola
página con scroll y solo en inglés. Sin scroll-driven animations, `@starting-style` o View
Transitions, simplemente hay menos animación. Con `prefers-reduced-motion: reduce` se desactiva
todo el movimiento.
