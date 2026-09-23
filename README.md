# Dataset Sintético — Dashboard de BI para Gestão Acadêmica

Base de dados **totalmente sintética** (nenhum dado de aluno real), gerada para o
Trabalho de Conclusão de Curso *"Dashboard de Business Intelligence para Suporte
à Gestão Acadêmica: Uma Abordagem com Modelagem Dimensional e Visualização
Interativa de Dados"*, apresentado por **Stephani dos Santos Melo** à FATEC Zona
Leste (Curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas), 2026.

Os dados seguem um **modelo dimensional em esquema estrela**, com 6 tabelas
dimensão e 2 tabelas fato, prontas para uso em Power BI, Excel ou qualquer
ferramenta de BI/análise de dados.

## Conteúdo

| Arquivo | Linhas | Descrição |
|---|---|---|
| `DIM_Aluno.csv` | 5.000 | Dados demográficos e acadêmicos dos alunos |
| `DIM_Curso.csv` | 6 | Cursos oferecidos pela instituição |
| `DIM_Disciplina.csv` | 15 | Disciplinas por curso e período |
| `DIM_Endereco.csv` | 5.000 | Cidade, bairro e distância ao campus |
| `DIM_Deslocamento.csv` | 5.000 | Tempo de deslocamento e modal de transporte |
| `DIM_Tempo.csv` | 100 | Dimensão de tempo (data, mês, ano, semestre letivo) |
| `FACT_Desempenho.csv` | 5.000 | Notas, frequência, aprovação/reprovação por disciplina |
| `FACT_Eventos.csv` | 1.000 | Evasões, trancamentos, jubilamentos e trocas de turno |
| `PROMPT_GERACAO_IA.md` | — | Prompt utilizado para gerar os dados com apoio de IA generativa |
| `LICENSE` | — | Licença de uso dos dados (CC0 1.0) |

## Dicionário de dados (resumo)

**DIM_Aluno**: `id_aluno`, `nome`, `sobrenome`, `data_nascimento`, `idade` (18–45),
`trabalha` (0/1), `tipo_escola` (Particular/Publica), `semestre_atual` (1–8),
`turno` (Manha/Tarde/Noite), `id_curso` (FK → DIM_Curso).

**DIM_Curso**: `id_curso`, `nome_curso`, `carga_horaria`, `qtd_alunos`,
`duracao_semestres`, `modalidade`.

**DIM_Disciplina**: `id_disciplina`, `nome_disciplina`, `id_curso` (FK),
`periodo`, `carga_horaria_disc`.

**DIM_Endereco**: `id_endereco`, `id_aluno` (FK), `cidade`, `bairro`,
`distancia_km` (0.5–45).

**DIM_Deslocamento**: `id_deslocamento`, `id_aluno` (FK), `tempo_min` (5–130),
`qtd_transportes` (1–4), `modal_principal` (a_pe/metro/carro/onibus/bicicleta).

**DIM_Tempo**: `id_tempo`, `data`, `mes`, `ano`, `semestre_letivo` (1/2),
`ano_semestre`.

**FACT_Desempenho**: `id_fato`, `id_aluno` (FK), `id_disciplina` (FK),
`id_curso` (FK), `id_tempo` (FK), `nota` (0–10), `aprovado` (0/1),
`reprovado` (0/1), `faltas`, `presencas`, `freq_percent`.

**FACT_Eventos**: `id_evento`, `id_aluno` (FK), `tipo_evento`
(evasao/trancamento/jubilamento/troca_turno), `motivo`, `id_tempo` (FK),
`semestre_evento`, `id_curso` (FK).

## Como os dados foram gerados

Os dados foram gerados com Python (`openpyxl`), a partir de regras estatísticas
definidas pela autora (faixas de valores, proporções por categoria e taxas-alvo
de aprovação/evasão), com apoio de um assistente de IA generativa na tradução
dessas regras em código e na verificação de integridade referencial. O prompt
completo utilizado está em [`PROMPT_GERACAO_IA.md`](./PROMPT_GERACAO_IA.md).

## Licença

Este dataset é disponibilizado sob a licença **CC0 1.0 Universal** (domínio
público) — veja [`LICENSE`](./LICENSE). Por se tratar de dados 100% sintéticos,
não há restrições de privacidade ou direitos de terceiros envolvidos.

## Como citar

```
MELO, Stephani dos Santos. Dataset sintético: Dashboard de Business
Intelligence para Gestão Acadêmica. São Paulo, 2026. Disponível em:
<URL_DO_REPOSITORIO>. Acesso em: DD mês. AAAA.
```

## Trabalho relacionado

Este dataset foi construído para o TCC citado no topo deste documento,
orientado pelo Prof. Ricardo Satoshi (FATEC Zona Leste, 2026). O modelo
dimensional e as medidas DAX utilizadas no dashboard estão documentados no
próprio trabalho.
