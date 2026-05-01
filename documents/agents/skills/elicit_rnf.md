---
name: elicit-rnf
description: Elicitação e documentação de Requisitos Não Funcionais (RNF) baseada na ISO 25010. Use esta skill para transformar objetivos de qualidade vagos em métricas mensuráveis (SLIs/SLOs) e atributos de qualidade técnica.
---

# Elicitação de Requisitos Não Funcionais (RNF)

Esta skill guia o processo de definição de qualidades sistêmicas para garantir que o sistema não apenas funcione, mas opere com a qualidade necessária.

## Quando Usar
- Quando o usuário solicitar a definição de RNFs.
- Requisitos: Existência de `specs/00_problem.md` e/ou `documents/01_functional_requirements.md`.

## Atributos ISO 25010 a Cobrir
1. **Adequação Funcional:** Completude e correção funcional.
2. **Eficiência de Desempenho:** Comportamento temporal, utilização de recursos, capacidade.
3. **Compatibilidade:** Coexistência, interoperabilidade.
4. **Usabilidade:** Reconhecimento de adequação, facilidade de aprendizado, operabilidade.
5. **Confiabilidade:** Maturidade, disponibilidade, tolerância a falhas, recuperabilidade.
6. **Segurança:** Confidencialidade, integridade, não-repúdio, responsabilidade, autenticidade.
7. **Manutenibilidade:** Modularidade, reusabilidade, analisabilidade, modificabilidade, testabilidade.
8. **Portabilidade:** Adaptabilidade, facilidade de instalação, substituibilidade.

## Regras de Ouro
- **Proibido Termos Vagos:** "O sistema deve ser confiável" é inválido. Use métricas.
- **Mensurabilidade:** Todo RNF deve ter um **SLI** (Service Level Indicator) e um **SLO** (Service Level Objective).
- **Janela de Tempo:** Defina sempre o período de medição (ex: por dia, por execução).

## Passos da Execução
1. Analise os stakeholders e fluxos críticos no `00_problem.md`.
2. Mapeie cada fluxo aos 8 atributos da ISO 25010.
3. Para cada atributo, proponha de 1 a 3 RNFs mensuráveis.
4. Defina a prioridade usando o método **MoSCoW**.
5. Identifique as fontes de medição (onde o dado do SLI será coletado).

## Formato de Saída (Exemplo)
O arquivo `documents/02_non_functional_requirements.md` deve conter:

### [Atributo ISO]
- **RNF-01:** Descrição técnica.
- **SLI:** O que medir (ex: tempo de execução).
- **SLO:** Meta (ex: < 30 min).

### Tabela Resumo Final
| ID | Atributo | SLI | SLO | Fonte | Prioridade |
| :--- | :--- | :--- | :--- | :--- | :--- |
| RNF-01 | Desempenho | Tempo de Processamento | < 30min/100k linhas | Logs de Execução | Must |

## Critérios de Aceitação
- Todos os 8 atributos da ISO 25010 foram avaliados.
- Nenhum requisito é aspiracional.
- Prioridades MoSCoW estão explícitas.
