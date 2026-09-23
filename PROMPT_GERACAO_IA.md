# Prompt utilizado para geração da base sintética (via IA generativa)

> Este prompt foi reconstruído a partir da análise estatística do dataset final
> (`Base_TCC.xlsx`), para documentar de forma fiel as regras que geraram os dados
> — em conformidade com a seção 3.5 do TCC ("Uso de inteligência artificial na
> pesquisa"). Ele pode ser reutilizado, com um script Python (openpyxl/pandas),
> para gerar uma nova amostra com as mesmas características estatísticas.

---

## Prompt

Gere uma base de dados sintética em Python (openpyxl), para um dashboard
acadêmico de Business Intelligence, seguindo um modelo dimensional em esquema
estrela com 6 tabelas dimensão e 2 tabelas fato. Nenhum dado deve corresponder
a pessoas reais.

### DIM_Curso (6 linhas)
Cursos fixos, cada um com: `id_curso`, `nome_curso`, `carga_horaria` (int,
2400–3050h), `qtd_alunos`, `duracao_semestres`, `modalidade` ("Presencial").
Usar exatamente estes 6 cursos:
1. Análise e Desenvolvimento de Sistemas — 4 semestres
2. Logística — 4 semestres
3. Administração — 6 semestres
4. Comércio Exterior — 4 semestres
5. Direito — 10 semestres
6. Enfermagem — 8 semestres

### DIM_Disciplina (15 linhas)
`id_disciplina`, `nome_disciplina`, `id_curso` (FK), `periodo` (1 a 4),
`carga_horaria_disc` (60 ou 80h). 2 a 3 disciplinas por curso, distribuídas
pelos primeiros 4 períodos.

### DIM_Aluno (5.000 linhas)
`id_aluno`, `nome`, `sobrenome` (nomes brasileiros comuns), `data_nascimento`,
`idade` (derivada da data, faixa 18–45 anos, distribuição aproximadamente
uniforme), `trabalha` (0/1, ~50%/50%), `tipo_escola` ("Particular"/"Publica",
~50%/50%), `semestre_atual` (inteiro de 1 a 8, sorteado de forma independente
da duração do curso — não usar como indicador de atraso de conclusão sem
cruzar com `duracao_semestres`), `turno` ("Manha"/"Tarde"/"Noite", ~33% cada),
`id_curso` (FK, distribuição aproximadamente uniforme entre os 6 cursos).

### DIM_Endereco (5.000 linhas, 1:1 com aluno)
`id_endereco`, `id_aluno` (FK), `cidade` (fixo "São Paulo"), `bairro`
("Bairro " + número sequencial ou aleatório), `distancia_km` (float, 0.5 a
45 km, distribuição aproximadamente uniforme, 1 casa decimal).

### DIM_Deslocamento (5.000 linhas, 1:1 com aluno)
`id_deslocamento`, `id_aluno` (FK), `tempo_min` (int, 5 a 130 min,
aproximadamente uniforme, correlacionado com `distancia_km` do endereço),
`qtd_transportes` (int, 1 a 4, ~25% cada), `modal_principal` (uma de:
"a_pe", "metro", "carro", "onibus", "bicicleta" — aproximadamente 20% cada).

### DIM_Tempo (100 linhas)
`id_tempo`, `data` (datas entre 2020 e 2024), `mes`, `ano`, `semestre_letivo`
(1 ou 2, conforme o mês), `ano_semestre` (texto "AAAA/S", ex.: "2024/1").

### FACT_Desempenho (5.000 linhas — 1 por aluno)
`id_fato`, `id_aluno` (FK), `id_disciplina` (FK, compatível com o curso do
aluno), `id_curso` (FK, denormalizado para facilitar DAX), `id_tempo` (FK),
`nota` (float, 0 a 10, 1 casa decimal, média alvo ≈ 5,07, desvio-padrão ≈
2,9), `aprovado` (1 se `nota` ≥ 5,0, senão 0 — taxa de aprovação alvo
≈ 51,4%), `reprovado` (complementar a `aprovado`), `faltas` (int, 0 a 30),
`presencas` (int, 40 a 100), `freq_percent` = `presencas` / (`presencas` +
`faltas`).

### FACT_Eventos (1.000 linhas)
`id_evento`, `id_aluno` (FK), `tipo_evento` (uma de: "evasao", "trancamento",
"jubilamento", "troca_turno" — aproximadamente 25% cada), `motivo` (uma de:
"Distancia", "Desmotivacao", "Problemas pessoais", "Trabalho", "Insatisfacao
com curso", "Reprovacoes acumuladas", "Mudanca de cidade", "Financeiro" —
aproximadamente 12,5% cada), `id_tempo` (FK), `semestre_evento` (texto "Nº
Semestre", concentrado do 1º ao 6º semestre), `id_curso` (FK).

### Regras gerais
- Todas as chaves estrangeiras devem ser válidas (nenhum registro órfão entre
  tabelas fato e dimensão).
- Usar `random.seed()` fixo para reprodutibilidade.
- Exportar cada tabela como uma aba de um único arquivo `.xlsx`, usando
  openpyxl, com cabeçalhos na primeira linha.
- Não incluir nenhum dado de aluno, escola ou instituição reais.

---

## Observação sobre fidelidade

Os parâmetros acima (médias, faixas e proporções) foram calculados a partir do
dataset final e batem com os indicadores citados no Capítulo 5 do TCC — **taxa
de aprovação 51,4% e nota média 5,07** conferem exatamente. Vale registrar,
para honestidade metodológica: ao recalcular os recortes por sub-grupo
diretamente da base, a diferença de nota entre alunos que trabalham e os que
não trabalham ficou em ~0,01 ponto (não ~0,9 como consta no texto atual), e a
taxa de aprovação no turno noturno (54,9%) ficou **acima** da manhã (49,0%),
não abaixo. Vale revisar esses dois trechos específicos do Capítulo 5 antes da
defesa, para que o texto reflita exatamente os números desta base.
