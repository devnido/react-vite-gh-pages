# React Vite Github Pages y Github Actions

Este proyecto es una prueba para desplegar una web app en React y Vite a Github Pages usando Github Actions.

## Configurar el proyecto

1. Instalar depencencia de desarrollo para Github Pages
```bash
npm i -D gh-pages
```

2. Agregar **homepage** a `package.json`
```json
{
    ...
    "homepage": "https://<github-username>.github.io/<repo-name>",
    ...
}
```

3. Agregar **predeploy** y **deploy** a la sección scripts del `package.json`
```json
{
    "scripts": {
        ...
        "predeploy": "npm run build",
        "deploy": "gh-pages -d dist"
    }
}
```

4. Modificar archivo **vite.config.js** agregando la siguiente linea de codigo `base:'/<nombre de tu repo en github>/'` dentro del objeto que recibe la funcion **defineConfig**, debes reemplazar `<nombre de tu repo en github>` por el nombre de tu repo.
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  base:'/<nombre de tu repo en github>/'
})
```

## Crear workflow para Github Actions

1. Crear carpeta **.github** en la base del proyecto 

2. Crear carpeta **workflows** dentro de la carpeta **.github**

3. Crear archivo `.yml` para el workflow, puede tener cualquier nombre, por ejemplo `deploy.yml`

4. Dentro del archivo vamos a pegar el siguiente codigo:
```yml
name: DEPLOY TO GITHUB PAGES

on:
  push:
    branches:
      - "main"

jobs:
  build-and-deploy:
    name: Build and deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Install dependencies
        run: npm install

      - name: Deploy
        run: |
          git config --global user.email "<Tu email asociado a Github>"
          git config --global user.name "<Tu nombre asociado a Github>"
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }} 
          npm run deploy
```

## Habilitar permiso para el workflow

1. Abrir la pestaña settings del proyecto luego hacer click en **Actions** y **General**.

![Settings](/public/permissions-1.png)

2. Al final de la pagina donde dice **Workflow permissions** selecciona **Read and write permissions** y activa la casilla **Allow Github Actions to create and approve pull requests**.

![Settings](/public/permissions-2.png)

## Eso es todo :v:
