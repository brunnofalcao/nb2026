# Redesign NB 2026 · foco em venda das cidades

Entrega técnica. Arquivos prontos para copiar na raiz do repo `nb2026-oficial` (branch `main`, Vercel `cleanUrls: true`).

## 1. O que foi analisado

- `brief-home-nb2026.md` (spec da nova home).
- Repo `nb2026-oficial (3)`: 14 páginas HTML estáticas, `assets/`, `api/leads.js`, `vercel.json`, sitemaps.
- Home atual: vendia Brasília (3º lote, contador, barra de lote), evento já realizado de 27 a 29 de agosto.

## 2. Diagnóstico

| Item | Estado antes | Ação |
|---|---|---|
| Home | Vendia Brasília, evento encerrado | Reconstruída, foco nas 3 cidades |
| Barra de lote com contador | Prazo 24/08 vencido | Removida da home, virou aviso de "edição encerrada" no arquivo |
| Header | Programação, Ingressos, NB Run, Expo, Seja expositor, Cidades. CTA para `/ingressos` | Refeito: Belém, Goiânia, Porto Alegre, O que é, Notícias. CTA para `/cidades` |
| Footer | Links para páginas de Brasília | Refeito, com link discreto para o arquivo |
| Páginas de cidade | Já construídas e convertendo | **Não alteradas** |
| Páginas de Brasília | Ativas e indexadas | Mantidas no ar, removidas do sitemap e bloqueadas no robots |

**Divergência declarada em relação ao brief:** o item 2 do brief pede header e footer idênticos byte a byte aos das outras 13 páginas. Isso manteria o CTA principal apontando para `/ingressos`, que é a compra de um evento que já aconteceu. A instrução do chat ("ocultar o que já passou") prevalece. O header e o footer novos estão neste pacote e **precisam ser propagados para as outras páginas** antes do deploy, para que a unidade byte a byte seja restabelecida na nova versão.

## 3. Arquivos entregues

| Arquivo | Destino no repo | Status |
|---|---|---|
| `index.html` | `/index.html` | Novo, substitui a home |
| `o-que-e.html` | `/o-que-e.html` → rota `/o-que-e` | Novo |
| `brasilia-2026.html` | `/brasilia-2026.html` → rota `/brasilia-2026` | Novo, cópia da home antiga com `noindex` e aviso de edição encerrada |
| `sitemap.xml` | substitui o atual | Só páginas vivas |
| `robots.txt` | substitui o atual | Bloqueia rotas encerradas, libera agentes de IA |
| `llms.txt` | novo, na raiz | Resumo factual para respostas de IA (AEO/GEO) |
| `assets/*.jpg` | já existem no repo | Sem mudança |

## 4. Estrutura da nova home

1. Hero de decisão, curto, sem imagem pesada.
2. Três cards de cidade, ordem fixa Belém, Goiânia, Porto Alegre. Card inteiro clicável.
3. Faixa de prova: 13 capitais, +10 mil participantes, +100 palestrantes, desde 2014.
4. Brasília como prova social, com 4 fotos e link para `/noticias`.
5. O que é uma imersão, com os 5 itens.
6. Parede de marcas do Cloudinary em `dpr_2.0`, grayscale leve.
7. CTA final com os três botões sobre foto em `opacity: .2`.
8. Barra fixa mobile abaixo de 860px, com scroll suave até o bloco 2.

## 5. SEO, AEO e GEO

- `Event` JSON-LD para as três imersões, com data, local, preço e disponibilidade (`InStock` para Belém, `PreOrder` para Goiânia e Porto Alegre).
- `Organization`, `FAQPage` e `BreadcrumbList` em `/o-que-e`.
- Página `/o-que-e` responde em texto direto às perguntas que uma IA faz ("o que é", "qual a diferença entre congresso e imersão", "quais cidades").
- Bloco "Resumo rápido" e tabela comparativa: formatos legíveis por extração.
- `llms.txt` com os fatos canônicos, para evitar que modelos citem datas de edições encerradas.
- Brasília sai do sitemap e entra no robots como `Disallow`, mas continua no ar via link do rodapé.

## 6. Tracking

| Evento | Onde | Payload |
|---|---|---|
| `select_city` | GA4 e Meta (`SelectCity`) | `city`, `origin` (`card` ou `cta_final`) |
| `click_cta_final` | GA4 | `city` |
| `click_sticky_mobile` | GA4 e Meta (`ClickStickyMobile`) | sem parâmetro |

Sem `localStorage`, sem `sessionStorage`, sem cookie próprio.

## 7. Regra de esgotamento

Quando uma cidade esgotar, no card correspondente:

1. Trocar o selo para `Esgotado` com cor `#8a7aa5`.
2. Aplicar `text-decoration:line-through` no preço.
3. Trocar o `<a>` do card por `<div>` e o botão por um selo estático sem link.
4. No JSON-LD, trocar `availability` para `https://schema.org/SoldOut`.

Nunca remover o card.

## 8. Pendências antes do deploy

1. Propagar o novo header e o novo footer para as 13 páginas restantes.
2. Adicionar `noindex,follow` nas páginas de Brasília: `/ingressos`, `/corrida`, `/expo`, `/visitante`, `/programacao` e subpáginas.
3. Confirmar se `/cidades` deve continuar listando Brasília ou apenas as três cidades vivas.
4. Rodar Lighthouse mobile na home publicada.
5. Confirmar as fotos de Brasília: hoje o pacote usa `assets/*.jpg` do repo. Se a regra do Cloudinary valer também para elas, subir e trocar os `src`.
