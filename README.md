# MEYRX: site institucional

Site estático de [meyrx.com.br](https://meyrx.com.br), usado como site da organização, Política de Privacidade e página de exclusão de conta dos apps da MEYRX na Google Play Store.

HTML + CSS puro, sem framework, sem JavaScript e sem etapa de build.

## Estrutura

```
.
├── index.html               → /               início, "Nossos apps" e contato
├── privacidade/index.html   → /privacidade/   Política de Privacidade (LGPD + Google Play)
├── termos/index.html        → /termos/        Termos de Uso
├── suporte/index.html       → /suporte/       suporte e FAQ
├── excluir-conta/index.html → /excluir-conta/ exclusão de conta (exigida pelo Google Play)
├── 404.html                 → página de erro (o Cloudflare Pages usa automaticamente)
├── assets/
│   ├── css/style.css        → único CSS do site (mobile first, tema claro/escuro automático)
│   └── img/
│       ├── favicon.svg
│       └── logo.svg         → logotipo para uso externo (no site, o logo está embutido no HTML)
├── _headers                 → cabeçalhos HTTP de segurança e cache do Cloudflare Pages
├── robots.txt
└── sitemap.xml
```

O cabeçalho e o rodapé são **iguais em todas as páginas** e estão copiados em cada HTML. Ao alterar algum deles (um link do menu, por exemplo), faça a mesma mudança em todos os arquivos `.html` com um localizar e substituir no editor.

## URLs para o Google Play Console

| Campo no Play Console               | URL                                    |
| ----------------------------------- | -------------------------------------- |
| Site                                | https://meyrx.com.br                   |
| Política de Privacidade             | https://meyrx.com.br/privacidade/      |
| Exclusão de conta (Segurança dos dados) | https://meyrx.com.br/excluir-conta/ |

## Apps publicados

| App      | Pacote                     |
| -------- | -------------------------- |
| Mensalix | `br.com.meyrx.mensalix`    |

## Conteúdo pendente

No momento, não há nada pendente. Se precisar marcar um trecho para preencher depois, use `<mark class="todo">{{PREENCHER: ...}}</mark>`: ele aparece destacado em amarelo no site. Para listar os pendentes, rode `git grep -n "{{PREENCHER"`.

## Como adicionar um novo app

1. **Página inicial** (`index.html`): duplique o bloco `<article class="card app-card">` que está entre os comentários `CARD DE APP`.
2. **Privacidade** (`privacidade/index.html`): duplique o bloco `<section>` entre os comentários `SEÇÃO DE APP` (14.1, 14.2, …).
3. Inclua os passos de exclusão de dados do novo app em `excluir-conta/index.html`. Hoje a página descreve o caminho do Mensalix.
4. Atualize a data de "Última atualização" nas páginas alteradas e o `<lastmod>` no `sitemap.xml`.

## Testar localmente

Qualquer servidor estático serve. Por exemplo:

```bash
python -m http.server 8000
# abra http://localhost:8000
```

## Publicar no Cloudflare Pages

1. Cloudflare Dashboard → Workers & Pages → Create → Pages → **Connect to Git** → escolha este repositório.
2. Configuração de build:
   - Framework preset: **None**
   - Build command: *(vazio)*
   - Build output directory: **`/`**
   - Production branch: **`main`**
3. Depois do primeiro deploy, adicione o domínio personalizado `meyrx.com.br` (e `www.meyrx.com.br`, se quiser) em **Custom domains**.

Cada `git push` na branch `main` publica o site automaticamente.

> O Cloudflare Pages serve `pasta/index.html` em `/pasta/` e redireciona `/pasta` para `/pasta/`, por isso os links internos e o sitemap usam barra no final.

## Segurança

O `_headers` aplica `Content-Security-Policy` restritiva (sem scripts ou estilos inline), HSTS, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy` e `Permissions-Policy`. Se um dia você adicionar JavaScript ou recursos externos, ajuste a CSP.
