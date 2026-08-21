# CONTEXTO DO PROJETO — TCC MBA Data Science e Analytics · USP/ESALQ
**Arquivo:** CONTEXTO_SESSAO.md  
**Função:** Retomada de contexto entre sessões (Claude.ai, Claude Desktop ou qualquer ambiente)  
**Última atualização:** agosto/2025  
**Repositório:** https://github.com/franciscoteston/tcc_mba_datascience

---

## 1. IDENTIFICAÇÃO

| Campo | Valor |
|---|---|
| Aluno | Francisco Teston |
| Curso | MBA em Data Science e Analytics — USP/ESALQ |
| Tema | Regiões Homogêneas de Valorização Imobiliária em Porto Alegre: atualização metodológica com clustering geoespacial e regressão hedônica (terrenos) |
| Estudo base | Lapolli et al. (1994) — SMF/PMPA |
| Delineamento | Estudo de Caso Único Instrumental |
| Limite | 30 páginas (normas USP/ESALQ) |

---

## 2. OBJETIVO DA PESQUISA

Atualizar a delimitação de Regiões Homogêneas de Valorização Imobiliária de terrenos em Porto Alegre por meio de clustering geoespacial aplicado sobre superfície de valores estimados por modelo hedônico, utilizando dados oficiais da PMPA/SMF, e comparar os resultados com Lapolli et al. (1994).

---

## 3. CLASSIFICAÇÃO METODOLÓGICA

| Dimensão | Classificação |
|---|---|
| Delineamento | Estudo de Caso Único — Instrumental |
| Natureza | Quantitativa |
| Objetivo | Explicativo com elementos descritivos |
| Tipologia | Somente terrenos · somente transações de venda |
| Recorte temporal | 2022–2023 (excluindo pandemia 2020–2021 e pós-enchente 2024) |
| Dados | Oficiais PMPA/SMF — acesso formalmente autorizado |
| Linguagem | Python |
| Normas | NBR 14653-1:2019 e NBR 14653-2:2011 |

---

## 4. FLUXO ANALÍTICO

```
Dados brutos (SIAT + ITBI + Ofertas — PMPA/SMF)
        ↓
Filtragem: terrenos · somente vendas · 2022–2023
        ↓
Pré-processamento: outliers (IQR) · geocodificação · join GEO-SMF
        ↓
[1] Modelo OLS log-log SEM variável RH
        ↓
Índice de Moran I nos resíduos
(confirma autocorrelação espacial — justifica necessidade das RH
e afastamento do MCD puro — NBR 14653-2, item 8.2)
        ↓
Lote representativo (mediana dos atributos)
        ↓
Estimativa em grade de pontos → superfície contínua de R$/m²
        ↓
[2] Clustering sobre a superfície
(K-Means · DBSCAN · comparação entre algoritmos)
        ↓
RH definidas → reintroduzidas como dummy no modelo
        ↓
[3] Modelo OLS log-log COM variável RH
        ↓
Índice de Moran I nos resíduos finais (validação)
R² comparado com Lapolli et al. (1994): R²=0,94
```

---

## 5. FONTES DE DADOS

