# Nutrição Brasil 2026 — Front-end reconstruído (deploy)

Pasta pronta para substituir o repositório do site (Vercel). Estrutura, URLs e automações preservadas.

## O que foi trocado (18 páginas, novo design)
index, ingressos, corrida, expo, visitante, programacao, cidades, belem, golden, expositores, hall-da-fama, nbrun-patrocinio, noticias/index e programacao/{plenaria-principal, nutricao-esportiva, nb-universitario, medbrasil, corrida-de-rua}.

## O que foi preservado (automações)
- Pixels em todas as páginas: Meta `460696430970324`, GA4 `G-8Y2HLCV0V0`, RD Station (loader) e Clarity `xk8p8zm744`
- `track_checkout()` disparando em todos os CTAs de compra (Hotmart/TicketSports) e na barra de lote
- Barra de virada de lote com countdown real (13/08, 23h59, horário de Brasília) na home e em /ingressos
- Formulário de visitante: mesma API Supabase (`visitante-inscreve`), mesmos campos/validações, isca anti-robô, eventos Lead (fbq) e generate_lead (gtag)
- SEO: titles, descriptions, canonicals, OG/Twitter e JSON-LD (schema.org) copiados das páginas originais
- `vercel.json`, `robots.txt`, `sitemap.xml`, `sitemap-news.xml`, `api/leads.js`, matérias de /noticias, 404, privacidade, proposta, sebrae, crn1: intocados

## Novidades
- `/assets/` com as fotos oficiais usadas nos heros (plenária, expo, homenagem, plateia, parceiros)
- Checkouts reais mantidos: Plenária, Esportiva, NB Universitário, Golden (Hotmart) e NB Run (TicketSports)

## Atenção antes de publicar
1. `vercel.json` redireciona `/programacao/medbrasil` → `/programacao` e `/programacao/corrida-de-rua` → `/programacao/nb-universitario`. As duas páginas novas estão no ZIP; para ativá-las, remova esses 2 redirects.
2. A data da virada de lote (13/08) está nos scripts de countdown de `index.html` e `ingressos.html` — atualizar a cada virada.
3. Deploy: subir o conteúdo desta pasta como raiz do projeto Vercel (cleanUrls já ativo).
