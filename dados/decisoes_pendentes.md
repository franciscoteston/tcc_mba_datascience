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
**Implementação:** `pd.read_csv(arquivo, sep="|", encoding="latin1", decimal=",")`

### DP-08 — Chave de ligação SIAT ↔ GEO-SMF
**Status:** Definido  
**Decisão:** `NUM_BLOCO` no SIAT é inteiro de 10 dígitos; `NUMBLOCO` no shapefile
é string de 12 dígitos com zeros à esquerda. Conversão obrigatória antes do join:  
`df["NUM_BLOCO_STR"] = df["NUM_BLOCO"].astype(str).str.zfill(12)`

### DP-09 — Tabelas SIAT e variáveis a utilizar
**Status:** Definido  

| Tabela | Papel | Variáveis relevantes |
|---|---|---|
| IMO_VW_LOTE_FISCAL | Base principal — área territorial, posição na quadra | `NUM_BLOCO`, `MTR_AREA_REAL`, `DES_FIGURA`, `DES_POSICAO_NA_QUADRA`, `IND_GLEBA`, `IND_PARCELAMENTO_IRREGULAR`, `DES_TIPO_CATEGORIA`, `DES_SITUACAO`, `NUM_SETOR`, `NUM_QUARTEIRAO` |
| IMO_VW_UNIDADE_IMOBILIARIA | Uso, finalidade, endereço, logradouro | `NUM_BLOCO`, `NUM_INSCRICAO`, `COD_ENDLOC_LOGRADOURO`, `NME_ENDLOC_LOGRADOURO`, `NUM_ENDLOC_ENDERECO`, `NUM_ENDLOC_UNIDADE`, `NME_ENDLOC_BAIRRO_CDL`, `DES_FINALIDADE`, `DES_USO`, `COD_USO`, `IND_CONSTRUCAO_NAO_CONSTITUIDA`, `DES_SITUACAO_LOTE`, `DES_SITUACAO_UNIDADE` |
| IMO_VW_CONSTRUCAO | Área construída total por inscrição | `IDF_LOTE`, `NUM_INSCRICAO`, `MTR_AREA_CONSTRUIDA_TOTAL`, `ANO_CONSTRUCAO`, `DES_TIPO_CONSTRUCAO` |
| IMO_VW_TESTADA | Frentes do lote, metragem, logradouro principal | `IDF_LOTE`, `COD_LOGRADOURO`, `FLG_PRINCIPAL`, `MTR_TESTADA`, `NUM_SEQUENCIA` |
| IMO_VW_REGISTRO_IMOVEL | Matrícula — validação NUMBLOCO compartilhado | `IDF_LOTE`, `NUM_INSCRICAO`, `DES_IDENTIFICACAO`, `TPO_REGISTRO_IMOVEL` |
| IMO_VW_IPTU_TCL | Somente para comparação: RH atual e valor venal | `NUM_INSCRICAO`, `ANO_EXERCICIO`, `IDF_REG_REGIAO_HOMOGENEA`, `VLR_VENAL_TERRENO` |
| IMO_CLASSES_E_RH | Somente para comparação: nome e valor das RH | `IDF_REG_REGIAO_HOMOGENEA`, `DES_REGIAO`, `VLR_REGIAO_CLASSE`, `ANO_EXERCICIO` |
| IPT_VW_VALORES_EXERCICIO_DAT | **Removido da pasta — desconsiderado** | — |

### DP-10 — Frentes do lote
**Status:** Definido  
**Decisão:** Número de frentes derivado de `IMO_VW_TESTADA` — contar linhas
por `IDF_LOTE` com `FLG_PRINCIPAL = 'S'` ou total de `NUM_SEQUENCIA` distintos.
`COD_LOGRADOURO` cruzado com `CDIDELOG` dos eixos para obter categoria da via.
Complementa `DES_POSICAO_NA_QUADRA` de `IMO_VW_LOTE_FISCAL`.

### DP-11 — Coordenadas (COORD_X e COORD_Y)
**Status:** Pendente de decisão  
**Descrição:** Coordenadas não existem nas views do SIAT. Serão produzidas a
partir da geometria dos lotes consolidados (centroide de cada polígono na camada
LOTES). Derivar no notebook de pré-processamento após join SIAT ↔ GEO-SMF.  
**Ação futura:** Definir se usar centroide do polígono BF (quando disponível)
ou centroide do LF/GL. Seguir hierarquia de qualidade geométrica BF > LF = GL.

### DP-12 — Área construída (AREA_CONSTRUIDA)
**Status:** Pendente de decisão  
**Descrição:** Obtida pelo somatório de `MTR_AREA_CONSTRUIDA_TOTAL` em
`IMO_VW_CONSTRUCAO`, agrupado por `NUM_INSCRICAO`.  
**Ação futura:** Decidir se utilizamos apenas a área total ou se mantemos
detalhamento por sequência construtiva (tipo, ano). Para terrenos sem construção
(`IND_CONSTRUCAO_NAO_CONSTITUIDA = 'N'`), área construída = 0.

### DP-13 — Área territorial (AREA_TERRITORIAL)
**Status:** Definido  
**Decisão:** Usar `MTR_AREA_REAL` de `IMO_VW_LOTE_FISCAL` como área territorial.  
**Pendência:** Analisar tratamento de terrenos de condomínios vs. lotes isolados
— ambos cadastrados como lotes fiscais, mas com dinâmica de mercado diferente.
Verificar campo `DES_TIPO_CATEGORIA` e `DES_RATEIO` para identificar condomínios.

### DP-14 — RH atual (RH_NOME e RH_VALOR)
**Status:** Definido  
**Decisão:** `IDF_REG_REGIAO_HOMOGENEA` e valores venais associados obtidos de
`IMO_VW_IPTU_TCL` e `IMO_CLASSES_E_RH` servem **apenas como parâmetro de
comparação** com as RH identificadas pelo modelo — não entram como variáveis
de modelagem.

