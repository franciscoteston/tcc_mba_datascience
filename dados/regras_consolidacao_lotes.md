# Regras de Consolidação da Camada Base Territorial — LOTES

**Documento:** regras_consolidacao_lotes.md  
**Versão:** 1.0  
**Data:** junho/2025  
**Autor:** Francisco Teston  
**Status:** Ativo — atualizar sempre que houver revisão de critérios

---

## Origem dos dados

Três camadas shapefile fornecidas pela SMF/PMPA (GEO-SMF), referência fevereiro/2024:

| Camada | Arquivo | Registros brutos | Descrição |
|---|---|---|---|
| LF | lf.shp | 42.016 | Lotes fiscais |
| BF | bf.shp | 155.705 | Blocos fiscais (unidades individuais) |
| GL | gl.shp | 468 | Glebas (áreas não parceladas ou em parcelamento) |

**CRS original:** TM-POA (projeção local de Porto Alegre), baseada em SIRGAS 2000.  
**CRS de trabalho:** a converter para SIRGAS 2000 / UTM Zona 22S (EPSG:31982) antes das análises espaciais.

---

## Estrutura do campo NUMBLOCO

Campo-chave de ligação entre as camadas geoespaciais (GEO-SMF) e o cadastro alfanumérico (SIAT, ITBI, Ofertas).

**Formato:** 12 caracteres numéricos — `XXXXXXXXYYYY`

| Posições | Conteúdo | Exemplo |
|---|---|---|
| 1–8 | Identificador do lote fiscal | `00163685` |
| 9–12 | Identificador do sublote | `0000` (sem subdivisão) ou `0001`, `0002`... |

**Casos especiais:**

| Valor | Significado |
|---|---|
| `XXXXXXXX0000` | Lote fiscal sem subdivisão — caso majoritário |
| `XXXXXXXX0001`, `0002`... | Sublotes de parcelamento regular ou irregular |
| `000000000000` | Polígono sem cadastro vinculado — geometria existe, cadastro pendente |

---

## Relação entre as camadas

- LF, BF e GL são camadas **complementares**, não hierárquicas.
- Sobreposições detectadas entre LF e BF são predominantemente **imprecisões de borda** de digitalização (área de overlap < 5 m²), confirmadas por verificação visual no QGIS.
- Casos de sobreposição real entre NUMBLOCO cadastrados (≠ 000000000000) foram verificados visualmente e representam **unidades territoriais independentes válidas** — podem refletir erros cadastrais entre matrículas, mas não invalidam nenhuma das unidades.
- GL representa glebas brutas; LF e BF dentro de GL são lotes já individualizados — ambos mantidos na consolidação.

---

## Regras de consolidação

### Etapa 1 — União simples

LOTES_RAW = LF + BF + GL


Todas as camadas unidas sem distinção de hierarquia.

### Etapa 2 — Remoção de registros inválidos

Aplicadas em sequência, com contagem de registros removidos por regra:

| # | Regra | Critério | Justificativa |
|---|---|---|---|
| ① | Geometry nula | `geometry IS NULL` | Sem representação espacial — inutilizável para análise |
| ② | Área zero | `AREA = 0` | Registro sem dimensão real |
| ③ | Registro fantasma | `SETOR = 0 AND QUARTEIRAO = 0` | Atributos cadastrais ausentes — registro corrompido ou de teste |
| ④ | NUMBLOCO duplicado | Mesmo NUMBLOCO + mesma AREA, exceto `000000000000` | Duplicata de importação — manter apenas primeira ocorrência |

### Etapa 3 — Campos derivados

Calculados após a limpeza:

