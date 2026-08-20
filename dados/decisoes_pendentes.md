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

