# Arquitetura do Sistema - Olist ETL (RM-ODP)

Este documento descreve a arquitetura do pipeline de ETL utilizando o framework RM-ODP, garantindo a rastreabilidade com os requisitos e respeitando as restrições do AWS Academy Learner Lab.

## 1. Enterprise Viewpoint (Visão de Negócio)
O sistema visa automatizar a consolidação de dados de e-commerce para suporte à decisão diária.
- **Objetivo:** Processar ~100k pedidos/dia com alta confiabilidade e observabilidade.
- **Stakeholders:** Analistas de Negócio (Consumidores), Time SRE (Operadores).
- **Atende:** RF01, RF03, RNF02.

## 2. Information Viewpoint (Visão de Informação)
Define o fluxo e a estrutura dos dados ao longo do pipeline.
- **Data Schemas:** Baseado no Kaggle Olist Dataset (9 entidades).
- **Metadata:** Inclusão de colunas técnicas (`_ingestion_at`, `_source_file_name`).
- **Estados:** Landing (S3/Local) -> Processing (Pandas/Python) -> Gold/Analytical (PostgreSQL/SQLite).
- **Atende:** RF02, RF07, RNF01, RNF07.

## 3. Computational Viewpoint (Visão Computacional)
Decomposição funcional do software em módulos independentes.
- **Extractor Module:** Interface para leitura de CSVs com tratamento de encodings. (Atende: RF01).
- **Transformer Module:** Lógica de limpeza, conversão de tipos e validação de schema. (Atende: RF02, RF05, RNF06).
- **Loader Module:** Abstração de banco de dados para carga idempotente (UPSERT). (Atende: RF03, RF04, RNF05).
- **Logger Service:** Emissor de eventos estruturados em JSON. (Atende: RF06, RNF04).

## 4. Engineering Viewpoint (Visão de Engenharia)
Mecanismos de infraestrutura e suporte à execução.
- **Orquestração:** Script principal `main.py` com controle de fluxo e tratamento de exceções.
- **Persistência de Logs:** Armazenamento de logs de execução em tabela dedicada para auditoria.
- **Segurança:** Gestão de credenciais via variáveis de ambiente.
- **Atende:** RF08, RF09, RNF06, RNF08.

## 5. Technology Viewpoint (Visão de Tecnologia)
Stack tecnológica compatível com AWS Academy Learner Lab.
- **Linguagem:** Python 3.12.
- **Processamento:** Pandas + SQLAlchemy.
- **Banco de Dados:** PostgreSQL (RDS) ou SQLite (Local Dev).
- **Containerização:** Docker (para portabilidade e isolamento).
- **Infraestrutura:** EC2 (instância t3.medium) para execução do pipeline.
- **Restrição:** Sem AWS Glue, Sem AWS Redshift.
- **Atende:** RNF03, RNF09.

---

## Architecture Decision Records (ADRs)

### ADR 01: Uso de SQLAlchemy e PostgreSQL no RDS
- **Contexto:** Necessidade de um banco analítico relacional persistente e robusto.
- **Decisão:** Utilizar PostgreSQL hospedado no RDS com a camada de abstração SQLAlchemy.
- **Consequência:** Garante interoperabilidade (RNF03) e permite o uso de UPSERT nativo para idempotência (RNF05), respeitando a cota do Learner Lab.

### ADR 02: Processamento com Pandas e Python em EC2
- **Contexto:** Volume de ~100k linhas não justifica o overhead de Spark/Glue, e as ferramentas gerenciadas estão restritas.
- **Decisão:** Executar o pipeline como um processo Python em uma instância EC2.
- **Consequência:** Simplicidade de manutenção, baixo custo e controle total sobre o ambiente (Portabilidade - RNF09).

### ADR 03: Idempotência via Chave Primária e UPSERT
- **Contexto:** Riscos de duplicidade em caso de re-execuções (Rerun).
- **Decisão:** Definir chaves primárias baseadas nos IDs originais (ex: `order_id`) e usar lógica de `ON CONFLICT DO UPDATE`.
- **Consequência:** Garante o estado final consistente independentemente do número de execuções (RNF05).

---

## Matriz de Rastreabilidade (Resumo)
| Componente | Requisitos Funcionais | Requisitos Não Funcionais |
| :--- | :--- | :--- |
| **Extractor** | RF01 | RNF01 |
| **Transformer** | RF02, RF05 | RNF06, RNF08 |
| **Loader** | RF03, RF04 | RNF05, RNF03 |
| **Logger** | RF06, RF08 | RNF04, RNF07 |
| **EC2/Docker** | RF01-09 | RNF02, RNF09 |
