# Fontes de Dados

Documentação completa das fontes, variáveis e decisões metodológicas sobre os dados.

**Acesso autorizado pela PMPA/SMF** para fins acadêmicos, com vedação à identificação de contribuintes.

---

## Fontes primárias — PMPA/SMF

| Fonte | Sistema | Formato | Conteúdo | Status |
|---|---|---|---|---|
| Cadastro de Imóveis | SIAT | .xlsx | Atributos cadastrais dos lotes | ✅ Em mãos |
| Dados de Ofertas | SMF | .xlsx | Preços ofertados de terrenos | ✅ Em mãos |
| Guias de ITBI | SMF | .xlsx | Valores de transações de venda efetivadas | ✅ Em mãos |
| Lotes fiscais espacializados | GEO-SMF | Shapefile | Geometria dos lotes com atributos | ✅ Em mãos |
| Bairros | GEO-SMF | Shapefile | Delimitação oficial dos bairros | ✅ Em mãos |
| Eixos de vias | GEO-SMF | Shapefile | Rede viária municipal | ✅ Em mãos |

## Fontes complementares — dados abertos

| Fonte | Formato | Conteúdo | Status |
|---|---|---|---|
| IBGE — Censo 2022 | CSV + Shapefile | Renda, população, domicílios por setor censitário | ✅ Disponível |
| OpenStreetMap (OSMnx) | GeoJSON | Pontos de interesse, equipamentos urbanos | ✅ Disponível |

---

## Escopo e filtros definidos

- **Tipologia:** somente terrenos (sem benfeitorias)
- **Tipo de transação:** somente vendas (excluídos aluguéis)
- **Recorte temporal:** 2022–2023
  - Excluído 2020–2021: distorção por pandemia
  - Excluído 2024 em diante: distorção por enchentes de maio/2024
  - Justificativa: NBR 14653-2:2011 — exigência de homogeneidade temporal da amostra
- **Recorte geográfico:** município de Porto Alegre (possível recorte regional se amostra exigir)

---

## Variáveis do modelo

### Variável dependente
| Variável | Fonte | Transformação |
|---|---|---|
| Preço unitário (R$/m²) | Guias de ITBI + Ofertas | log(R$/m²) — modelo log-log |

### Variáveis independentes — Modelo sem RH (etapa 1)
| Variável | Fonte | Tipo | Transformação |
|---|---|---|---|
| Área do terreno (m²) | SIAT / GEO-SMF | Contínua | log(área) |
| Testada (m) | SIAT / GEO-SMF | Contínua | log(testada) |
| Uso predominante do entorno | SIAT × GEO-SMF | Categórica | Dummy (residencial / comercial / misto) |
| Concentração de unidades no entorno | SIAT agregado por raio | Contínua | — |
| Distância ao centro (m) | GEO-SMF + cálculo espacial | Contínua | log(distância) |
| Distância a eixos viários estruturantes (m) | GEO-SMF | Contínua | log(distância) |
| Renda média do setor censitário | IBGE Censo 2022 | Contínua | log(renda) |

### Variável adicional — Modelo com RH (etapa 3)
| Variável | Origem | Tipo |
|---|---|---|
| Região Homogênea | Resultado do clustering | Dummy por RH |

---

## Lote representativo

Para geração da superfície de valores:
- Definido pela **mediana** dos atributos contínuos dos terrenos da amostra
- Estimado em **grade regular de pontos** sobre o município (resolução a definir)
- Resultado: superfície contínua de R$/m² estimado → base para o clustering

---

## Justificativa do afastamento do Método Comparativo Direto puro (NBR 14653-2)

O MCD (NBR 14653-2, item 8.2) pressupõe homogeneidade prévia da amostra por região. Aplicá-lo exigiria conhecer as RH previamente — criando circularidade metodológica. A abordagem adotada quebra essa circularidade:

1. Modelo OLS sem RH → detecta autocorrelação espacial residual (Índice de Moran I)
2. Autocorrelação confirmada → evidência de que a variável espacial está ausente
3. Superfície de valores estimados → clustering → RH definidas a posteriori
4. RH reintroduzidas no modelo → validação pela redução da autocorrelação residual

---

## Normas aplicáveis

- **ABNT NBR 14653-1:2019** — Procedimentos gerais de avaliação de bens
- **ABNT NBR 14653-2:2011** — Avaliação de imóveis urbanos
