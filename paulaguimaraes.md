# Paula Guimarães Fotografia · Backup e Documento-mãe do projeto

> Arquivo de restauração completo. Se o chat atingir o limite, abra um chat novo
> e leia este arquivo primeiro: ele contém tudo sobre o projeto, o trabalho feito,
> as mídias e como continuar. Fonte única de verdade para futuras atualizações.
>
> Última atualização: 2026-07-01.

---

## 0. Como retomar em um novo chat (leia isto primeiro)

1. O site está **no ar e concluído** em https://paulaguimaraess.com.br (HTTPS ativo).
2. Código-fonte local: `C:/Users/MICRO/Documents/Arrange Corp/WebDesign-Architect/output/2026-06-16-paula-guimaraes/`.
3. É um site estático de arquivo único (`index.html`) + `assets/`. Sem build, sem framework.
4. Deploy: GitHub Pages, repo `github.com/revellaia/paula-guimaraes`, branch `main`, pasta raiz. Push publica sozinho (credencial `revellaia` salva no Git Credential Manager).
5. Para mudar algo: editar `index.html`/trocar imagens, depois `git add -A && git commit -m "..." && git push`.
6. **Regra de ouro:** nunca usar travessão longo (em-dash) em nenhum texto (copy, código, docs). Usar vírgula, ponto ou dois-pontos.
7. Documentos irmãos nesta pasta: `docs/DOMINIO-SETUP.md` (DNS/domínio) e `docs/RELATORIO-AUDITORIA.md` (relatório para Codex/Cline).
8. Memória persistente do assistente: `paula-guimaraes-site.md` (índice em MEMORY.md).

---

## 1. Status e links

| Item | Valor |
|---|---|
| **No ar (oficial)** | https://paulaguimaraess.com.br (HTTPS, Enforce ativo) |
| **www** | https://www.paulaguimaraess.com.br (301 para o apex) |
| **URL antiga** | https://revellaia.github.io/paula-guimaraes/ (redireciona para o domínio) |
| **Repositório** | https://github.com/revellaia/paula-guimaraes |
| **Conta GitHub** | `revellaia` (mesma do Pires Contabilidade e Memoré) |
| **Deploy** | GitHub Pages, branch `main`, pasta raiz `/`, `.nojekyll` |
| **Pasta local** | `output/2026-06-16-paula-guimaraes/` |
| **Stack** | HTML + CSS + JS puro (vanilla), single-file, sem build |

---

## 2. Como atualizar o site (fluxo padrão)

1. Edite `index.html` (e/ou troque imagens em `assets/`).
2. No terminal, dentro da pasta do projeto:
   ```bash
   git add -A
   git commit -m "descricao da mudanca em portugues"
   git push
   ```
3. O GitHub Pages republica sozinho em 1 a 2 minutos.
4. Conferir no ar: https://paulaguimaraess.com.br (pode precisar de hard-refresh por cache).

A credencial do GitHub já está salva no Git Credential Manager (conta `revellaia`), o push autentica sozinho. Para chamadas de API do GitHub, recuperar o token com:
`printf 'protocol=https\nhost=github.com\n\n' | git credential fill`.

---

## 3. Direção de design (Editorial Luxury, chave noturna)

Variante noturna do site Pires Contabilidade. Referências: revista de moda, portfólio boutique, cinema. A protagonista é a Paula e a sua fotografia.

### Paleta (tokens de cor)
| Token | Hex | Uso |
|---|---|---|
| noir | `#14100E` | Fundo principal (preto quente) |
| noir-2 | `#1E1714` | Fundo de seções alternadas (Olhar, Portfólio, Autoridade) |
| noir-3 | `#0C0908` | Fundo de imagens / intro / footer |
| wine | `#7E2233` | Bordô primário (botões, card destaque) |
| wine-deep | `#5A1623` | Fim do gradiente do card destaque |
| wine-soft | `#9B3A48` | Hover de botões |
| gold | `#C9A24B` | Acento metálico (numeração, eyebrows, pontos, anel monograma) |
| gold-soft | `#D9BE86` | Itálicos de destaque nos títulos |
| cream | `#EFE6D8` | Texto principal sobre o escuro |
| cream-2 | `#C9BEAD` | Corpo de texto |
| muted | `#9A8E7D` | Eyebrows, labels, captions |