### 5.1 Dados geoespaciais — GEO-SMF (em mãos)
**Pasta:** `C:\Users\franc\OneDrive\Francisco\Profissional\MBA_Data_Science_ e_Analytics\00_TCC\06_Dados_base\GEO\`

| Arquivo | Pasta | Formato | Conteúdo | Status |
|---|---|---|---|---|
| lf.shp | 2024_02_basegeo | Shapefile | Lotes fiscais (42.016) | ✅ Explorado |
| bf.shp | 2024_02_basegeo | Shapefile | Blocos fiscais (155.705) | ✅ Explorado |
| gl.shp | 2024_02_basegeo | Shapefile | Glebas (468) | ✅ Explorado |
| lotes_consolidados.gpkg | 2024_02_basegeo | GeoPackage | LOTES consolidados (198.126) | ✅ Gerado |
| eixos.shp | eixos | Shapefile | Eixos viários (31.113 trechos) | ✅ Explorado |
| limite_poa_saa_tm-sirgas.shp | limite_poa | Shapefile | Limite municipal (18 polígonos) | ✅ Explorado |
| poa_ilhas_tm-sirgas.shp | poa_ilhas | Shapefile | Ilhas (17 polígonos) | ✅ Explorado |
| bairros_vigentes_ago17_tm-sirgas.shp | bairros_vigentes | Shapefile | Bairros (122) | ✅ Explorado |

**CRS de todos os shapefiles:** TM-POA (projeção local POA, base SIRGAS 2000)  
**CRS de trabalho:** a converter para EPSG:31982 (SIRGAS 2000 UTM Zona 22S)

### 5.2 Dados cadastrais — SIAT (em mãos)
**Pasta:** `C:\Users\franc\OneDrive\Francisco\Profissional\MBA_Data_Science_ e_Analytics\00_TCC\06_Dados_base\GEO\2023_11_03_SIAT\`  
**Referência:** novembro/2023  
**Formato:** `.txt` separado por `|`, encoding `latin1`, decimal `,`  
**Schema:** documentado em `IMO_GRD.xlsx` (pasta GEO/)

| Arquivo | Tamanho | Colunas | Status |
|---|---|---|---|
| IMO_VW_LOTE_FISCAL_20231103.txt | 30,2 MB | 22 | ⏳ Pendente leitura completa |
| IMO_VW_UNIDADE_IMOBILIARIA_20231103.txt | 225,7 MB | 44 | ⏳ Pendente |
| IMO_VW_CONSTRUCAO_20231103.txt | 91,6 MB | 14 | ⏳ Pendente |
| IMO_VW_TESTADA_20231103.txt | 8,7 MB | 11 | ⏳ Pendente |
| IMO_VW_REGISTRO_IMOVEL_20231103.txt | 71,2 MB | 13 | ⏳ Pendente |
| IMO_VW_IPTU_TCL_20231103_0.txt | 140,8 MB | 24 | ⏳ Pendente |
| IMO_CLASSES_E_RH.xlsx | 0,3 MB | 6 | ⏳ Somente comparação futura |

### 5.3 Dados de mercado (em mãos — a explorar)
- **ITBI:** transações de venda — formato e localização a confirmar
- **Ofertas:** preços ofertados — formato e localização a confirmar

---

## 6. VARIÁVEIS DO MODELO

### Variável dependente
| Variável | Fonte | Transformação |
|---|---|---|
| Preço unitário (R$/m²) | ITBI + Ofertas | log(R$/m²) |

### Variáveis independentes — Modelo sem RH
| Variável | Fonte SIAT | Campo |
|---|---|---|
| Área do terreno (m²) | IMO_VW_LOTE_FISCAL | MTR_AREA_REAL |
| Testada principal (m) | IMO_VW_TESTADA | MTR_TESTADA onde FLG_PRINCIPAL='S' |
| Número de frentes | IMO_VW_TESTADA | count(NUM_SEQUENCIA) por IDF_LOTE |
| Uso do entorno | IMO_VW_UNIDADE_IMOBILIARIA | COD_USO / DES_USO |
| Finalidade | IMO_VW_UNIDADE_IMOBILIARIA | DES_FINALIDADE |
| Posição na quadra | IMO_VW_LOTE_FISCAL | DES_POSICAO_NA_QUADRA |
| Categoria da via principal | IMO_VW_TESTADA × eixos.shp | COD_LOGRADOURO → CDIDECAT |
| Distância ao centro (m) | GEO-SMF + cálculo espacial | centroide → ponto referência |
| Renda do setor censitário | IBGE Censo 2022 | — |

### Variável adicional — Modelo com RH
| Variável | Origem | Tipo |
|---|---|---|
| Região Homogênea | Resultado do clustering | Dummy por RH |

### Variáveis apenas para comparação (não entram no modelo)
- `IDF_REG_REGIAO_HOMOGENEA` + `DES_REGIAO` + `VLR_REGIAO_CLASSE` → RH atual da PMPA

---

## 7. DECISÕES TOMADAS

### Escopo
- ✅ Somente terrenos (sem benfeitorias)
- ✅ Somente transações de venda (excluídos aluguéis)
- ✅ Período 2022–2023 (excluindo pandemia e pós-enchente)
- ✅ Ilhas excluídas da análise, mantidas na visualização do mapa
- ✅ Abrangência municipal (possível recorte regional se amostra exigir)

### Metodologia
- ✅ Delineamento: Estudo de Caso Único Instrumental
- ✅ Afastamento do MCD puro justificado por circularidade metodológica + Moran I
- ✅ Testar K-Means e DBSCAN — comparar qual melhor captura estrutura de POA
- ✅ Eixos viários como sinalizadores de limites entre RH (não barreiras rígidas)

### Dados geoespaciais
- ✅ LOTES = LF + BF + GL (união simples, 198.126 registros após limpeza)
- ✅ Hierarquia de qualidade geométrica: BF > LF = GL (BF ajustado por aero)
- ✅ Sobreposições entre camadas são artefatos de período de transição — manter ambos
- ✅ Limiar de borda: 5 m² (overlaps menores = imprecisão de digitalização)
- ✅ CRS de trabalho: EPSG:31982 (a converter)

### SIAT
- ✅ Separador `|`, encoding `latin1`, decimal `,`
- ✅ Chave SIAT↔GEO: `NUM_BLOCO.astype(str).str.zfill(12)` = `NUMBLOCO`
- ✅ Coordenadas (COORD_X/Y) derivadas do centroide da geometria (não existem no SIAT)
- ✅ Área construída = somatório de MTR_AREA_CONSTRUIDA_TOTAL por NUM_INSCRICAO
- ✅ RH atual (IDF_REG_REGIAO_HOMOGENEA) somente para comparação final

### Sistema e ferramentas
- ✅ Linguagem: Python
- ✅ Repositório: GitHub (franciscoteston/tcc_mba_datascience)
- ✅ Ambiente: Cursor (execução local)
- ✅ Deploy futuro: Railway (aplicação web SIGRH)

---

## 8. PENDÊNCIAS — EM ORDEM DE EXECUÇÃO

### Fase atual: Exploração e preparação dos dados

**P-01 — Leitura completa dos TXT do SIAT**  
Próximo notebook: `07_explora_lote_fiscal.ipynb`  
Sequência: LOTE_FISCAL → UNIDADE_IMOBILIARIA → TESTADA → IPTU_TCL → REGISTRO_IMOVEL  
Ler apenas colunas definidas no AUX_IMO + complementares identificadas

**P-02 — Exploração dos dados de ITBI e Ofertas**  
Localização e formato ainda não verificados  
Confirmar volume amostral por período (2022–2023)

**P-03 — Conversão de CRS**  
Reprojetar `lotes_consolidados.gpkg` de TM-POA para EPSG:31982  
Aplicar a todos os shapefiles antes das análises espaciais

**P-04 — Tratamento de condomínios vs. lotes isolados**  
Verificar `DES_TIPO_CATEGORIA` e `DES_RATEIO` no LOTE_FISCAL  
Decidir se condomínios entram no modelo ou são tratados separadamente

**P-05 — Decisão sobre área construída**  
Definir se usar apenas área total ou detalhar por sequência construtiva  
Relevante para identificar terrenos sem construção (IND_CONSTRUCAO_NAO_CONSTITUIDA)

**P-06 — Hierarquia viária oficial**  
Pesquisar se PMPA/IPPOA tem categorização hierárquica das vias (PDDUA)  
Verificar influência estatística de CDIDECAT no modelo

**P-07 — Geometria preferencial BF para cálculos espaciais**  
Implementar lógica: quando mesmo NUMBLOCO existir em BF e LF, usar geometria BF  
Implementar no notebook de pré-processamento

**P-08 — Dados IBGE Censo 2022**  
Baixar malha de setores censitários e dados de renda  
Join espacial com camada LOTES

### Fase seguinte: Pipeline analítico

**P-09 — Modelo OLS sem RH**  
**P-10 — Índice de Moran I nos resíduos**  
**P-11 — Superfície de valores estimados**  
**P-12 — Clustering (K-Means e DBSCAN)**  
**P-13 — Modelo OLS com RH**  
**P-14 — Validação final (Moran I)**  

### Fase futura: Sistema web (SIGRH)

**P-15 — Interface Streamlit + FastAPI**  
**P-16 — Deploy no Railway**  
**P-17 — Gestão de dados por usuário**  

---

## 9. ESTRUTURA DO REPOSITÓRIO

```
tcc_mba_datascience/
├── README.md                              ← visão geral do projeto
├── CONTEXTO_SESSAO.md                     ← este arquivo
├── .gitignore
├── requirements.txt
├── painel/
│   └── index.html                         ← painel interativo de acompanhamento
├── dados/
│   ├── README.md                          ← fontes e variáveis
│   ├── regras_consolidacao_lotes.md       ← regras LF+BF+GL → LOTES
│   └── decisoes_pendentes.md             ← registro de todas as decisões
├── notebooks/
│   ├── 01_explora_shapefiles_v1-v6.ipynb ← exploração LF, BF, GL
│   ├── 02_consolida_lotes.ipynb          ← geração de lotes_consolidados.gpkg
│   ├── 03_explora_eixos.ipynb            ← exploração eixos viários
│   ├── 03_explora_eixos_v2.ipynb         ← categorias e extensão por via
│   ├── 04_explora_shapes_aux.ipynb       ← limite_poa, ilhas, bairros
│   ├── 05_explora_siat.ipynb             ← schema IMO_GRD.xlsx
│   └── 06_explora_siat_txt.ipynb         ← estrutura dos TXT do SIAT
├── outputs/
│   ├── mapas/
│   └── tabelas/
└── referencias/
    └── lapolli_1994_resumo.md            ← síntese do estudo de 1994
