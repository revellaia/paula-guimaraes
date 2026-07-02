# Relatório de Auditoria: Site Paula Guimarães Fotografia

Documento para revisão independente por **Codex** e **Cline**. Descreve o que
foi construído, decisões técnicas, testes realizados e pendências, com pontos de
verificação objetivos.

- Data do relatório: 2026-07-01
- Autor da implementação: Claude (Opus 4.8), sessão WebDesign-Architect
- Produto: site institucional/portfólio da fotógrafa Paula Guimarães (Araguaína-TO)

---

## 1. Visão geral

| Item | Valor |
|---|---|
| No ar | https://revellaia.github.io/paula-guimaraes/ |
| Repositório | https://github.com/revellaia/paula-guimaraes (branch `main`) |
| Deploy | GitHub Pages, raiz `/`, `.nojekyll` presente |
| Stack | HTML + CSS + JS puro (vanilla), single-file `index.html`, sem build, sem framework |
| Tamanho | `index.html` 909 linhas (~80KB); assets de portfólio ~12MB (52 fotos) |
| Idioma | pt-BR |
| Origem do design | handoff de alta fidelidade do cliente (Google Drive), convertido de runtime proprietário `x-dc` para HTML standalone |

Direção visual: Editorial Luxury em chave noturna. Paleta preto quente `#14100E`
+ vinho `#7E2233` + dourado `#C9A24B`. Fontes Cormorant Garamond (títulos) e
Hanken Grotesk (corpo).

---

## 2. Estrutura de arquivos

```
2026-06-16-paula-guimaraes/
  index.html                     (site completo, single-file)
  .nojekyll                      (desliga Jekyll no GitHub Pages)
  robots.txt                     (Allow: /)
  paulaguimaraes.md              (documento-mae para manutencao futura)
  docs/
    DOMINIO-SETUP.md             (guia de DNS do dominio)
    RELATORIO-AUDITORIA.md       (este arquivo)
  assets/
    paula-camera-web.jpg         (intro/hero)
    paula-retrato.jpg            (secao 02 A Fotografa)
    editorial.jpg                (CTA secao 08)
    portfolio/
      retrato/01..07.jpg
      15anos/01..04.jpg
      gestante/01..19.jpg
      newborn/01..07.jpg
      familia/01..04.jpg
      infantil/01..02.jpg
      casamento/01..04.jpg
      eventos/01..05.jpg
```

---

## 3. Seções do site

1. Intro (loop de íris de lente): overlay `#pgIntro`, animação CSS `clip-path: circle()` + `transform: scale()` sincronizadas, chrome de visor (REC piscando, `f/2.8 · 1/250 · ISO 400`, mira de foco, cantoneiras), botão "Pular", remoção via JS após 3500ms.
2. `#top` Hero.
3. `#paula` A Fotógrafa: retrato da Paula + copy sobre essência/legado + sub-bloco "Retrato Autoral".
4. `#olhar` O Olhar (manifesto).
5. `#ensaios` Ensaios e Serviços (6 blocos).
6. `#portfolio` Portfólio: galeria por categoria com álbum lightbox (detalhado na seção 4 deste relatório).
7. `#experiencia` A Experiência (jornada 6 etapas).
8. `#autoridade` Autoridade.
9. `#contato` CTA final.
10. Footer.

---

## 4. Foco da última entrega (para auditoria detalhada)

### 4.1 Seção 05 reconstruída como galeria por categoria
- 8 categorias reais, cada uma um card (capa `01.jpg` + nome + "Ver álbum · N fotos").
- Fotos extraídas do acervo do cliente (`raw06_stitch_premium/Paula Guimarães/Fotos Paula Guimarães/`), redimensionadas para no máximo 1600px no lado maior, JPEG q82 progressivo, via PyMuPDF/Pillow.
- Mapeamento pasta -> categoria:
  - Posicionamento Profissional -> Retrato Autoral (7)
  - 15 Anos -> Debutante · 15 Anos (4)
  - Gestante -> Gestante (19)
  - New Born -> Newborn (7)
  - Familia -> Família (4)
  - Acompanhamento Infantil -> Acompanhamento Infantil (2)
  - Making Off Casamento -> Casamento · Making Off (4)
  - Eventos de Inauguração -> Eventos e Celebrações (5)