Usar dourado com parcimônia (fios de 1px, numeração, pontos, contornos). O único acento "precioso" é o dourado mais o vinho.

### Tipografia
- **Títulos:** Cormorant Garamond (400/500/600), itálico nas palavras emocionais em `gold-soft`.
- **Corpo / UI / labels:** Hanken Grotesk (300/400/500/600).
- Import Google Fonts no `<head>`.

### Tratamento das fotos (CSS, em todas)
`filter: saturate(1.05) contrast(1.05) brightness(1.02)` mais overlay vinho sutil mais grão de filme global.

---

## 4. Estrutura da página (long-scroll, 8 seções + footer)

| # | id | Seção | Fundo |
|---|---|---|---|
| Intro | `#pgIntro` | Loop de íris de lente (removido por JS após ~3.5s) | noir-3 |
| Nav | `[data-nav]` | Monograma PG + links + CTA "Agendar ensaio" (abre modal) | transparente para glass no scroll |
| 01 | `#top` | Hero "Há um instante que só acontece uma vez." | noir |
| 02 | `#paula` | A Fotógrafa (retrato + copy essência/legado + sub-bloco "Retrato Autoral") | noir |
| 03 | `#olhar` | O Olhar (manifesto centralizado) | noir-2 |
| 04 | `#ensaios` | Ensaios e Serviços (6 blocos assimétricos) | noir |
| 05 | `#portfolio` | Portfólio: 8 categorias, cada capa abre álbum lightbox com slider | noir-2 |
| 06 | `#experiencia` | A Experiência (jornada 6 etapas) | noir |
| 07 | `#autoridade` | Autoridade (4 colunas) | noir-2 |
| 08 | `#contato` | CTA "Vamos criar juntos" (foto vamos-criar) + WhatsApp | noir |
| Footer | | Monograma + contatos + estúdio + assinatura BrunoDevAI | noir-3 |

---

## 5. Assinatura do hero: intro "íris de lente"

Pedido literal do cliente: *"um loop que abre dentro da imagem para dentro do site"*.

- 100% CSS: `clip-path: circle()` mais `transform: scale()` sincronizados.
  - `pgIris`: `circle(6.5% at 26% 33%)` para `circle(150% at 50% 42%)` (3s).
  - `pgZoom`: `scale(2.45)` para `scale(1)` (zoom saindo de dentro da lente).
- Anel dourado (`.ring`) cresce de `15vmin` a `230vmin` e some.
- Chrome de visor (`.chrome`): cantoneiras nos 4 cantos, "REC" piscando, readout `f/2.8 · 1/250 · ISO 400`, mira de foco central, legenda "enquadrando o instante...".
- Botão "Pular" remove na hora. JS remove tudo após 3500ms.
- **Importante:** a intro TOCA em todos os dispositivos, inclusive com `prefers-reduced-motion: reduce` (pedido explícito do cliente; antes era removida na hora no Windows desktop). Só o "breathing" contínuo das fotos é desligado sob reduced-motion.
- Fundo `#0C0908` por trás do clip é o mesmo near-black da foto, costura perfeita.

---

## 6. Portfólio (Seção 05): galeria por categoria + álbum lightbox

- 8 cards de categoria (`.pg-cat`, `data-album="i"`), capa `01.jpg` + nome + "Ver álbum · N fotos". Cards são HTML estático (bom para SEO).
- Clicar na capa abre um **lightbox** (`#pg-lb`) com: slider da categoria (setas, teclado esquerda/direita, swipe, contador `1 / N`), título, texto da categoria e botão "Agendar este ensaio" (abre o modal com o ensaio pré-selecionado).
- Fechar: X, clique no backdrop, Esc. Restaura o scroll.
- Dados dos álbuns: `<script type="application/json" id="pg-albums">` (nome, service, textos). Contagem por categoria: objeto `albumCounts` no JS. Imagens resolvidas por convenção `assets/portfolio/<slug>/NN.jpg`.

