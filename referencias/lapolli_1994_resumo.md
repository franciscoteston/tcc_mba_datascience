# Síntese: Lapolli et al. (1994)

**Título:** Metodologia para a Determinação de Regiões Homogêneas de Valorização Imobiliária  
**Autores:** Lapolli, E.M. et al.  
**Instituição:** Secretaria Municipal da Fazenda (SMF) / Prefeitura Municipal de Porto Alegre (PMPA)  
**Ano:** 1994  
**Referência normativa utilizada:** NBR 5676/1989

---

## Objetivo original

Definir metodologia para delimitação de Regiões Homogêneas (RH) de valorização imobiliária em Porto Alegre, visando subsidiar a Planta Genérica de Valores (PGV) para cálculo do IPTU.

---

## Metodologia empregada

### Amostra
- Aproximadamente **700 amostras** de preços de terrenos.

### Delimitação das RH
- **Qualitativa e quantitativa**, por **vistoria de campo**.
- Aproximadamente **140 Regiões Homogêneas** definidas.
- Critérios: homogeneidade de padrão construtivo, infraestrutura, uso do solo e valorização percebida.

### Modelos estatísticos
- **Regressão múltipla logarítmica** para três tipologias:
  - Terrenos
  - Casas
  - Apartamentos
- A variável RH (Região Homogênea) foi incluída como variável explicativa nos modelos.

### Resultados
| Tipologia | R² |
|---|---|
| Terrenos | 0,94 |
| Casas | 0,95 |
| Apartamentos | não especificado no resumo |

---

## Comparativo com o presente estudo (2024/2025)

| Aspecto | Lapolli et al. (1994) | Este TCC |
|---|---|---|
| Amostra | ~700 terrenos | Milhares de anúncios/transações (a definir) |
| Tipologias | Terrenos, casas, apartamentos | Idem + eventual ampliação |
| Delimitação das RH | Vistoria de campo (qualitativa) | Clustering geoespacial automatizado |
| Validação espacial | Não aplicada | Índice de Moran I |
| Ferramentas | Regressão múltipla clássica | Python / R + GeoPandas + PySAL |
| Referência normativa | NBR 5676/1989 | NBR 14653-2:2011 |
| Visualização | Mapas impressos | Mapas interativos (Folium/QGIS) |

---

## Variáveis originais (a detalhar após acesso ao artigo completo)

> ⚠ Esta seção será completada após consulta ao artigo original completo de Lapolli et al. (1994). As variáveis exatas utilizadas nos modelos de 1994 são essenciais para o comparativo metodológico.

---

## Localização do documento

- Arquivo original: SMF/PMPA — verificar disponibilidade no acervo da Secretaria ou biblioteca da UFRGS/PUCRS.
- Resumo analisado: disponível nos arquivos do projeto (arquivo 04 — PDF do artigo).
