# Paula Guimarães Fotografia · Documento-mãe do site

> Fonte única de verdade para futuras atualizações. Sempre que mexer no site, leia este arquivo primeiro.

---

## 1. Status e links

| Item | Valor |
|---|---|
| **No ar** | https://revellaia.github.io/paula-guimaraes/ |
| **Repositório** | https://github.com/revellaia/paula-guimaraes |
| **Conta GitHub** | `revellaia` (mesma do Pires Contabilidade e Memoré) |
| **Deploy** | GitHub Pages · branch `main` · pasta raiz `/` |
| **Pasta local** | `output/2026-06-16-paula-guimaraes/` |
| **Stack** | HTML/CSS/JS puro, standalone, sem build (igual ao Pires) |

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
4. Conferir no ar: `https://revellaia.github.io/paula-guimaraes/`

A credencial do GitHub já está salva no Git Credential Manager (conta `revellaia`), o push autentica sozinho.

**Regra de ouro:** nunca usar travessão longo (em-dash) em nenhum texto. Usar vírgula, ponto ou dois-pontos.

---

## 3. Direção de design (Editorial Luxury · chave noturna)

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

Usar dourado com parcimônia (fios de 1px, numeração, pontos, contornos). O único acento "precioso" é o dourado + o vinho.

### Tipografia
- **Títulos:** Cormorant Garamond (400/500/600), itálico nas palavras emocionais em `gold-soft`.
- **Corpo / UI / labels:** Hanken Grotesk (300/400/500/600).
- Import Google Fonts já está no `<head>`.

### Tratamento das fotos (CSS, aplicado em todas)
`filter: saturate(1.05) contrast(1.05) brightness(1.02)` + overlay vinho sutil + grão de filme global.

---

## 4. Estrutura da página (long-scroll, 8 seções + footer)

| # | id | Seção | Fundo |
|---|---|---|---|
| Intro | `#pgIntro` | Loop de íris de lente (removido por JS após ~3.5s) | noir-3 |
| Nav | `[data-nav]` | Monograma PG + links + CTA "Agendar ensaio" | transparente → glass no scroll |
| 01 | `#top` | Hero "Há um instante que só acontece uma vez." | noir |
| 02 | `#paula` | A Fotógrafa | noir |
| 03 | `#olhar` | O Olhar (manifesto centralizado) | noir-2 |
| 04 | `#ensaios` | Ensaios & Serviços (6 blocos assimétricos) | noir |
| 05 | `#portfolio` | Portfólio (galeria por categoria + álbum lightbox) | noir-2 |
| 06 | `#experiencia` | A Experiência (jornada 6 etapas) | noir |
| 07 | `#autoridade` | Autoridade (4 colunas) | noir-2 |
| 08 | `#contato` | CTA WhatsApp | noir |
| Footer | | Monograma + contatos + estúdio | noir-3 |

---

## 5. A assinatura: intro "íris de lente"

Pedido literal do cliente: *"um loop que abre dentro da imagem para dentro do site"*.

- 100% CSS: `clip-path: circle()` + `transform: scale()` sincronizados.
  - `pgIris`: `circle(6.5% at 26% 33%)` → `circle(150% at 50% 42%)` (3s).
  - `pgZoom`: `scale(2.45)` → `scale(1)` (zoom saindo de dentro da lente).
- Anel dourado (`.ring`) cresce de `15vmin` a `230vmin` e some.
- Chrome de visor (`.chrome`): cantoneiras nos 4 cantos, "● REC" piscando, readout `f/2.8 · 1/250 · ISO 400`, mira de foco central, legenda "enquadrando o instante…".
- Botão "Pular" remove na hora. JS remove tudo após 3500ms.
- `prefers-reduced-motion: reduce`: a intro de lente TOCA em todos os dispositivos (pedido explícito do cliente); só o "breathing" contínuo das fotos é desligado.
- Fundo `#0C0908` por trás do clip = mesmo near-black da foto, costura perfeita.

---

## 6. Mapa de imagens (assets/)