### Categorias e mapa de pastas (fonte: acervo real do cliente)
Origem: `raw06_stitch_premium/Paula Guimarães/Fotos Paula Guimarães/`. Processadas com Pillow: máx 1600px no lado maior, JPEG q82 progressivo.

| Slug | Categoria (site) | Pasta de origem | Fotos |
|---|---|---|---|
| retrato | Retrato Autoral | Posicionamento Profissional | 7 |
| 15anos | Debutante · 15 Anos | 15 Anos | 4 |
| gestante | Gestante | Gestante | 19 |
| newborn | Newborn | New Born | 7 |
| familia | Família | Familia | 4 |
| infantil | Acompanhamento Infantil | Acompanhamento Infantil | 2 |
| casamento | Casamento · Making Off | Making Off Casamento | 4 |
| eventos | Eventos e Celebrações | Eventos de Inauguração | 5 |

### Como adicionar/trocar fotos do portfólio
1. Otimize (máx 1600px, q82 progressivo) e coloque em `assets/portfolio/<slug>/` como `01.jpg, 02.jpg, ...` (a `01.jpg` é a capa).
2. Se a contagem mudar: atualize `albumCounts` no JS e o texto "Ver álbum · N fotos" do card.
3. Nova categoria: adicione um card `.pg-cat` com `data-album="i"` e um objeto no JSON `#pg-albums`, na mesma ordem.
4. Commit + push.

---

## 7. Modal "Agendar ensaio" (lead-capture WhatsApp, sem backend)

- Formulário client-side: Nome, Telefone/WhatsApp, Qual ensaio (select), E-mail (opcional).
- Submit valida no cliente e abre `https://wa.me/5563999719869` com mensagem pronta. Nenhum dado enviado a servidor.
- Abre por qualquer CTA com a classe `.js-open-form`: nav, hero, menu mobile, CTA final e o botão dentro do álbum (que pré-seleciona o ensaio da categoria).
- Padrão herdado do site da Pires Contabilidade, adaptado ao tema noturno.

---

## 8. Mapa de imagens (assets/)

Imagens fixas:
| Arquivo | Onde aparece | Origem |
|---|---|---|
| `paula-camera-web.jpg` | Intro (íris) + Hero | Paula com câmera (handoff) |
| `paula-retrato.jpg` | Seção 02 "A Fotógrafa" | Paula, top branco, mão no queixo (raw `A Fotografa.png`) |
| `cta-vamos-criar.jpg` | Seção 08 (CTA) | Paula sentada, luz da janela em arco (raw `vamos criar juntos.jpg`) |
| `brunodevai-logo.png` | Rodapé (assinatura do desenvolvedor) | Logo BrunoDevAI (dourada) |

Portfólio: `assets/portfolio/<slug>/NN.jpg` (ver seção 6). Total ~12MB, 52 fotos.

Originais pesados (PDF catálogo, `paula-camera.jpg` 11MB, acervo bruto) ficam só no Google Drive / raw, não versionados.

---

## 9. Dados reais da Paula

| Campo | Valor |
|---|---|
| Nome / marca | Paula Guimarães · Paula Guimarães Studio |
| WhatsApp (principal) | (63) 9 9971-9869 = `https://wa.me/5563999719869` |
| WhatsApp (alt.) | (63) 9 8112-9770 |
| Instagram | @paulaguimaraes.fotografias |
| E-mail | paulag.fotografia@gmail.com |
| Estúdio | Rua Rodoviária, 201, Centro, Araguaína, Tocantins (em frente ao Colégio Santa Cruz Junior) |
| Especialidades | Retrato, posicionamento, gestante, newborn, acompanhamento infantil, família, editorial, casamentos, eventos |

Bio oficial: *"A fotografia entrou na minha vida como um presente de Deus. Fotografar é mais do que registrar imagens; é eternizar histórias, sentimentos e momentos únicos da vida. Cada sessão é feita com carinho, sensibilidade e dedicação."*

---

## 10. Domínio paulaguimaraess.com.br (concluído, HTTPS)

