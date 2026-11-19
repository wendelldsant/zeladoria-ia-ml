# Zeladoria do Bairro · IA & Machine Learning

**GS CitizenTech 2026.2 · FIAP · Engenharia de Software (3º ano Fev)**

**Integrantes**
- Augusto Rocha Silva · RM556316
- Erik Yuuta Goto · RM558076
- Guilherme Vieira Augusto · RM557264
- Wendell dos Santos Silva · RM558859

**Vídeo (até 5 min):** https://youtu.be/9uRK2AbNikg

## O que esta pasta entrega
A parte de IA & ML da Zeladoria do Bairro responde a uma pergunta de classificação:

> Dado um chamado de **tapa-buraco** aberto na cidade de São Paulo (canal, local e dia da semana), ele será **resolvido em até 7 dias**?

| Item | Resumo |
|---|---|
| Dados | SP156, chamados reais da Prefeitura de São Paulo (dados abertos, licença CCZero), 2024 T4 a 2025 T4 |
| Divisão | Por tempo: treino = 2024 T4, 2025 T1 e T2 (86.710 linhas); teste = 2025 T3 e T4 (49.791 linhas) |
| Modelo | Random Forest, 12 combinações de 3 hiperparâmetros, validação cruzada só no treino |
| Resultado no teste | AUC 0,573 (treino 0,763); precisão 0,664; a ordem entre subprefeituras bate (Spearman 0,78) |
| Conclusão | O modelo não deve prometer prazo a um chamado, mas mostra em quais regiões o buraco tende a demorar mais |
| ODS | 11 (cidades e comunidades sustentáveis) e 9 (indústria, inovação e infraestrutura) |

## Estrutura da pasta
```
ml/
├── analise_zeladoria.ipynb   notebook executado (limpeza, treino, resultados)
├── RELATORIO_ML.md           relatório do problema à conclusão
├── requirements.txt          dependências
├── prompts.md                prompts de IA usados
├── dados/
│   └── README.md             fonte, arquivos, período, filtros e decisões sobre os dados
└── saida/
    ├── cv_resultados.csv     resultado das 12 combinações da busca
    ├── metricas_finais.csv   métricas de treino e teste
    └── figuras/              gráficos do notebook e do relatório
```

## Como executar
1. Instale as dependências: `python -m pip install -r requirements.txt`
2. Baixe os 5 CSVs do SP156 (lista e link em `dados/README.md`) e salve em `dados/` (cada arquivo tem cerca de 100 MB, por isso **os CSVs não estão no repositório**).
3. Abra `analise_zeladoria.ipynb` e use *Run All*. As figuras e tabelas são regeneradas em `saida/`.

O notebook já está salvo com as saídas, então o resultado pode ser conferido sem executar de novo.

## Documentos
- Relatório: [`RELATORIO_ML.md`](RELATORIO_ML.md)
- Dados e decisões sobre eles: [`dados/README.md`](dados/README.md)
- Prompts de IA: [`prompts.md`](prompts.md)
