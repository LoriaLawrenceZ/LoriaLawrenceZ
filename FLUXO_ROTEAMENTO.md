# Fluxo de Roteamento SPA

## Como Funciona a Solução

### Cenário 1: Navegação Interna (Client-Side)
```
Usuário está em: /
Clica em link para: /produtos

┌──────────────┐
│  JavaScript  │
│  do Router   │──> Atualiza URL para /produtos
└──────────────┘    Renderiza conteúdo de produtos
                    (sem recarregar página)
```

✅ **Resultado**: Funciona perfeitamente (sempre funcionou)

---

### Cenário 2: Acesso Direto ou F5 (ANTES da solução)
```
Usuário acessa diretamente: /produtos
ou pressiona F5 em /produtos

┌──────────────────┐
│  GitHub Pages    │
│  (Servidor)      │──> Procura arquivo em /produtos
└──────────────────┘    Arquivo não existe
         │
         v
    ❌ ERRO 404
```

❌ **Resultado**: Erro 404 (problema!)

---

### Cenário 3: Acesso Direto ou F5 (DEPOIS da solução)
```
Usuário acessa: https://username.github.io/repo/produtos
ou pressiona F5

┌─────────────────────────────────────────────────────┐
│ PASSO 1: GitHub Pages procura /produtos             │
│ └──> Não encontra arquivo                           │
│ └──> Serve o 404.html automaticamente               │
└─────────────────────────────────────────────────────┘
                    │
                    v
┌─────────────────────────────────────────────────────┐
│ PASSO 2: 404.html executa JavaScript                │
│                                                      │
│ URL original: /repo/produtos                        │
│ Converte para: /repo/?/produtos                     │
│ Redireciona para index.html                         │
└─────────────────────────────────────────────────────┘
                    │
                    v
┌─────────────────────────────────────────────────────┐
│ PASSO 3: index.html carrega                         │
│                                                      │
│ Detecta query string: ?/produtos                    │
│ Usa history.replaceState()                          │
│ Restaura URL para: /repo/produtos                   │
└─────────────────────────────────────────────────────┘
                    │
                    v
┌─────────────────────────────────────────────────────┐
│ PASSO 4: JavaScript Router processa                 │
│                                                      │
│ Vê URL: /repo/produtos                              │
│ Renderiza conteúdo de produtos                      │
└─────────────────────────────────────────────────────┘
                    │
                    v
        ✅ Página carrega corretamente!
```

---

## Transformação da URL

### Exemplo Detalhado

**URL Original Acessada:**
```
https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/produtos
```

**404.html Transforma em:**
```
https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/?/produtos
```

**index.html Restaura para:**
```
https://LoriaLawrenceZ.github.io/LoriaLawrenceZ/produtos
```

### Com Query String

**URL Original:**
```
https://username.github.io/repo/produtos?id=123&categoria=tech
```

**404.html Transforma em:**
```
https://username.github.io/repo/?/produtos&id=123~and~categoria=tech
```

**index.html Restaura para:**
```
https://username.github.io/repo/produtos?id=123&categoria=tech
```

### Com Hash

**URL Original:**
```
https://username.github.io/repo/produtos#secao1
```

**404.html Transforma em:**
```
https://username.github.io/repo/?/produtos#secao1
```

**index.html Restaura para:**
```
https://username.github.io/repo/produtos#secao1
```

---

## Variável Importante: pathSegmentsToKeep

### Para Repositórios de Projeto (Project Pages)
```javascript
var pathSegmentsToKeep = 1;  // ✅ USE ESTE VALOR
```

**Exemplo:**
- URL: `https://username.github.io/repo/produtos`
- Mantém: `/repo/` (1 segmento)
- Transforma: `/repo/?/produtos`

### Para Sites de Usuário/Organização (User/Org Pages)
```javascript
var pathSegmentsToKeep = 0;  // Use este valor
```

**Exemplo:**
- URL: `https://username.github.io/produtos`
- Mantém: nenhum segmento
- Transforma: `/?/produtos`

---

## Detalhes Técnicos

### Por que .nojekyll?

O GitHub Pages usa Jekyll por padrão para processar arquivos. Jekyll pode:
- Ignorar arquivos/pastas que começam com `_`
- Modificar o conteúdo dos arquivos
- Causar problemas com SPAs

O arquivo `.nojekyll` desabilita Jekyll, garantindo que:
- ✅ Todos os arquivos são servidos como estão
- ✅ Nenhum processamento extra é feito
- ✅ Build da SPA permanece intacto

### Por que > 512 bytes?

Internet Explorer tem uma "funcionalidade" onde páginas 404 com menos de 512 bytes são substituídas por uma página de erro "amigável" do IE, impedindo que o JavaScript seja executado.

**Nossa solução:**
- 404.html tem 1741 bytes ✅
- Bem acima do limite de 512 bytes
- Funciona em todos os navegadores

### window.history.replaceState()

Esta API permite mudar a URL sem recarregar a página:

```javascript
window.history.replaceState(
  null,           // state object (não usamos)
  null,           // title (não usado pelos navegadores)
  '/produtos'     // nova URL
);
```

**Benefícios:**
- ✅ Muda a URL na barra de endereços
- ✅ Não recarrega a página
- ✅ Não adiciona entrada no histórico (replace, não push)
- ✅ Preserva o estado da aplicação

---

## Comparação: Antes vs Depois

| Situação | Antes | Depois |
|----------|-------|--------|
| Navegar internamente | ✅ Funciona | ✅ Funciona |
| Link direto para /produtos | ❌ 404 Error | ✅ Funciona |
| F5 em /produtos | ❌ 404 Error | ✅ Funciona |
| Botão Voltar/Avançar | ✅ Funciona | ✅ Funciona |
| Compartilhar link | ❌ Recebe 404 | ✅ Funciona |
| Bookmark/Favorito | ❌ Abre em 404 | ✅ Funciona |
| Abrir em nova aba | ❌ 404 Error | ✅ Funciona |

---

## Arquivos da Solução

```
├── .nojekyll          ← Desabilita Jekyll
├── 404.html           ← Intercepta 404 e redireciona
├── index.html         ← Restaura URL e renderiza
├── COMO_USAR.md       ← Guia de uso
├── SPA_ROUTING.md     ← Documentação técnica
└── FLUXO_ROTEAMENTO.md ← Este arquivo
```

---

## Performance

- **Latência adicional**: Mínima (~10-50ms)
- **Redirecionamentos**: Apenas 1 (404.html → index.html)
- **JavaScript**: Executado apenas uma vez no carregamento
- **Impacto SEO**: Nenhum (SPAs já têm considerações especiais de SEO)

---

## Compatibilidade

| Navegador | Versão Mínima | Status |
|-----------|---------------|--------|
| Chrome | Todos | ✅ |
| Firefox | Todos | ✅ |
| Safari | Todos | ✅ |
| Edge | Todos | ✅ |
| Internet Explorer | 10+ | ✅ |
| Mobile Safari | Todos | ✅ |
| Chrome Mobile | Todos | ✅ |

---

## Referências

- [spa-github-pages](https://github.com/rafgraph/spa-github-pages) - Solução original
- [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) - MDN Docs
- [GitHub Pages](https://docs.github.com/en/pages) - Documentação oficial
