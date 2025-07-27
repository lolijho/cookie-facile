# 🚀 Guida Deploy Cookie Facile

## 1. Deploy Documentazione (Vercel)

### Setup
```bash
# Vai nella folder docs
cd docs

# Installa dipendenze
npm install

# Build locale
npm run docs:build
```

### Deploy su Vercel
1. **GitHub**: Push il progetto su GitHub
2. **Vercel Dashboard**: Vai su [vercel.com](https://vercel.com)
3. **Import Project**: Connetti GitHub repo
4. **Configurazione**:
   - **Framework**: VitePress
   - **Root Directory**: `docs`
   - **Build Command**: `npm run docs:build`
   - **Output Directory**: `docs/.vitepress/dist`

### Configurazione Vercel (vercel.json)
```json
{
  "buildCommand": "cd docs && npm run docs:build",
  "outputDirectory": "docs/.vitepress/dist",
  "installCommand": "cd docs && npm install"
}
```

## 2. Deploy Playground (Vercel)

### Setup
```bash
# Vai nella folder playground
cd playground

# Installa dipendenze
npm install

# Build
npm run build
```

### Deploy su Vercel
1. **Nuovo Progetto** su Vercel
2. **Configurazione**:
   - **Framework**: Astro
   - **Root Directory**: `playground`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`

## 3. Deploy Libreria (NPM)

### Preparazione
```bash
# Torna alla root
cd ..

# Build libreria
npm run build

# Test
npm test
```

### Pubblicazione NPM
```bash
# Login NPM
npm login

# Pubblica
npm publish
```

### CDN Automatico
Dopo la pubblicazione NPM, la libreria sarà disponibile su:
- **jsDelivr**: `https://cdn.jsdelivr.net/npm/cookie-facile@latest/dist/cookieconsent.umd.js`
- **unpkg**: `https://unpkg.com/cookie-facile@latest/dist/cookieconsent.umd.js`

## 4. Configurazione Domini

### Update package.json
```json
{
  "homepage": "https://cookie-facile.tuodominio.com",
  "repository": {
    "url": "https://github.com/tuousername/cookie-facile"
  }
}
```

### Vercel Custom Domain
1. **Project Settings** → **Domains**
2. **Add Domain**: `cookie-facile.tuodominio.com`
3. **DNS Setup**: Aggiungi CNAME al tuo DNS

## 5. Environment Variables

### Vercel Environment Variables
- `NODE_ENV=production`
- `BASE_URL=https://cookie-facile.tuodominio.com`

## 6. Deployment Commands

```bash
# Build tutto
npm run build

# Deploy docs
cd docs && npm run docs:build

# Deploy playground
cd playground && npm run build

# Test
npm test

# Pubblica libreria
npm publish
```

## 7. Automatizzazione (GitHub Actions)

Crea `.github/workflows/deploy.yml`:
```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 18
          
      - name: Install & Build
        run: |
          npm install
          npm run build
          npm test
          
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

## 8. Checklist Deploy

- [ ] Build libreria: `npm run build`
- [ ] Test: `npm test`
- [ ] Update version in package.json
- [ ] Commit & push su GitHub
- [ ] Deploy docs su Vercel
- [ ] Deploy playground su Vercel
- [ ] Pubblica su NPM: `npm publish`
- [ ] Verifica CDN funzionanti
- [ ] Test integrazione completa