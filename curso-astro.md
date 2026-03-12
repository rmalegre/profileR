# 🚀 Curso Completo de Astro Web Framework

## ¿Qué es Astro?

Astro es un **framework web moderno** diseñado para crear sitios web rápidos y orientados al contenido:

- **Zero JavaScript por defecto** - Envía solo HTML estático al navegador
- **Islands Architecture** - Carga JavaScript solo donde se necesita
- **Framework agnostic** - Usa React, Vue, Svelte o ninguno
- **Perfecto para**: blogs, portfolios, documentación, sitios de marketing

### ¿Por qué Astro?

| Característica | Astro | Next.js | Gatsby |
|----------------|-------|---------|--------|
| Velocidad | ✅ Muy rápida | 🟡 Media | 🟡 Media |
| JavaScript | ✅ Mínimo | ❌ Mucho | ❌ Mucho |
| Learning Curve | 🟡 Media | ❌ Alta | ❌ Alta |
| Lighthouse Score | 100 | ~80 | ~80 |

---

## Lección 1: Instalación y Configuración

### Requisitos Previos
- Node.js 18.17.1+ (recomendado 20+)
- npm, pnpm o yarn

### Crear un Proyecto

```bash
# Crear proyecto interactivo
npm create astro@latest mi-portfolio

# O con opciones
npm create astro@latest mi-portfolio -- --template minimal --no-git --no-install
cd mi-portfolio
npm install
```

### Opciones del Wizard
1. ¿Cómo quieres empezar? → **Empty** o **Blog**
2. ¿Instalar dependencias? → **Yes**
3. ¿Inicializar Git? → **Yes/No**
4. ¿TypeScript? → **Strict** (recomendado)

---

## Lección 2: Estructura del Proyecto

```
mi-proyecto/
├── public/              # Archivos estáticos (imágenes, fuentes)
├── src/
│   ├── components/      # Componentes reutilizables
│   ├── layouts/         # Plantillas de página
│   ├── pages/           # Rutas (file-based routing)
│   ├── content/         # Colecciones de contenido (Markdown)
│   └── styles/          # Estilos globales
├── astro.config.mjs     # Configuración de Astro
├── package.json
└── tsconfig.json
```

### Ejecutar el Servidor

```bash
npm run dev     # Desarrollo en http://localhost:4321
npm run build   # Producción
npm run preview # Previsualizar build
```

---

## Lección 3: Componentes en Astro

Los componentes Astro tienen extensión `.astro` y tres partes:

1. **Frontmatter** (`---`) - JavaScript/TypeScript
2. **Template** - HTML con expresiones JS
3. **Styles** - CSS scoped (opcional)

### Ejemplo: Componente Simple

```astro
---
// src/components/Saludo.astro
const nombre = "Rodrigo";
const colores = ["rojo", "azul", "verde"];
---

<h1>Hola, {nombre}!</h1>
<ul>
  {colores.map((color) => (
    <li style={`color: ${color}`}>{color}</li>
  ))}
</ul>

<style>
  h1 {
    color: #ff5d01;
  }
</style>
```

### Usar Componente

```astro
---
// src/pages/index.astro
import Saludo from '../components/Saludo.astro';
---

<Saludo />
```

---

## Lección 4: Props en Componentes

### Definir Props

```astro
---
// src/components/Card.astro
interface Props {
  titulo: string;
  descripcion?: string;  // opcional
}

const { titulo, descripcion } = Astro.props;
---

<article class="card">
  <h2>{titulo}</h2>
  {descripcion && <p>{descripcion}</p>}
</article>

<style>
  .card {
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 8px;
  }
</style>
```

### Usar con Props

```astro
---
import Card from '../components/Card.astro';
---

<Card titulo="Mi Portfolio" descripcion="Un sitio increíble" />
```

---

## Lección 5: Layouts

Los layouts son plantillas que envuelven páginas.

### Layout Base

```astro
---
// src/layouts/Layout.astro
interface Props {
  titulo: string;
  descripcion?: string;
}

const { titulo, descripcion = "Mi sitio web" } = Astro.props;
---

<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>{titulo}</title>
    <meta name="description" content={descripcion} />
  </head>
  <body>
    <header>
      <nav>
        <a href="/">Inicio</a>
        <a href="/about">Sobre mí</a>
        <a href="/blog">Blog</a>
      </nav>
    </header>
    
    <main>
      <slot />  <!-- Aquí va el contenido de la página -->
    </main>
    
    <footer>
      <p>&copy; 2026 Mi Portfolio</p>
    </footer>
  </body>
</html>

<style is:global>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font-family: system-ui, sans-serif;
    line-height: 1.6;
  }
</style>
```

### Usar Layout en Página

```astro
---
// src/pages/index.astro
import Layout from '../layouts/Layout.astro';
---

<Layout titulo="Mi Portfolio">
  <h1>Bienvenido</h1>
  <p>Este es mi sitio personal.</p>
</Layout>
```

---

## Lección 6: Routing (File-Based Routing)

Astro usa **enrutamiento basado en archivos**:

| Archivo | Ruta |
|--------|------|
| `src/pages/index.astro` | `/` |
| `src/pages/about.astro` | `/about` |
| `src/pages/blog/index.astro` | `/blog` |
| `src/pages/blog/[slug].astro` | `/blog/:slug` (dinámico) |

