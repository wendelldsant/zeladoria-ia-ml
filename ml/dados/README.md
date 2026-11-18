# Dados · Zeladoria do Bairro (IA & ML)

Todos os dados usados são **reais e públicos**. Nenhum dado simulado foi usado nesta parte.

## Fonte

| Item | Descrição |
|---|---|
| Base | SP156 (chamados registrados na central 156 da Prefeitura de São Paulo) |
| Portal | Portal de Dados Abertos da Cidade de São Paulo |
| Link | https://dados.prefeitura.sp.gov.br/dataset/dados-do-sp156 |
| Licença | Creative Commons CCZero (conforme a página do dataset) |
| Formato | CSV trimestral, separador `;`, codificação Latin-1 |

## Arquivos usados (pasta `data/`, não versionada)

Cada arquivo tem cerca de 100 MB, acima do limite do GitHub, então **os CSVs não estão no repositório**. Para reproduzir, baixe-os na página do dataset (um recurso por trimestre) e salve em `data/` com estes nomes:

| Arquivo | Trimestre | Uso |
|---|---|---|
| `arquivo-final-4-trim.-2024.csv` | 2024 T4 | treino |
| `arquivo-final-1-trim-25.csv` | 2025 T1 | treino |
| `arquivo-final-2-trim-25.csv` | 2025 T2 | treino |
| `arquivo-final-3-trim-25.csv` | 2025 T3 | teste |
| `arquivo-final-4-trim-25.csv` | 2025 T4 | teste |

## Colunas usadas e descartadas

- **Usadas:** `Data de Abertura`, `Data de Finalização`, `Status`, `Serviço`, `Meio de Comunicação` (canal), `Prefeitura Operacional` (em geral, a subprefeitura), `Distrito`, `Latitude` e `Longitude`.
- **Não carregadas:** `Logradouro`, `Número`, `CEP`, `Órgão`, `Tema`, `Assunto` e `Bairro`. O notebook lê só as colunas necessárias. Com isso, endereço e CEP não entram no modelo.
- `Data de Finalização` e `Status` servem apenas para montar o alvo (`resolvido_7d`) e **não são variáveis do modelo**, para evitar vazamento de dados.

## Filtro aplicado

Só os serviços **Tapa-buraco** e **Tapa-buraco em faixa exclusiva ou corredor de ônibus**.

| Etapa | Linhas |
|---|---|
| Chamados de tapa-buraco nos 5 trimestres | 137.299 |
| Após remover cancelados (588) | 136.711 |
| Após remover "Finalizada" sem data de finalização (187) | 136.524 |
| Após remover coordenadas fora de São Paulo (23) | 136.501 |

Divisão final: **treino 86.710** linhas (2024 T4 a 2025 T2) e **teste 49.791** (2025 T3 e T4).

## Por que estes trimestres

Foram avaliados 10 trimestres (2024 T1 a 2026 T3). Os critérios foram a proporção de chamados ainda abertos e a estabilidade do serviço.

| Trimestre | % abertos | % resolvidos em até 7 dias | Mediana de dias | Decisão |
|---|---|---|---|---|
| 2024 T1 | 0,7 | 29,0 | 30 | fora (serviço em outro patamar) |
| 2024 T2 | – | – | – | fora (arquivo sem a coluna `Serviço` com o nome esperado) |
| 2024 T3 | 22,9 | 33,3 | 10 | fora (muitos chamados abertos dois anos depois) |
| **2024 T4** | 1,0 | 40,7 | 11 | **treino** |
| **2025 T1** | 0,9 | 47,9 | 8 | **treino** |
| **2025 T2** | 1,6 | 56,1 | 6 | **treino** |
| **2025 T3** | 2,8 | 65,1 | 5 | **teste** |
| **2025 T4** | 6,5 | 57,7 | 5 | **teste** |
| 2026 T1 a T3 | 25 a 32 | 27 a 46 | 4 a 14 | fora (muitos chamados em aberto; o T3 mudou de formato) |

Percentuais sobre os chamados de tapa-buraco não cancelados; chamados abertos contam como "não resolvido em 7 dias".

## Observações de qualidade dos dados

- **O formato dos arquivos muda entre trimestres** (colunas, separador, codificação e ordem dia/mês). O 2026 T3, por exemplo, usa vírgula, UTF-8 e mês/dia. Os arquivos usados aqui têm o mesmo esquema (`;`, Latin-1, dia/mês/ano).
- **O arquivo de 2025 T1 passou pelo Excel:** tem 1.048.575 linhas (o limite do Excel), das quais 305.671 (29%) estão vazias. As aberturas vão de 01/01 a 31/03/2025 e o volume mensal é parecido; as linhas vazias saem no filtro de serviço.
- **Chamados repetidos:** o mesmo buraco pode gerar vários chamados próximos no tempo e no espaço. Por isso a divisão treino/teste é por período, e não aleatória.
- **Status pode estar desatualizado:** os arquivos são republicados e o status de chamados antigos pode mudar.
- **`Prefeitura Operacional` nem sempre é subprefeitura** (aparece, por exemplo, uma superintendência de usinas de asfalto). A análise por região usa só valores com pelo menos 300 chamados no teste, que são subprefeituras.
- **Iluminação pública não existe como serviço no SP156** (o único resultado para "apagada" é repintura de sinalização de solo). Entulho e limpeza de bueiros foram avaliados, mas fecham em até 7 dias em 85% a 100% dos casos, e por isso o ML foca em tapa-buraco.

## Privacidade

Os dados já são tratados pela prefeitura para retirar informações pessoais, conforme a LGPD. Mesmo assim, o notebook não carrega logradouro, número nem CEP, e usa apenas região (distrito e subprefeitura) e coordenadas dos chamados.