- Cards são HTML estático (`<button class="pg-cat" data-album="i">`), bom para SEO e funcionam sem depender de JSON.

### 4.2 Álbum em lightbox (slider)
- Um único overlay `#pg-lb` reutilizado.
- Ao clicar em uma capa: abre o álbum com a 1ª foto, título, texto da categoria e contador `1 / N`.
- Navegação: setas prev/next, teclado (setas esquerda/direita), swipe em touch, contador.
- Fechar: botão X, clique no backdrop, tecla Esc. Após fechar, navegação normal (scroll do body é restaurado).
- Dados dos álbuns em `<script type="application/json" id="pg-albums">` (nome, service, textos). Contagem por categoria em `albumCounts` no JS. As imagens são resolvidas por convenção `assets/portfolio/<slug>/NN.jpg`.
- Textos das categorias: fornecidos pelo cliente, aplicados literalmente (Debutante, Gestante, Newborn, Família, Acompanhamento Infantil, Casamento, Eventos). Pré-Wedding foi enviado no texto mas NÃO tem pasta de fotos, então não virou álbum (registrado como pendência). Existe como opção de agendamento no formulário.

### 4.3 Modal "Agendar ensaio" (paridade com o site da Pires)
- Formulário lead-capture client-side: Nome, Telefone/WhatsApp, Qual ensaio (select), E-mail (opcional).
- No submit: valida no cliente e abre `https://wa.me/5563999719869` com mensagem pré-preenchida. Sem backend, nenhum dado enviado a servidor.
- Disparado por todos os CTAs principais (`.js-open-form`): nav, hero, menu mobile, CTA final e o botão "Agendar este ensaio" dentro do álbum (que pré-seleciona o ensaio da categoria).
- Abrir/fechar via clique, backdrop, Esc; foco vai para o primeiro campo ao abrir.

---

## 5. Segurança e privacidade

- Site 100% estático. Sem backend, sem banco, sem variáveis de ambiente, sem segredos versionados.
- Formulário: validação apenas client-side (por design). Comentário no código deixa explícito que, se um backend for adicionado, é obrigatório revalidar e sanitizar no servidor.
- Nenhum dado do formulário é persistido nem enviado a terceiros; o fluxo abre o WhatsApp com o texto montado localmente (`encodeURIComponent`).
- Sem cookies, sem analytics, sem trackers de terceiros nesta versão.
- Links externos usam `rel="noopener"`.
- Ponto de atenção LGPD: ao acionar o WhatsApp, o usuário compartilha nome/telefone com a fotógrafa por vontade própria; não há coleta silenciosa. Recomenda-se, futuramente, uma nota curta de privacidade se analytics for adicionado.

---

## 6. Acessibilidade

- Modais e álbum com `role="dialog"`, `aria-modal="true"`, `aria-label`.
- Navegação por teclado: Esc fecha álbum e modal; setas trocam foto; CTAs com `role="button"` respondem a Enter/Espaço.
- Campos do formulário com labels associados e `aria-required`; erros marcam `aria-invalid`.
- `prefers-reduced-motion`: o "breathing" contínuo das fotos é desligado. NOTA DE PRODUTO: por pedido explícito do cliente, a intro de lente toca em todos os dispositivos, inclusive com reduced-motion (decisão conhecida, não é bug). O botão "Pular" e a remoção automática (3.5s) mitigam.
- Foco visível preservado; imagens com `alt`.

---

## 7. Performance

