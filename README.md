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

O workflow [`site-pages.yml`](../.github/workflows/site-pages.yml) publica a pasta
`site/` a cada push na `main` que a altere.

1. No GitHub: repositório → Settings → Pages → Build and deployment →
   **Source: GitHub Actions** (uma única vez).
2. Faça push (ou rode o workflow manualmente em Actions → Site (GitHub Pages) →
   Run workflow). O endereço fica `https://<usuario>.github.io/<repositorio>/`.
3. Para usar `appmeuciclo.com.br`: Settings → Pages → Custom domain, e no
   registro.br crie o `CNAME www` → `<usuario>.github.io` e os registros `A` do apex
   apontando para os IPs do GitHub Pages (documentação oficial do GitHub). Marque
   **Enforce HTTPS** quando o certificado for emitido.
4. Confira `/privacidade` e `/excluir-conta` antes de colar as URLs no Play Console.

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

`suporte@appmeuciclo.com.br` e `privacidade@appmeuciclo.com.br`. Crie-os (Cloudflare
Email Routing redireciona gratuitamente para uma caixa existente) antes de publicar,
ou troque os endereços nas três páginas.

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
