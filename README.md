# PrestaPay — Landing site

Sitio estático de PrestaPay (`prestapay.app`).

## Stack

- HTML estático
- React 18 + Babel (cargados desde CDN, sin build step)
- Google Fonts: Bricolage Grotesque, Inter, JetBrains Mono

## Estructura

```
.
├── index.html                 ← landing page principal (ES/EN toggle)
├── assets/
│   └── prestapay-logo.png     ← logo
├── vercel.json                ← config de Vercel (cache headers)
└── README.md
```

## Desarrollo local

No requiere build. Cualquier servidor estático sirve:

```bash
# Python
python3 -m http.server 8000

# Node
npx serve .
```

Abrir http://localhost:8000

## Despliegue

Hosted en Vercel, conectado a este repo de GitHub. Cada `push` a `main` se despliega automáticamente.

Ver `DEPLOY.md` para instrucciones detalladas.

---

© 2026 PrestaPay · IBBA Group