```

---

## 10. COMMITS RECENTES

| Hash | Mensagem |
|---|---|
| b008325 | docs: hierarquia qualidade geometrica BF>LF=GL confirmada pela PMPA |
| aad96cb | feat: exploracao SIAT schema + TXT + decisoes DP-07 a DP-10 |
| 72f879e | feat: camada LOTES consolidada (198.126 registros) + docs atualizado |
| 123c07d | docs: regras consolidacao LOTES v1.0 + notebooks exploracao shapefiles v1-v6 |

---

## 11. COMO RETOMAR ESTA SESSÃO

### No Claude.ai (web)
Cole no início da conversa:
> "Estou retomando o desenvolvimento do TCC de MBA em Data Science e Analytics (USP/ESALQ) sobre Regiões Homogêneas de Valorização Imobiliária em Porto Alegre. O contexto completo está em CONTEXTO_SESSAO.md no repositório https://github.com/franciscoteston/tcc_mba_datascience. A próxima tarefa é [descrever]."

### No Claude Desktop (com MCP GitHub configurado)
> "Leia o arquivo CONTEXTO_SESSAO.md do repositório franciscoteston/tcc_mba_datascience e retome o desenvolvimento a partir da pendência P-01."

### Próxima tarefa imediata
**P-01 — Leitura completa do IMO_VW_LOTE_FISCAL**  
Notebook a criar: `07_explora_lote_fiscal.ipynb`  
Ler apenas colunas: `IDF_LOTE`, `NUM_BLOCO`, `MTR_AREA_REAL`, `DES_FIGURA`, `DES_POSICAO_NA_QUADRA`, `IND_GLEBA`, `IND_PARCELAMENTO_IRREGULAR`, `DES_TIPO_CATEGORIA`, `DES_RATEIO`, `DES_SITUACAO`, `NUM_SETOR`, `NUM_QUARTEIRAO`
