# Configuração SPA para Vercel

## 🎯 Problema Resolvido

**Antes:** Erro 404 ao acessar rotas diretamente (como `/produtos`) ou pressionar F5
**Depois:** Todas as rotas funcionam corretamente! ✅

## 📋 Arquivos da Solução

### 1. vercel.json
Este é o arquivo principal que resolve o problema de roteamento no Vercel:

```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

**O que faz:**
- Redireciona TODAS as requisições para `index.html`
- O JavaScript no `index.html` então processa a rota correta
- Funciona perfeitamente com SPAs (React, Vue, Angular, etc.)

### 2. index.html
Aplicação SPA simples com roteamento client-side implementado.

## 🚀 Como Funciona

1. **Usuário acessa:** `https://seu-site.vercel.app/produtos`
2. **Vercel recebe** a requisição para `/produtos`
3. **vercel.json reescreve** para `/index.html`
4. **index.html carrega** com JavaScript
5. **JavaScript lê** `window.location.pathname` (`/produtos`)
6. **Router renderiza** o conteúdo correto para `/produtos`
7. ✅ **Usuário vê** a página de produtos!

## ✅ O Que Funciona Agora

- ✅ Navegação interna entre páginas
- ✅ Acesso direto a qualquer rota via URL
- ✅ Pressionar F5 em qualquer página
- ✅ Compartilhar links específicos
- ✅ Botões voltar/avançar do navegador
- ✅ Bookmarks/Favoritos

## 🧪 Testando

1. **Deploy no Vercel**
   - Conecte seu repositório ao Vercel
   - O deploy é automático!

2. **Teste as rotas:**
   ```
   https://seu-site.vercel.app/
   https://seu-site.vercel.app/produtos
   https://seu-site.vercel.app/sobre
   ```

3. **Teste o F5:**
   - Acesse qualquer rota
   - Pressione F5
   - ✅ A página recarrega corretamente!

## 🔧 Usando com Frameworks

### React
```json
// vercel.json já está configurado!
// Apenas use BrowserRouter normalmente:
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/produtos" element={<Produtos />} />
  </Routes>
</BrowserRouter>
```

### Vue.js
```javascript
// vercel.json já está configurado!
// Use createWebHistory normalmente:
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', component: Home },
    { path: '/produtos', component: Produtos }
  ]
})
```

### Angular
```typescript
// vercel.json já está configurado!
// Angular Router já usa pushState por padrão
RouterModule.forRoot(routes)
```

## 📚 Diferenças: Vercel vs GitHub Pages

| Aspecto | Vercel | GitHub Pages |
|---------|--------|--------------|
| Configuração | `vercel.json` com rewrites | `404.html` hack + `.nojekyll` |
| Simplicidade | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| Suporte Oficial | ✅ Nativo | 🔧 Workaround |
| Performance | 🚀 Excelente | ✅ Boa |

## 🎨 Personalização

Para adicionar mais rotas, edite o `index.html`:

```javascript
switch(path) {
  case '/':
    // Conteúdo da home
    break;
  case '/produtos':
    // Conteúdo de produtos
    break;
  case '/nova-rota':
    // Novo conteúdo aqui
    break;
}
```

## 🐛 Troubleshooting

### Ainda recebo 404
- ✅ Certifique-se de que `vercel.json` está na raiz do projeto
- ✅ Faça redeploy no Vercel
- ✅ Limpe o cache do navegador (Ctrl+Shift+R)

### Página em branco
- ✅ Verifique o console do navegador (F12)
- ✅ Certifique-se de que o JavaScript está carregando

## 📖 Referências

- [Vercel Rewrites Documentation](https://vercel.com/docs/concepts/projects/project-configuration#rewrites)
- [SPA Routing Best Practices](https://vercel.com/guides/deploying-react-with-vercel#routing)

---

**Configuração simples e eficiente para SPAs no Vercel! 🚀**