- Imagens de portfólio otimizadas (máx 1600px, q82 progressivo). Total ~12MB para 52 fotos.
- Capas das categorias e imagens de álbum com `loading="lazy"`; as fotos internas do álbum só são baixadas quando o usuário navega.
- Sem dependências JS externas; apenas Google Fonts via `<link>` com preconnect.
- Sugestão de melhoria (não bloqueante): gerar variantes `webp`/`srcset` e thumbnails dedicados para as capas, reduzindo ainda mais o peso inicial.

---

## 8. Testes realizados (Playwright, Chromium)

- Intro com `prefers-reduced-motion: reduce` no desktop: confirmado que a intro agora TOCA (antes era removida na hora). Removida após ~3.5s. Sem regressão no mobile e no modo normal.
- Seção 05: grid renderiza as 8 categorias; abrir álbum (Gestante) mostra `total=19`, `idx=1`, `src=assets/portfolio/gestante/01.jpg`; "próxima" avança para `02.jpg`/`idx=2`.
- Modal: o CTA do álbum abre o formulário com o ensaio pré-selecionado (`Gestante`) e fecha o álbum.
- Console: 0 erros de JS. Verificado em 1440x900 e viewport mobile.
- Regra editorial: 0 travessões longos (em-dash) em todo o projeto (código e docs).

Como reproduzir (o auditor pode rodar):
```
# a partir de um projeto com playwright instalado
node -e "const {chromium}=require('playwright');(async()=>{const b=await chromium.launch();const p=await (await b.newContext({reducedMotion:'reduce'})).newPage();await p.goto('https://revellaia.github.io/paula-guimaraes/');await p.waitForTimeout(4200);await p.evaluate(()=>document.querySelector('.pg-cat[data-album=\"2\"]').click());await p.waitForTimeout(600);console.log(await p.evaluate(()=>document.getElementById('pg-lb-total').textContent));await b.close();})()"
```

---

## 9. Domínio paulaguimaraess.com.br (em andamento)

- Estratégia escolhida com o cliente: DNS primeiro (zero downtime). Detalhes e registros em `docs/DOMINIO-SETUP.md`.
- Estado: registros de DNS documentados; aguardando o cliente aplicar no registro.br. O `CNAME` e o Enforce HTTPS serão configurados após a propagação.
- Enquanto isso, o site permanece no ar via github.io.

---

## 10. Pendências conhecidas

- [ ] Pré-Wedding: existe texto e opção de agendamento, mas não há pasta de fotos; criar álbum quando o acervo chegar.
- [ ] Acompanhamento Infantil tem apenas 2 fotos no acervo atual.
- [ ] Confirmar número de WhatsApp principal (usado: 5563999719869).
- [ ] Logo oficial (hoje é monograma tipográfico "PG").
- [ ] Domínio: aplicar DNS + apontar (ver seção 9).
- [ ] Opcional: webp/srcset e thumbnails de capa para performance.

---

## 11. Checklist objetivo para o auditor

- [ ] `index.html` abre e renderiza sem erro de console.
- [ ] As 8 capas de categoria aparecem na Seção 05 e cada uma abre o álbum correto.
- [ ] Slider: setas, teclado e swipe trocam a foto; contador coerente; wrap-around funciona.
- [ ] Fechar álbum (X, backdrop, Esc) restaura o scroll e a navegação.
- [ ] Modal "Agendar ensaio" valida campos obrigatórios e monta o link `wa.me` correto.
- [ ] CTA dentro do álbum pré-seleciona o ensaio no formulário.
- [ ] Intro toca em desktop e mobile, inclusive com reduced-motion, e some sozinha.
- [ ] Nenhum segredo/credencial versionado; nenhum backend.
- [ ] Zero travessões longos (em-dash) no conteúdo.
- [ ] Responsivo: 4 colunas (desktop) -> 2 (tablet/mobile); lightbox vira coluna única no mobile.
