# Vínculo do domínio paulaguimaraess.com.br

Estratégia escolhida: **DNS primeiro** (zero downtime). O site segue no ar em
`https://revellaia.github.io/paula-guimaraes/` enquanto o DNS propaga. Só depois
apontamos o domínio (commit do `CNAME` + HTTPS), então o github.io passa a
redirecionar para o domínio.

Domínio confirmado: **paulaguimaraess.com.br** (com dois "s").

---

## Passo 1 · Configurar o DNS no registro.br (feito por você)

Acesse o painel do domínio no registro.br e adicione os registros abaixo.

### Apex / raiz (paulaguimaraess.com.br) · registros A
| Tipo | Nome | Valor |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

### (Opcional, recomendado) IPv6 · registros AAAA
| Tipo | Nome | Valor |
|---|---|---|
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |

### www (www.paulaguimaraess.com.br) · registro CNAME
| Tipo | Nome | Valor |
|---|---|---|
| CNAME | www | revellaia.github.io. |

Observações:
- No registro.br, os registros A do apex ficam no próprio domínio (campo "@" ou raiz da zona).
- O `www` usa CNAME apontando para `revellaia.github.io.` (com o ponto final, se o painel exigir).
- Propagação de DNS costuma levar de alguns minutos a algumas horas.

---

## Passo 2 · Apontar o domínio no GitHub (feito por mim, quando o DNS estiver pronto)

Quando os registros acima estiverem propagando, me avise. Eu executo:

1. Crio o arquivo `CNAME` na raiz do repositório com o conteúdo:
   ```
   paulaguimaraess.com.br
   ```
2. Configuro o custom domain via API do GitHub Pages e habilito **Enforce HTTPS**
   (certificado Let's Encrypt automático do GitHub).
3. Valido `https://paulaguimaraess.com.br` e o redirecionamento do www.

Como verificar a propagação antes de apontar:
```
nslookup paulaguimaraess.com.br
# deve retornar os IPs 185.199.108-111.153
```

---

## Estado atual (CONCLUÍDO em 2026-07-01)

- [x] Estratégia definida: DNS-first (sem downtime).
- [x] Registros de DNS documentados (este arquivo).
- [x] Registros aplicados no registro.br (apex A 185.199.108-111.153; www CNAME revellaia.github.io) e propagados.
- [x] `CNAME` criado (`paulaguimaraess.com.br`) + custom domain configurado no GitHub Pages.
- [x] Certificado Let's Encrypt emitido e **Enforce HTTPS habilitado**.

Site no ar em **https://paulaguimaraess.com.br** (www redireciona para o apex).
O `revellaia.github.io/paula-guimaraes` passa a redirecionar para o domínio.
