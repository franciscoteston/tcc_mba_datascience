# Decisões Pendentes e Registros para Análise Futura

**Documento:** decisoes_pendentes.md
**Última atualização:** junho/2025

---

## Eixos viários — decisões registradas

### DP-01 — Hierarquia da via principal de acesso
**Status:** Pendente de análise
**Descrição:** Investigar se a categoria da via de acesso principal de cada lote
(AV > ESTR > DIR > R > BC...) tem influência estatisticamente significativa
sobre o preço/m² dos terrenos. Se confirmada, incluir como variável hedônica
categórica ordinal no modelo OLS.
**Ação futura:** Pesquisar se há material oficial da PMPA ou IPPOA que categorize
hierarquicamente as vias de Porto Alegre (Plano Diretor, PDDUA). Verificar
também literatura hedônica sobre influência de hierarquia viária.
**Notebook:** a criar

### DP-02 — Eixos como delimitadores de RH
**Status:** Definido — registrar comportamento esperado
**Descrição:** Eixos estruturantes sinalizam possíveis limites entre RH, mas não
impõem barreiras rígidas ao clustering. Lados opostos de uma mesma via podem
pertencer à mesma RH ou a RH distintas — isso dependerá da análise de vizinhança
e dos valores estimados pelo modelo hedônico.
**Ação futura:** Após definição das RH por clustering, verificar quantas vezes
os limites de RH coincidem com eixos estruturantes (AV, ESTR, DIR). Esse
percentual pode ser reportado como validação qualitativa dos resultados.

### DP-03 — Número de frentes do lote
**Status:** Dados primários no SIAT
**Descrição:** Testadas por logradouro (metragem e logradouro de acesso) serão
extraídas do SIAT. Os shapefiles de eixos serão usados para conferência e para
derivar a hierarquia da via de cada testada.
**Ação futura:** Após exploração do SIAT, cruzar CDIDELOG dos eixos com o campo
de logradouro do cadastro para enriquecer cada testada com a categoria da via.

---

## Modelo hedônico — variáveis a confirmar

### DP-04 — Variável de uso do solo
**Status:** Pendente de exploração do SIAT
**Descrição:** Uso do solo (residencial, comercial, misto) pode entrar como
variável explicativa no modelo ou como critério de validação das RH.
**Ação futura:** Verificar campo de uso no SIAT após exploração dos XLSX.

### DP-05 — Recorte temporal definitivo
**Status:** Definido preliminarmente como 2022–2023
**Descrição:** Excluir 2020–2021 (pandemia) e 2024 (enchentes). Confirmar
volume amostral após exploração dos dados de ITBI e Ofertas.
**Ação futura:** Após exploração dos XLSX, verificar distribuição temporal
das transações e confirmar se 2022–2023 forma amostra suficiente.

---

## Escopo geográfico — decisões registradas

### DP-06 — Ilhas do município
**Status:** Definido
**Decisão:** Ilhas excluídas do escopo da análise (modelo hedônico e clustering).
**Justificativa:** Mercado imobiliário de ilhas tem dinâmica distinta do continente
— inclusão contaminaria o modelo com observações atípicas, violando o pressuposto
de homogeneidade amostral da NBR 14653-2.
**Implementação:**
- Camada `poa_ilhas_tm-sirgas.shp` usada como máscara de subtração sobre LOTES
- Lotes com centroide dentro de qualquer polígono de ilha → excluídos da amostra
- Ilhas mantidas na visualização do mapa do município (camada separada)
**Arquivo:** `poa_ilhas/poa_ilhas_tm-sirgas.shp`
**Notebook:** a implementar em etapa de pré-processamento

---

## SIAT — Schema e decisões de leitura

### DP-07 — Encoding e decimal dos arquivos TXT
**Status:** Definido
**Decisão:** Ler todos os `.txt` com `encoding="latin1"` e `decimal=","`.
Verificar se há caracteres corrompidos após leitura e tratar pontualmente.
**Implementação:** `pd.read_csv(arquivo, sep="|", encoding="latin1", decimal=",")`

### DP-08 — Chave de ligação SIAT ↔ GEO-SMF
**Status:** Definido
**Decisão:** `NUM_BLOCO` no SIAT é inteiro de 10 dígitos; `NUMBLOCO` no shapefile
é string de 12 dígitos com zeros à esquerda. Conversão obrigatória antes do join:
`df["NUM_BLOCO_STR"] = df["NUM_BLOCO"].astype(str).str.zfill(12)`

### DP-09 — Tabelas SIAT utilizadas no modelo
**Status:** Definido
| Tabela | Papel |
|---|---|
| IMO_VW_LOTE_FISCAL | Base principal — área, posição na quadra, figura |
| IMO_VW_UNIDADE_IMOBILIARIA | Uso, finalidade, flag terreno sem construção |
| IMO_VW_TESTADA | Frentes do lote, metragem, logradouro principal |
| IMO_VW_IPTU_TCL | Valor venal terreno, filtro temporal por ANO_EXERCICIO |
| IMO_VW_REGISTRO_IMOVEL | Matrícula — validação NUMBLOCO compartilhado |
| IMO_CLASSES_E_RH | RH da época — somente comparação futura |
| IPT_VW_VALORES_EXERCICIO_DAT | Desconsiderado (arquivo vazio) |

### DP-10 — Frentes do lote
**Status:** Definido
**Decisão:** Número de frentes derivado de `IMO_VW_TESTADA` — contar linhas
por `IDF_LOTE` onde `FLG_PRINCIPAL = 'S'` ou total de `NUM_SEQUENCIA` distintos.
`COD_LOGRADOURO` cruzado com `CDIDELOG` dos eixos para obter categoria da via.
Complementa `DES_POSICAO_NA_QUADRA` de `IMO_VW_LOTE_FISCAL`.

