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

- Amostra: aproximadamente **700 amostras** de preços de terrenos
- Delimitação das RH: **qualitativa e quantitativa**, por **vistoria de campo**
- Aproximadamente **140 Regiões Homogêneas** definidas
- Modelos: **regressão múltipla logarítmica** para terrenos, casas e apartamentos
- A variável RH foi incluída como variável explicativa nos modelos

### Resultados
| Tipologia | R² |
|---|---|
| Terrenos | 0,94 |
| Casas | 0,95 |

---

## Comparativo com o presente estudo (2025)

| Aspecto | Lapolli et al. (1994) | Este TCC (2025) |
|---|---|---|
| Tipologia | Terrenos, casas, apartamentos | **Somente terrenos** |
| Tipo de transação | Não especificado | **Somente vendas** |
| Amostra | ~700 terrenos | Cadastro SIAT + Guias ITBI + Ofertas (PMPA) |
| Recorte temporal | Não especificado | **2022–2023** |
| Origem dos dados | Pesquisa de campo | **Dados oficiais PMPA (acesso autorizado)** |
| Delimitação das RH | Vistoria de campo qualitativa | **Clustering geoespacial automatizado** |
| Abordagem | MCD direto | **Modelo sem RH → Moran I → superfície → clustering → modelo com RH** |
| Validação espacial | Não aplicada | **Índice de Moran I** |
| Referência normativa | NBR 5676/1989 | **NBR 14653-1:2019 e NBR 14653-2:2011** |
| Ferramentas | Regressão múltipla clássica | **Python: GeoPandas, Scikit-learn, PySAL, Statsmodels** |
| Visualização | Mapas impressos | **Mapas interativos (Folium)** |

---

## Variáveis originais de 1994

> ⚠ A confirmar após consulta ao artigo completo. As variáveis exatas dos modelos de 1994 são essenciais para o comparativo metodológico da seção de Resultados e Discussão.
