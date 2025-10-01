# Como Usar a Solução de Roteamento SPA

## Problema Resolvido

✅ Agora você pode:
- Atualizar a página (F5) em qualquer rota sem receber 404
- Compartilhar links diretos para rotas específicas (ex: `/produtos`)
- Navegar usando o botão voltar/avançar do navegador

## Passos para Ativar no GitHub Pages

### 1. Habilitar GitHub Pages

1. Vá para o seu repositório no GitHub
2. Clique em **Settings** (Configurações)
3. No menu lateral, clique em **Pages**
4. Em **Source**, selecione a branch `main` (ou `master`)
5. Mantenha a pasta como `/ (root)`
6. Clique em **Save**

### 2. Aguardar o Deploy

Aguarde alguns minutos (geralmente 1-5 minutos) para o GitHub Pages processar e publicar seu site.

### 3. Acessar o Site

Seu site estará disponível em:
```
https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/
```

### 4. Testar o Roteamento

Teste as seguintes URLs para verificar que não há mais erro 404:

- `https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/` (home)
- `https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/produtos` (produtos)
- `https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/sobre` (sobre)

Pressione F5 em qualquer uma dessas páginas e veja que continua funcionando!

## Arquivos da Solução

### Arquivos Essenciais (NÃO REMOVER)

1. **404.html** - Intercepta erros 404 e redireciona corretamente
2. **.nojekyll** - Desabilita processamento Jekyll (importante!)

### Arquivos de Exemplo

3. **index.html** - Exemplo de SPA funcionando (você pode substituir com sua aplicação)
4. **SPA_ROUTING.md** - Documentação técnica detalhada
5. **COMO_USAR.md** - Este arquivo de instruções

## Substituindo o index.html

Se você já tem uma aplicação SPA (React, Vue, Angular, etc.):

1. **Mantenha** os arquivos `404.html` e `.nojekyll`
2. **Substitua** o `index.html` pelo seu arquivo HTML principal
3. **Adicione** o script de roteamento do SPA ao `<head>` do seu HTML:

```html
<!-- Start Single Page Apps for GitHub Pages -->
<script type="text/javascript">
  (function(l) {
    if (l.search[1] === '/' ) {
      var decoded = l.search.slice(1).split('&').map(function(s) { 
        return s.replace(/~and~/g, '&')
      }).join('?');
      window.history.replaceState(null, null,
          l.pathname.slice(0, -1) + decoded + l.hash
      );
    }
  }(window.location))
</script>
<!-- End Single Page Apps for GitHub Pages -->
```

## Usando com Frameworks Populares

### React (Create React App)

1. Adicione `"homepage": "https://LoriaLawrenceZ.github.io/LoriaLawrenceZ"` ao `package.json`
2. Use `npm run build` para gerar os arquivos
3. Copie o conteúdo de `build/` para a raiz do repositório
4. Certifique-se de manter `404.html` e `.nojekyll`
5. Configure o router:

```javascript
<BrowserRouter basename="/LoriaLawrenceZ">
  <App />
</BrowserRouter>
```

### Vue.js

1. Configure o `vue.config.js`:

```javascript
module.exports = {
  publicPath: '/LoriaLawrenceZ/'
}
```

2. Use `npm run build` para gerar os arquivos
3. Copie o conteúdo de `dist/` para a raiz do repositório
4. Certifique-se de manter `404.html` e `.nojekyll`

### Angular

1. Configure o `angular.json`:

```json
"baseHref": "/LoriaLawrenceZ/"
```

2. Use `ng build --base-href=/LoriaLawrenceZ/`
3. Copie o conteúdo de `dist/` para a raiz do repositório
4. Certifique-se de manter `404.html` e `.nojekyll`

## Verificando se Funcionou

✅ **Teste 1: Navegação Interna**
- Clique nos links dentro da aplicação
- Deve funcionar normalmente

✅ **Teste 2: Atualizar Página (F5)**
- Navegue para uma rota (ex: `/produtos`)
- Pressione F5
- A página deve recarregar na mesma rota, sem erro 404

✅ **Teste 3: Link Direto**
- Copie a URL de uma rota específica
- Cole em uma nova aba do navegador
- Deve carregar a página corretamente, sem erro 404

✅ **Teste 4: Botões Voltar/Avançar**
- Navegue entre páginas
- Use os botões voltar e avançar do navegador
- Deve funcionar como esperado

## Problemas Comuns

### Ainda recebo 404

1. Verifique se o GitHub Pages está ativado
2. Certifique-se de que `.nojekyll` existe na raiz
3. Aguarde alguns minutos após fazer alterações
4. Limpe o cache do navegador (Ctrl+Shift+R ou Cmd+Shift+R)

### Páginas carregam mas ficam em branco

1. Verifique o console do navegador (F12)
2. Certifique-se de que os caminhos dos seus assets estão corretos
3. Use caminhos relativos ou absolute com o basename correto

### Imagens/CSS não carregam

1. Use o basename correto nos caminhos
2. Exemplo: `/LoriaLawrenceZ/assets/image.png` ao invés de `/assets/image.png`

## Suporte

Para mais informações, consulte:
- [SPA_ROUTING.md](./SPA_ROUTING.md) - Documentação técnica
- [spa-github-pages](https://github.com/rafgraph/spa-github-pages) - Projeto original
- [GitHub Pages Docs](https://docs.github.com/en/pages) - Documentação oficial

## Licença

Esta solução é baseada no projeto [spa-github-pages](https://github.com/rafgraph/spa-github-pages) sob licença MIT.