### Rutas Dinámicas

```astro
---
// src/pages/blog/[slug].astro
export function getStaticPaths() {
  return [
    { params: { slug: 'primero' }, props: { titulo: 'Primer Post' } },
    { params: { slug: 'segundo' }, props: { titulo: 'Segundo Post' } },
  ];
}

const { titulo } = Astro.props;
const { slug } = Astro.params;
---

<h1>{titulo}</h1>
<p>Slug: {slug}</p>
```

---

## Lección 7: Islands Architecture

Astro envía **cero JavaScript** por defecto. Las "islas" son componentes interactivos.

### Agregar React/Vue/Svelte

```bash
npx astro add react    # Agregar React
npx astro add vue      # Agregar Vue
npx astro add svelte   # Agregar Svelte
```

### Usar Componentes Interactivos

```astro
---
import Contador from '../components/Contador.jsx';
import Menu from '../components/Menu.jsx';
---

<!-- Se ejecuta solo en cliente cuando es visible -->
<Menu client:visible />

<!-- Se ejecuta inmediatamente en cliente -->
<Contador client:load />
```

### Directivas de Cliente

| Directive | Cuándo carga |
|-----------|---------------|
| `client:load` | Inmediatamente |
| `client:visible` | Cuando entra en viewport |
| `client:idle` | Cuando el navegador está idle |
| `client:only` | Solo en cliente (no SSR) |

---

## Lección 8: Content Collections (Markdown)

### Configurar Colecciones

```bash
# Crear estructura
mkdir -p src/content/blog
```

```typescript
// src/content/config.ts
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    titulo: z.string(),
    descripcion: z.string(),
    fecha: z.date(),
    imagen: z.string().optional(),
    activo: z.boolean().default(true),
  }),
});

export const collections = { blog };
```

### Crear Post en Markdown

```markdown
---
# src/content/blog/mi-primer-post.md
titulo: "Mi Primer Post"
descripcion: "Aprendiendo Astro es increíble"
fecha: 2026-03-11
---

# Hola Mundo

Este es mi primer post en **Astro**.

## Código

```javascript
console.log('¡Hola!');
```
```

### Listar Posts

```astro
---
// src/pages/blog/index.astro
import { getCollection } from 'astro:content';

const posts = await getCollection('blog');
---

<h1>Blog</h1>

{posts.map((post) => (
  <article>
    <h2><a href={`/blog/${post.slug}`}>{post.data.titulo}</a></h2>
    <p>{post.data.descripcion}</p>
    <time>{post.data.fecha.toLocaleDateString()}</time>
  </article>
))}
```

### Mostrar Post Individual

```astro
---
// src/pages/blog/[...slug].astro
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map(post => ({
    params: { slug: post.slug },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await post.render();
---

<Layout titulo={post.data.titulo}>
  <article>
    <h1>{post.data.titulo}</h1>
    <Content />
  </article>
</Layout>
```

---

## Lección 9: GraphQL en Astro

### Instalar GraphQL

```bash
npm install graphql
```

### Ejemplo: Consultar API GraphQL

```astro
---
// src/pages/graphql-example.astro
const query = `
  query {
    characters(first: 5) {
      name
      species
    }
  }
`;

const response = await fetch('https://rickandmortyapi.com/graphql', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ query }),
});

const { data } = await response.json();
---

<h1>Personajes de Rick and Morty</h1>

<ul>
  {data.characters.map((char: any) => (
    <li>{char.name} - {char.species}</li>
  ))}
</ul>
```

### Con Apollo Client (opcional)

```bash
npm install @apollo/client graphql
```

---

## Lección 10: Estilos y Tailwind CSS

### Agregar Tailwind

```bash
npx astro add tailwind
```

### Usar Tailwind

```astro
---
// src/components/Boton.astro
---

<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  <slot />
</button>
```

### Estilos Globales

```astro
<style is:global>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
  
  body {
    font-family: 'Inter', sans-serif;
  }
</style>
```

---

## Lección 11: Despliegue

### Build para Producción

```bash
npm run build
```

### Opciones de Hosting

| Servicio | Comando |
|----------|---------|
| **Vercel** | `npm i -g vercel` → `vercel` |
| **Netlify** | `npm i -g netlify-cli` → `netlify deploy` |
| **Cloudflare** | `npm i -g wrangler` → `wrangler pages deploy dist` |
| **GitHub Pages** | Configurar en repo settings |

### Despliegue Automático

1. Sube tu código a GitHub
2. Conecta el repo a Vercel/Netlify
3. ¡Listo! Deploy automático en cada push

---

## Recursos Adicionales

- **Documentación oficial**: https://docs.astro.build
- **Showcase**: https://astro.build/showcase
- **Discord**: https://astro.build/chat
- **ASTRO.new**: https://astro.new (playground online)

---

## Ejercicio Final: Crea tu Portfolio

Crea un portfolio con:

1. ✅ Página de inicio con presentación
2. ✅ Página "Sobre mí"
3. ✅ Blog con Markdown
4. ✅ Componente de contacto (puede ser un form simple)
5. ✅ Diseño responsivo con Tailwind
6. ✅ Despliegue en Vercel

---

**¡Felicitaciones! 🎉 Has completado el curso de Astro**

Continúa practicando y explorando la documentación oficial para aprender más.
