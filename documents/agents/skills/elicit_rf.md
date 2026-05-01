---
name: elicit-rf
description: Elicitação e documentação de Requisitos Funcionais (RF). Use esta skill para identificar o que o sistema deve fazer, definindo comportamentos, entradas, saídas e regras de negócio, especialmente em pipelines de dados e ETL.
---

# Elicitação de Requisitos Funcionais (RF)

Este guia orienta o processo de identificação, análise e documentação de Requisitos Funcionais, garantindo que as necessidades de negócio sejam traduzidas em funcionalidades técnicas claras.

## Diretrizes de Escrita
- Use sempre a forma imperativa: "O sistema deve..." ou "O pipeline precisa...".
- Seja atômico: cada requisito deve descrever apenas uma funcionalidade.
- Evite termos ambíguos como "rápido", "fácil" ou "eficiente" sem métricas (estes geralmente pertencem aos Requisitos Não Funcionais).

## Fluxo de Trabalho para Projetos de Dados (ETL)
1. **Identificação de Origem:** Quais são as fontes de dados (ex: CSVs do Olist)?
2. **Mapeamento de Transformação:** Quais limpezas, normalizações e agregações são necessárias?
3. **Definição de Carga:** Onde os dados serão persistidos e como?
4. **Auditoria e Logs:** Quais eventos devem ser registrados para garantir a rastreabilidade?

## Formato Padrão de Documentação
Os requisitos devem ser registrados na tabela de Requisitos Funcionais com os seguintes campos:

| ID | Requisito | Descrição | Prioridade (MoSCoW) |
| :--- | :--- | :--- | :--- |
| **RFXX** | Nome Curto | "O sistema deve [ação] para que [objetivo]." | Must/Should/Could |

## Exemplo para Olist ETL
- **RF01 - Ingestão de CSV:** O sistema deve ler os 9 arquivos CSV do dataset Olist preservando a codificação original.
- **RF02 - Tratamento de Nulos:** O sistema deve substituir valores nulos em colunas de preço por zero antes da carga.

## Validação
Antes de finalizar um requisito, questione:
1. **É testável?** Consigo criar um teste unitário ou de integração para isso?
2. **É necessário?** Ele contribui diretamente para os objetivos do projeto?
3. **É completo?** Ele descreve a entrada, o processo e a saída esperada?