Imagens fixas (fora do portfólio):
| Arquivo | Onde aparece | Origem |
|---|---|---|
| `paula-camera-web.jpg` | Intro (íris) + Hero | Paula com câmera (handoff) |
| `paula-retrato.jpg` | Seção 02 "A Fotógrafa" | Paula, top branco, mão no queixo (raw `A Fotografa.png`) |
| `editorial.jpg` | CTA (seção 08) | Retrato editorial (PDF) |

Portfólio (Seção 05), em `assets/portfolio/<slug>/NN.jpg`, extraído do acervo real do cliente (`raw06_stitch_premium/Paula Guimarães/Fotos Paula Guimarães/`) via Pillow (máx 1600px, q82 progressivo):
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
1. Otimize as fotos (máx 1600px, q82 progressivo) e coloque em `assets/portfolio/<slug>/`, nomeadas `01.jpg, 02.jpg, ...` em sequência (a `01.jpg` é a capa).
2. Se a contagem mudar, atualize `albumCounts` no JS e o texto "Ver álbum · N fotos" do card em `index.html`.
3. Textos e nomes dos álbuns ficam no `<script id="pg-albums">` (JSON). Para nova categoria: adicione um card `.pg-cat` com `data-album="i"` e um objeto no JSON, na mesma ordem.
4. Commit + push.

---

## 7. Dados reais da Paula

| Campo | Valor |
|---|---|
| Nome / marca | Paula Guimarães · Paula Guimarães Studio |
| WhatsApp (principal) | (63) 9 9971-9869 → `https://wa.me/5563999719869` |
| WhatsApp (alt.) | (63) 9 8112-9770 |
| Instagram | @paulaguimaraes.fotografias |
| E-mail | paulag.fotografia@gmail.com |
| Estúdio | Rua Rodoviária, 201, Centro, Araguaína · Tocantins (em frente ao Colégio Santa Cruz Junior) |
| Especialidades | Retrato, posicionamento, gestante, newborn, acompanhamento infantil, família, editorial, casamentos |

Bio oficial (do material): *"A fotografia entrou na minha vida como um presente de Deus. Fotografar é mais do que registrar imagens; é eternizar histórias, sentimentos e momentos únicos da vida. Cada sessão é feita com carinho, sensibilidade e dedicação."*

---

## 8. Origem do design (handoff do cliente)

Google Drive (pasta `1nGSU68ewAF35DXJUdpjC1zaeP9DzMzlb`):
- `Paula Guimaraes/design_handoff_paula_home/README.md` · tokens, escala, interações (alta fidelidade).
- `.../Paula Guimarães.dc.html` · protótipo (runtime proprietário `x-dc`). **Foi a fonte da verdade**, convertido para HTML/CSS/JS standalone neste `index.html`.
- `.../assets/` · imagens otimizadas + original.
- `Paula Guimaraes.pdf` · catálogo de marca + portfólio (fotos extraídas).

---

## 9. Pendências / próximas melhorias

- [ ] Confirmar o WhatsApp principal com a Paula (usado: 5563999719869).
- [ ] Receber mais fotos variadas do portfólio (hoje usamos o acervo do catálogo).
- [ ] Logo oficial (hoje é monograma tipográfico "PG"); se houver logotipo, substituir.
- [~] Domínio `paulaguimaraess.com.br`: estratégia DNS-first em andamento (ver `docs/DOMINIO-SETUP.md`).
- [x] Galeria por categoria com álbum lightbox + slider e modal "Agendar ensaio" (feito).
- [ ] Pré-Wedding: há texto e opção no formulário, mas ainda sem pasta de fotos para virar álbum.

---

## 10. Checklist de qualidade (já atendido)

- [x] Mobile-first (breakpoint 980px, hamburger, grids colapsam, hero reordena).
- [x] `prefers-reduced-motion` respeitado (intro + breathing).
- [x] SEO: title, description, Open Graph, Twitter Card, Schema.org LocalBusiness, favicon SVG.
- [x] Scroll reveals, barra de progresso, nav glass no scroll, breathing nas fotos.
- [x] Zero travessões (regra de ouro).
- [x] `.nojekyll` + `robots.txt` (Allow: /).
