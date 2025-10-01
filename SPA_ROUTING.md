# Solução de Roteamento SPA para GitHub Pages

## Problema

Quando você tem uma Single Page Application (SPA) hospedada no GitHub Pages e tenta:
- Atualizar a página (F5) em uma rota diferente da home
- Acessar diretamente um link (ex: `https://username.github.io/repo/produtos`)

Você recebe um erro 404. Isso acontece porque o GitHub Pages procura por um arquivo físico naquele caminho, mas em uma SPA todas as rotas são gerenciadas pelo JavaScript no cliente.

## Solução

Esta solução implementa o padrão recomendado para SPAs no GitHub Pages, baseado no projeto [spa-github-pages](https://github.com/rafgraph/spa-github-pages).

### Arquivos Criados

1. **404.html** - Intercepta todas as requisições 404 e redireciona para index.html preservando a URL
2. **index.html** - Contém o script que restaura a URL original e implementa o roteamento
3. **.nojekyll** - Desabilita o processamento Jekyll do GitHub Pages

### Como Funciona

1. Quando você acessa `https://username.github.io/repo/produtos` diretamente:
   - O GitHub Pages não encontra o arquivo e serve o `404.html`
   - O script no `404.html` converte a URL para `https://username.github.io/repo/?/produtos`
   - O navegador é redirecionado para `index.html` com a rota na query string

2. O `index.html` carrega e:
   - O script de roteamento detecta a query string `?/produtos`
   - Usa `window.history.replaceState()` para restaurar a URL original `https://username.github.io/repo/produtos`
   - O roteador da sua aplicação pode então processar a rota normalmente

### Configuração

Se você estiver usando um repositório de projeto (não um User/Organization site), certifique-se de que:
- A variável `pathSegmentsToKeep` no `404.html` está configurada para `1`
- Seu roteador JavaScript lida corretamente com o prefixo do repositório

Para um User/Organization site (como `username.github.io`), use `pathSegmentsToKeep = 0`.

### Ativando GitHub Pages

1. Vá para Settings > Pages no seu repositório
2. Selecione a branch desejada (geralmente `main` ou `master`)
3. Selecione a pasta raiz (`/`) como fonte
4. Clique em Save

Aguarde alguns minutos e seu site estará disponível em `https://username.github.io/repositorio/`

### Compatibilidade

Esta solução funciona com:
- ✅ Todos os navegadores modernos
- ✅ Internet Explorer 10+ (com alguns ajustes)
- ✅ Frameworks de roteamento populares (React Router, Vue Router, etc.)

### Personalização

Para usar esta solução com seu framework preferido:
1. Mantenha os arquivos `404.html` e `.nojekyll` como estão
2. Adicione o script de roteamento do `index.html` ao seu arquivo HTML principal
3. Configure seu roteador para trabalhar com o basename correto

### Exemplo com React Router

```javascript
import { BrowserRouter } from 'react-router-dom';

<BrowserRouter basename="/nome-do-repo">
  <App />
</BrowserRouter>
```

### Exemplo com Vue Router

```javascript
const router = createRouter({
  history: createWebHistory('/nome-do-repo/'),
  routes
})
```

## Referências

- [SPA GitHub Pages](https://github.com/rafgraph/spa-github-pages)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