- Estratégia: DNS-first (zero downtime). Detalhes em `docs/DOMINIO-SETUP.md`.
- DNS no registro.br: apex A para `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`; www CNAME para `revellaia.github.io`.
- No repo: arquivo `CNAME` com `paulaguimaraess.com.br`. Custom domain configurado via API do GitHub Pages.
- Certificado Let's Encrypt emitido e **Enforce HTTPS ativo**. www faz 301 para o apex.
- Reapontar/reverter (se necessário): editar/remover `CNAME` e usar a API `PUT /repos/revellaia/paula-guimaraes/pages` com `{"cname": "..."}`.

---

## 11. Origem do design (handoff do cliente)

Google Drive (pasta `1nGSU68ewAF35DXJUdpjC1zaeP9DzMzlb`):
- `Paula Guimaraes/design_handoff_paula_home/README.md`: tokens, escala, interações (alta fidelidade).
- `.../Paula Guimarães.dc.html`: protótipo (runtime proprietário `x-dc`). Foi a fonte da verdade, convertido para HTML/CSS/JS standalone.
- `.../assets/`: imagens otimizadas + original.
- `Paula Guimaraes.pdf`: catálogo de marca + portfólio.

Acervo de fotos reais: `raw06_stitch_premium/Paula Guimarães/Fotos Paula Guimarães/` (categorizado por pasta).

---

## 12. Histórico do que foi feito (linha do tempo)

1. Construção inicial: site noturno completo a partir do handoff (intro de lente, 8 seções, footer), publicado no GitHub Pages.
2. Correção da intro: passou a tocar em todos os dispositivos, inclusive com reduced-motion.
3. Seção 02 "A Fotógrafa": novo retrato da Paula + copy sobre essência/legado + sub-bloco "Retrato Autoral".
4. Seção 05 reconstruída: galeria por 8 categorias com álbum em lightbox (slider) e textos do cliente; 52 fotos reais do acervo.
5. Modal "Agendar ensaio" (WhatsApp, sem backend), no padrão do site da Pires; todos os CTAs abrem o modal.
6. Seção 08 (CTA): imagem trocada pela foto "vamos criar juntos" (luz da janela em arco).
7. Rodapé: assinatura "construído e desenvolvido por [logo BrunoDevAI]" com link para brunodevai.com (mesmo padrão, tamanho 22px e local do site da Pires).
8. Domínio `paulaguimaraess.com.br` vinculado com HTTPS.
9. Documentação: `docs/DOMINIO-SETUP.md`, `docs/RELATORIO-AUDITORIA.md` (para Codex/Cline) e este backup.

---

## 13. Pendências / próximas melhorias

- [ ] Confirmar o WhatsApp principal com a Paula (usado: 5563999719869).
- [ ] Pré-Wedding: há texto e opção no formulário, mas ainda sem pasta de fotos para virar álbum.
- [ ] Acompanhamento Infantil tem só 2 fotos no acervo atual (adicionar mais quando houver).
- [ ] Logo oficial da marca Paula (hoje o rodapé/nav usam o monograma tipográfico "PG"). A assinatura BrunoDevAI é separada e já está no rodapé.
- [ ] Opcional (performance): gerar `webp`/`srcset` e thumbnails de capa.

---

## 14. Checklist de qualidade (atendido)

- [x] Domínio próprio com HTTPS (Enforce ativo).
- [x] Mobile-first (breakpoint 980px, hamburger, grids colapsam, hero reordena, lightbox vira coluna única).
- [x] `prefers-reduced-motion` tratado (breathing off; intro toca por decisão do cliente).
- [x] SEO: title, description, Open Graph, Twitter Card, Schema.org LocalBusiness, favicon SVG.
- [x] Scroll reveals, barra de progresso, nav glass no scroll, breathing nas fotos.
- [x] Álbum lightbox (setas/teclado/swipe) + modal WhatsApp validados por Playwright (0 erros de console).
- [x] Assinatura BrunoDevAI no rodapé com link.
- [x] Zero travessões longos (regra de ouro).
- [x] `.nojekyll` + `robots.txt` (Allow: /).
