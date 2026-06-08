# Fontes de Dados

Documentação das fontes de dados utilizadas no TCC.

## Status de acesso

| Fonte | Tipo | Conteúdo | Acesso | Status |
|---|---|---|---|---|
| SMF/PMPA — ITBI | Transações reais | Valor de transações imobiliárias por endereço | Portal Dados Abertos POA | ⚠ A confirmar |
| IBGE — Censo 2022 | Dados socioeconômicos | Renda, população, domicílios por setor censitário | Aberto | ✅ Disponível |
| IBGE — Malha setores censitários | Shapefile | Geometria dos setores censitários | Aberto | ✅ Disponível |
| IPPOA | Shapefiles urbanos | Bairros, zoneamento, equipamentos urbanos | Aberto | ⚠ A confirmar |
| OpenStreetMap | Malha viária | Rede de ruas, pontos de interesse | Aberto (OSMnx) | ✅ Disponível |
| ZAP Imóveis / VivaReal / OLX | Preços anunciados | Anúncios de venda/locação com endereço | Scraping / API | ⚠ A definir método |

## Notas

- Dados brutos de grande volume **não são versionados neste repositório** (usar `.gitignore`).
- Apenas amostras, dados tratados e outputs são incluídos em `outputs/`.
- Documentar aqui qualquer transformação relevante aplicada aos dados antes da análise.

## Portal de Dados Abertos — Porto Alegre

URL: https://dadosabertos.poa.br

Datasets a verificar:
- Transmissões imobiliárias (ITBI)
- Planta de Valores Genéricos (PGV)
- Cadastro imobiliário
- Logradouros e endereços

## .gitignore recomendado para dados brutos

Adicionar ao `.gitignore` da raiz:
```
dados/brutos/
dados/*.csv
dados/*.xlsx
dados/*.geojson
dados/*.shp
dados/*.dbf
dados/*.shx
```