| Campo | Cálculo | Descrição |
|---|---|---|
| `LOTE_PAI` | `NUMBLOCO[0:8]` | Identificador do lote pai |
| `SUBLOTE` | `NUMBLOCO[8:12]` | Identificador do sublote |
| `TEM_CADASTRO` | `NUMBLOCO ≠ 000000000000` | Flag de cadastro vinculado |
| `AREA_GEO` | `geometry.area` | Área calculada da geometria em m² |
| `CAMADA_ORIG` | "LF", "BF" ou "GL" | Camada de origem do registro |

### Etapa 4 — O que manter

| Situação | Ação |
|---|---|
| NUMBLOCO cadastrado (≠ 000000000000) | **Sempre manter** |
| NUMBLOCO = 000000000000 sem sobreposição | **Manter** — indica ocupação territorial mesmo sem cadastro |
| NUMBLOCO = 000000000000 sobreposto a cadastrado | **Manter ambos** — unidades territoriais independentes |
| Dois NUMBLOCO cadastrados sobrepostos | **Manter ambos** — verificados visualmente como unidades válidas |
| Sobreposição de borda (≤ 5 m²) | **Ignorar** — imprecisão de digitalização |

---

## Decisões registradas

| Data | Decisão | Justificativa |
|---|---|---|
| jun/2025 | Manter todos os registros com NUMBLOCO cadastrado, mesmo sobrepostos | Verificação visual confirmou que sobreposições reais entre cadastrados representam unidades territoriais válidas e independentes |
| jun/2025 | Manter registros com NUMBLOCO = 000000000000 | Indicam existência de imóvel no território, mesmo sem cadastro — relevante para delimitação das RH |
| jun/2025 | Limiar de borda: 5 m² | Verificação visual (QGIS) confirmou que overlaps ≤ 5 m² são imprecisões de digitalização entre lotes contíguos |
| jun/2025 | Camadas LF, BF e GL tratadas como complementares (não hierárquicas) | Análise de sobreposição confirmou que não há contenção sistemática entre camadas — são unidades do mesmo nível territorial |
| jun/2025 | Hierarquia de qualidade geométrica: BF > LF = GL | Confirmado pela PMPA/SMF: BF têm geometria ajustada por aerolevantamento digital; LF e GL mantêm geometria pré-aero com possíveis divergências territoriais. Sobreposições entre camadas são artefatos do período de transição entre ajustes. Para cálculos espaciais (centroide, área geométrica, distâncias), preferir geometria BF quando disponível. |

---

## Resultado da consolidação

| Métrica | Valor |
|---|---|
| Total bruto (LF + BF + GL) | 198.189 registros |
| ① Geometry nula removidos | 4 |
| ② AREA = 0 removidos | 45 |
| ③ Registros fantasma removidos | 12 |
| ④ NUMBLOCO duplicados removidos | 9 |
| **Total removido** | **63 registros (0,03%)** |
| **Camada LOTES consolidada** | **198.126 registros** |
| Com cadastro (NUMBLOCO ≠ 000000000000) | 172.153 |
| Sem cadastro (NUMBLOCO = 000000000000) | 25.973 |
| Sublotes (SUBLOTE ≠ 0000) | 7.086 |
| Origem BF | 155.693 |
| Origem LF | 42.010 |
| Origem GL | 423 |
| Arquivo de saída | `lotes_consolidados.gpkg` (83,1 MB) |
| Notebook de geração | `02_consolida_lotes.ipynb` |
| Data de geração | junho/2025 |

---

## Pendências

- [x] Apurar contagem final após execução do `02_consolida_lotes.ipynb` — **concluído: 198.126 registros**
- [ ] Converter CRS para EPSG:31982 antes das análises espaciais
- [ ] Confirmar chave de ligação NUMBLOCO com SIAT e ITBI
- [ ] Verificar shapefiles adicionais (bairros, logradouros, eixos de vias)
- [ ] Para cálculos espaciais: implementar lógica de preferência BF sobre LF/GL quando o mesmo lote existir nas duas camadas (identificar pelo NUMBLOCO/LOTE_PAI). Implementar no notebook de pré-processamento.
