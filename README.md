# Seção 06 — Plano Mais Produção (PMP)

Análise dos financiamentos concedidos no âmbito do **Plano Mais Produção (PMP)**,
cobrindo as seis missões estratégicas, as fontes de financiamento (BNDES, BB,
CAIXA, BNB, BASA, Finep) e a distribuição regional no triênio 2023–2025.

## Responsável

| Campo | Informação |
|-------|-----------|
| Equipe | Equipe de Política Industrial |
| Responsável | Amanda |
| E-mail | responsavel@cgid.gov.br |
| Linguagem | Python 3.10+ |

## Fonte de Dados

| Arquivo | Origem | Onde colocar |
|---------|--------|--------------|
| `PlanoMaisProducao_Atualizado.xlsx` | Painel PMP — MDIC | `data/financiamento_consolidado/` |

O xlsx deve ter 4 abas pré-agregadas: **UF × Ano**, **Fonte × Ano**,
**Missão × Ano** e **UF × Missão × Ano**.

## Como executar

```bash
# 1. Instalar dependências
pip install -r requirements.txt

# 2. Colocar o xlsx em data/financiamento_consolidado/

# 3. Gerar parquets (pipeline de dados)
python -m src.pmp_financiamento

# 4. Renderizar o relatório
quarto render 06_pp.qmd          # capítulo do livro
quarto render 07_pmp.qmd         # versão standalone (HTML + docx)
```

## Estrutura de Arquivos

```
06_pp/
├── 06_pp.qmd                    # Capítulo do livro conjuntura_nacional
├── 07_pmp.qmd                   # Versão standalone (HTML + docx)
├── config.yml                   # Configurações da seção
├── requirements.txt             # Dependências Python
├── GUIA_VSCODE.md               # Guia de execução no VS Code
├── _quarto.yml                  # execute-dir: project
├── README.md                    # Este arquivo
├── src/
│   ├── __init__.py
│   ├── api_pmp.py               # Leitor do xlsx (4 abas) + filtro NV
│   ├── processamento.py         # Agregações + tabela_resumo + kpis_triplo
│   ├── visualizacao.py          # 8 gráficos matplotlib
│   └── pmp_financiamento.py     # Orquestrador do pipeline
└── data/                        # Gitignored — criado pelo pipeline
    ├── financiamento_consolidado/
    │   ├── PlanoMaisProducao_Atualizado.xlsx   ← coloque aqui
    │   ├── pmp_consolidado.parquet
    │   ├── pmp_tabela_resumo.parquet
    │   └── pmp_kpis_triplo.parquet
    ├── missoes/
    │   └── pmp_missoes.parquet
    ├── fontes/
    │   └── pmp_fontes.parquet
    └── regional/
        ├── pmp_ufs.parquet
        ├── pmp_uf_missao.parquet
        └── pmp_regioes.parquet
```

## Missões PMP

| Missão | Cor |
|--------|-----|
| Agroindustrial | #2E7D32 |
| Saúde | #1565C0 |
| Infraestrutura | #E65100 |
| Transformação Digital | #6A1B9A |
| Descarbonização e bioeconomia | #00838F |
| Defesa | #424242 |

## Nota sobre UF=NV

Operações do Banco do Brasil sem desagregação geográfica (sigla "NV")
somam ~R$ 87,4 bi no triênio. São excluídas dos gráficos por UF/Região
e sinalizadas com nota de rodapé. Análises por missão e fonte incluem esses valores.
