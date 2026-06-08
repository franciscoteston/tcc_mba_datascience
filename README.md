# TCC — MBA em Data Science e Analytics · USP/ESALQ

## Regiões Homogêneas de Valorização Imobiliária em Porto Alegre
### Atualização metodológica com clustering geoespacial e regressão hedônica

---

**Autor:** Francisco Teston  
**Curso:** MBA em Data Science e Analytics — USP/ESALQ  
**Orientador:** a definir  
**Status:** Em desenvolvimento

---

## Sobre o trabalho

Este TCC atualiza o estudo seminal de **Lapolli et al. (1994)** — *"Metodologia para a Determinação de Regiões Homogêneas de Valorização Imobiliária"*, desenvolvido pela Secretaria Municipal da Fazenda de Porto Alegre — aplicando ferramentas modernas de Data Science ao mercado imobiliário do município.

O estudo original delimitou qualitativamente cerca de 140 Regiões Homogêneas (RH) por vistoria de campo, construindo modelos de regressão múltipla logarítmica com R² de 0,94 (terrenos) e 0,95 (casas). Após 30 anos, este trabalho busca replicar e expandir esse esforço com maior volume de dados, clustering geoespacial automatizado e atualização normativa (NBR 5676/1989 → NBR 14653-2:2011).

---

## Classificação metodológica

| Dimensão | Classificação |
|---|---|
| Delineamento | Estudo de Caso Único — Instrumental |
| Natureza dos dados | Quantitativa |
| Objetivo | Explicativo com elementos descritivos |
| Coleta de dados | Dados secundários externos |
| Referência normativa | NBR 14653-2:2011 (ABNT) |
| Limite de páginas | 30 (normas USP/ESALQ) |

---

## Objetivo da pesquisa

> Atualizar a delimitação de Regiões Homogêneas de Valorização Imobiliária do município de Porto Alegre por meio de técnicas de clustering geoespacial e regressão hedônica, comparando os resultados com o estudo de referência de Lapolli et al. (1994).

---

## Estrutura do repositório

```
tcc_mba_datascience/
├── README.md                        ← este arquivo
├── painel/
│   └── index.html                   ← painel de acompanhamento do TCC (abre no navegador)
├── dados/
│   └── README.md                    ← documentação das fontes de dados
├── notebooks/
│   └── 01_exploratorio.ipynb        ← análise exploratória (a criar)
│   └── 02_clustering.ipynb          ← clustering geoespacial (a criar)
│   └── 03_regressao.ipynb           ← regressão hedônica OLS (a criar)
├── outputs/
│   ├── mapas/                       ← mapas temáticos gerados
│   └── tabelas/                     ← tabelas de resultados
└── referencias/
    └── lapolli_1994_resumo.md       ← síntese do estudo original de 1994
```

---

## Ferramentas previstas

- **Python:** GeoPandas, Scikit-learn, PySAL, Statsmodels, Folium
- **Dados:** Portal de Dados Abertos de Porto Alegre (ITBI/SMF), IBGE Censo 2022, OpenStreetMap, portais imobiliários
- **Análises:** K-Means / DBSCAN, Índice de Moran I, OLS log-log

---

## Acompanhamento

O arquivo [`painel/index.html`](painel/index.html) contém o painel interativo de acompanhamento do TCC — estrutura de seções, status de cada etapa, decisões tomadas e pendências abertas. Abra no navegador para visualizar.

---

## Referências principais

- Lapolli, E.M. et al. (1994). Metodologia para a Determinação de Regiões Homogêneas de Valorização Imobiliária. SMF/PMPA, Porto Alegre.
- ABNT NBR 14653-2:2011. Avaliação de bens — Parte 2: Imóveis urbanos.
- Fávero, L.P.; Belfiore, P. (2019). *Data science for business and decision making*. Academic Press.
- Gujarati, D.N. (2011). *Econometria básica*. 5. ed. Bookman, Porto Alegre.
- Anselin, L. (1988). *Spatial Econometrics: Methods and Models*. Kluwer Academic Publishers.
