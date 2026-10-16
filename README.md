# Site appmeuciclo.com.br

Site estático, sem build, sem JavaScript, sem terceiros. Três páginas:

| Página | Arquivo | Uso |
| --- | --- | --- |
| `/` | `index.html` | Apresentação do aplicativo |
| `/privacidade` | `privacidade.html` | URL de política de privacidade exigida pelo Play Console |
| `/excluir-conta` | `excluir-conta.html` | URL de exclusão de conta exigida pelo Play Console (Data Safety → Data deletion) |

Os links internos são relativos (`privacidade.html`, `assets/styles.css`), então as
páginas funcionam abertas direto do disco, no GitHub Pages (inclusive na URL de
projeto `usuario.github.io/repositorio/`) e em qualquer outra hospedagem estática.

`_headers` e `_redirects` são lidos pelo Cloudflare Pages e pelo Netlify. No GitHub
Pages eles são ignorados: as URLs limpas continuam funcionando (`/privacidade` serve
`privacidade.html`), mas os cabeçalhos de segurança não.

## Publicar no GitHub Pages (hospedagem atual)

Este repositório é privado e o GitHub Pages gratuito só publica repositórios
públicos, então o site é publicado pelo repositório público
[`juniorJBF/meuciclo-site`](https://github.com/juniorJBF/meuciclo-site), que contém
só esta pasta (na raiz). O Pages dele serve a branch `main` e o endereço é
`https://juniorjbf.github.io/meuciclo-site/`.

A fonte da verdade continua sendo esta pasta. Depois de alterar algo aqui e fazer
commit na `main`, reenvie para o repositório do site:

```sh
git subtree push --prefix site https://github.com/juniorJBF/meuciclo-site.git main
```

Para usar `appmeuciclo.com.br`: no `meuciclo-site`, Settings → Pages → Custom
domain, e no registro.br crie o `CNAME www` → `juniorjbf.github.io` e os registros
`A` do apex apontando para os IPs do GitHub Pages (documentação oficial do GitHub).
Marque **Enforce HTTPS** quando o certificado for emitido.

Confira `/privacidade` e `/excluir-conta` antes de colar as URLs no Play Console.

Limitação: o GitHub Pages não aplica os cabeçalhos de `_headers` (CSP etc.). Se isso
voltar a ser requisito, use o Cloudflare Pages (instruções abaixo).

## Alternativa: Cloudflare Pages

1. No registro.br, troque os servidores DNS para os que a Cloudflare indicar ao
   adicionar o domínio (plano gratuito).
2. Cloudflare → Workers & Pages → Create → Pages → conecte o repositório.
   Build command: vazio. Build output directory: `site`.
3. Custom domains: `appmeuciclo.com.br` e `www.appmeuciclo.com.br`.
4. Em SSL/TLS, modo **Full (strict)** e **Always Use HTTPS**.
5. Confira `https://appmeuciclo.com.br/privacidade` e `/excluir-conta` antes de colar
   as URLs no Play Console.

## E-mails usados nas páginas

Todas as páginas usam `meucicloa@gmail.com`, caixa dedicada ao aplicativo
(suporte e privacidade no mesmo endereço; o assunto do mailto distingue os fluxos).
Se um dia houver endereços no domínio (ex.: via Cloudflare Email Routing), troque-os
nas três páginas.

## Pendências de conteúdo

- A política de privacidade está marcada como **versão em revisão** até a revisão
  jurídica do [ADR-0008](../docs/adr/0008-distribuicao-bloqueada-ate-revisao-lgpd.md):
  prazos de retenção, base legal por finalidade e o tratamento de menores de 18 anos
  precisam ser confirmados por quem assina a revisão. Remova o aviso `.draft` e preencha
  a data quando isso acontecer.
- As telas na home são reproduções em HTML das telas reais. Para trocar por capturas do
  aparelho, substitua cada `<figure>` por `<img src="/assets/screens/...png" alt="...">`
  com 1080×2340 ou proporção 9:19,5 e `width="230"`.
- `/.well-known/assetlinks.json` (App Links do login) entra quando houver a impressão
  digital do certificado do Play App Signing. Ver `docs/SECURITY.md`.
