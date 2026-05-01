# Requisitos Não Funcionais - Olist ETL (ISO 25010)

Este documento define as qualidades sistêmicas do pipeline de ETL, transformando objetivos de negócio em métricas técnicas mensuráveis (SLIs/SLOs).

### 1. Adequação Funcional
- **RNF-01: Integridade de Cálculo de Valores**
- **SLI:** Diferença absoluta entre a soma de `payment_value` na origem (CSV) e no destino (DB) por lote de execução.
- **SLO:** Diferença = 0 (100% de integridade financeira).
- **Prioridade:** Must Have

### 2. Eficiência de Desempenho
- **RNF-02: Vazão de Processamento (Throughput)**
- **SLI:** Tempo total de execução para um lote de 100.000 registros de pedidos.
- **SLO:** < 30 minutos em ambiente de produção (janela diária).
- **Prioridade:** Should Have

### 3. Compatibilidade
- **RNF-03: Independência de Engine de Banco de Dados**
- **SLI:** Sucesso na execução do pipeline completo trocando o driver de conexão de SQLite para PostgreSQL.
- **SLO:** 100% de sucesso sem alterações no código-fonte de transformação.
- **Prioridade:** Could Have

### 4. Usabilidade
- **RNF-04: Observabilidade e Estrutura de Log**
- **SLI:** Porcentagem de logs de erro que contêm obrigatoriamente `trace_id`, `step_name` e `timestamp_iso8601`.
- **SLO:** 100% dos logs emitidos em caso de falha.
- **Prioridade:** Must Have

### 5. Confiabilidade
- **RNF-05: Idempotência de Carga**
- **SLI:** Número de registros duplicados (`order_id` repetido) após 3 re-execuções do mesmo arquivo de origem sobre a mesma tabela de destino.
- **SLO:** 0 duplicatas (garantido por lógica de UPSERT).
- **Prioridade:** Must Have

- **RNF-06: Resiliência a Falhas de Schema**
- **SLI:** Taxa de conclusão do pipeline quando até 1% das linhas possuem tipos de dados inválidos (ex: string em campo de data).
- **SLO:** O job deve finalizar com sucesso para 99% das linhas válidas, isolando as inválidas.
- **Prioridade:** Must Have

### 6. Segurança
- **RNF-07: Linhagem e Auditoria de Dados**
- **SLI:** Presença das colunas de metadados `_ingestion_at` e `_source_file_name` em todas as tabelas do banco analítico.
- **SLO:** 100% das tabelas criadas pelo processo de ETL.
- **Prioridade:** Must Have

### 7. Manutenibilidade
- **RNF-08: Cobertura de Testes de Regressão**
- **SLI:** Porcentagem de linhas de código da camada `transform` cobertas por testes automatizados (Pytest).
- **SLO:** > 85% de cobertura.
- **Prioridade:** Should Have

### 8. Portabilidade
- **RNF-09: Isolamento de Dependências (Containerização)**
- **SLI:** Tempo para subir o ambiente e executar o job de teste em um container Docker "limpo" (sem dependências locais).
- **SLO:** < 5 minutos.
- **Prioridade:** Could Have

## Tabela Resumo Final

| ID | Atributo | SLI | SLO | Fonte | Prioridade |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **RNF-01** | Adequação | Checksum de `payment_value` | Diferença = 0 | Log de Auditoria | Must |
| **RNF-02** | Desempenho | Tempo total / 100k registros | < 30 min | Monitoramento | Should |
| **RNF-03** | Compatibilidade | Sucesso Multi-DB | 100% Pass Rate | CI/CD Tests | Could |
| **RNF-04** | Usabilidade | Campos JSON no Log | 100% conformidade | CloudWatch/ELK | Must |
| **RNF-05** | Confiabilidade | Contagem de `order_id` duplicados | 0 duplicatas | SQL Query | Must |
| **RNF-06** | Confiabilidade | Taxa de sucesso (Error Threshold) | > 99% conclusão | Logs do Job | Must |
| **RNF-07** | Segurança | Colunas de metadados `_` | 100% cobertura | Data Dictionary | Must |
| **RNF-08** | Manutenibilidade | Code Coverage (Pytest) | > 85% | Report de CI | Should |
| **RNF-09** | Portabilidade | Setup do Container | < 5 min | Docker Build | Could |
