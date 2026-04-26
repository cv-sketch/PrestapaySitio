# Despliegue: GitHub + Vercel → prestapay.app

Guía paso a paso para publicar este sitio usando GitHub y Vercel.

---

## Paso 1 — Subir a GitHub

### 1.1. Crear el repo en GitHub

1. Entra a https://github.com/new
2. Repository name: `prestapay-landing` (o el que prefieras)
3. Visibilidad: **Private** (recomendado) o Public
4. **NO** marques "Add a README" ni `.gitignore` (ya los tenemos en el paquete)
5. Click **Create repository**

GitHub te mostrará una URL tipo:
```
https://github.com/TU_USUARIO/prestapay-landing.git
```

### 1.2. Inicializar git y subir el código

Desde tu máquina local, dentro de la carpeta `deploy/` (después de descomprimir):

```bash
cd deploy

git init
git add .
git commit -m "Initial commit: PrestaPay landing"

git branch -M main
git remote add origin https://github.com/TU_USUARIO/prestapay-landing.git
git push -u origin main
```

✅ Tu código ya está en GitHub.

> Si te pide credenciales, usa un **Personal Access Token** en lugar de contraseña: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token con scope `repo`.

---

## Paso 2 — Conectar a Vercel

### 2.1. Importar el repo

1. Entra a https://vercel.com/new
2. Inicia sesión con GitHub (autoriza la integración si es la primera vez)
3. Verás la lista de tus repos. Click **Import** junto a `prestapay-landing`.
4. Configuración:
   - **Project Name:** `prestapay`
   - **Framework Preset:** Other
   - **Root Directory:** `./` (default)
   - **Build Command:** dejar vacío
   - **Output Directory:** dejar vacío
5. Click **Deploy**.

En ~30 segundos tendrás el sitio en `prestapay.vercel.app`.

### 2.2. Auto-deploy configurado

A partir de ahora, cada `git push` a `main` despliega automáticamente. Ramas distintas a `main` generan **preview deployments** con su propia URL — útil para revisar cambios antes de mergear.

---

## Paso 3 — Conectar el dominio prestapay.app

### 3.1. Añadir el dominio en Vercel

1. Vercel → proyecto `prestapay` → **Settings** → **Domains**
2. Escribe `prestapay.app` → **Add**
3. Repite con `www.prestapay.app` (Vercel hace redirect automático a apex)

### 3.2. Configurar DNS

Vercel te mostrará una de dos opciones. Ve a tu proveedor de DNS (Linode, Cloudflare, registrador del dominio):

**Opción A — Mantener tu DNS actual:**

| Tipo  | Nombre | Valor                       |
|-------|--------|-----------------------------|
| A     | @      | `76.76.21.21`               |
| CNAME | www    | `cname.vercel-dns.com`      |

**Opción B — Mover nameservers a Vercel (más simple a largo plazo):**

En el registrador donde compraste el dominio, cambia los nameservers a los que Vercel te indique (suelen ser `ns1.vercel-dns.com` y `ns2.vercel-dns.com`).

### 3.3. Esperar propagación

5–60 min normalmente. Verifica:

```bash
dig prestapay.app +short
```

Cuando el DNS apunte correctamente, Vercel emite el SSL automáticamente. **HTTPS queda activo sin configuración adicional.**

---

## Workflow diario después del setup

```bash
# Editas el código localmente
nano index.html

# Lo subes
git add .
git commit -m "Update hero copy"
git push

# Vercel detecta el push y deploya en ~30s
```

Para preview de un cambio sin tocar producción:

```bash
git checkout -b nueva-seccion
# editas...
git push -u origin nueva-seccion
# Vercel genera una URL de preview tipo prestapay-git-nueva-seccion.vercel.app
```

Cuando estés conforme, mergeas a `main` y va a producción.

---

## Rollback

Si un deploy sale mal:

1. Vercel → proyecto → **Deployments**
2. Encuentra el deploy anterior que funcionaba
3. Click **⋯** → **Promote to Production**

Listo, en segundos.

---

## Costos

- GitHub: gratis (repo privado)
- Vercel plan **Hobby**: gratis (suficiente para este sitio)
- Dominio prestapay.app: lo que pagues a tu registrador

---

## Si en algún momento no quieres usar Vercel

El repo es 100% portable. Puedes:
- Servirlo desde Linode con Nginx (todos los archivos son estáticos)
- Subirlo a Netlify, Cloudflare Pages, GitHub Pages, etc.
