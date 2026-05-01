# Especificação de Requisitos - Olist ETL

Esta especificação segue as diretrizes do **SWEBOK (Software Engineering Body of Knowledge)**, categorizando as necessidades do sistema para garantir um ciclo de vida de software robusto e confiável.

## 1. Requisitos Funcionais (RF)
*Descrevem as funções que o sistema deve executar.*

| ID | Requisito | Descrição |
| :--- | :--- | :--- |
| **RF01** | Ingestão Multi-fonte | O sistema deve extrair dados de 9 arquivos CSV distintos (conforme mapeamento técnico). |
| **RF02** | Transformação de Dados | O sistema deve limpar e normalizar os dados, tratando tipos (Datas, Floats) e valores ausentes (NaN). |
| **RF03** | Carga Analítica | O sistema deve carregar os dados processados em um banco de dados relacional (SQLite/PostgreSQL). |
| **RF04** | Consolidação de Métricas | O sistema deve gerar tabelas agregadas (Fato/Dimensão) para suporte a dashboards diários. |
| **RF05** | Registro de Auditoria | O sistema deve persistir metadados de cada execução (Data/Hora, Status, Qtd de Linhas, Erros) em uma tabela de log. |

## 2. Requisitos Não Funcionais (RNF)
*Descrevem atributos de qualidade e restrições do sistema (Qualidades Sistêmicas).*

### 2.1 Confiabilidade (Reliability)
*   **RNF01 - Idempotência:** Repetidas execuções do ETL sobre o mesmo conjunto de dados não devem gerar duplicidade no destino final.
*   **RNF02 - Integridade de Dados:** O sistema deve garantir que nenhum registro válido seja perdido durante o processo de transformação (Zero Data Loss).
*   **RNF03 - Resiliência a Falhas Parciais:** Erros em registros individuais não devem interromper o processamento do lote completo. Registros falhos devem ser isolados para análise posterior.

### 2.2 Observabilidade (Observability)
*   **RNF04 - Logging Detalhado:** Toda etapa crítica (Extract, Transform, Load) deve emitir logs padronizados indicando o progresso e saúde da operação.
*   **RNF05 - Detecção de Falhas:** O sistema não deve "sofrer silenciosamente"; qualquer erro impeditivo deve resultar em uma terminação com código de erro explícito e log descritivo.

### 2.3 Desempenho (Performance)
*   **RNF06 - Escalabilidade Volumétrica:** O pipeline deve processar o volume de ~100 mil pedidos dentro de uma janela de tempo aceitável para processamento diário (ex: < 30 minutos em hardware padrão).

### 2.4 Manutenibilidade (Maintainability)
*   **RNF07 - Modularidade:** O código deve ser estruturado em módulos independentes (Extrator, Transformador, Carregador) para facilitar a evolução e testes unitários.
*   **RNF08 - Portabilidade do Banco:** O código de carga deve utilizar abstrações (SQLAlchemy/DBAPI) que permitam a troca do banco de dados analítico com esforço mínimo.

## 3. Matriz de Priorização (MoSCoW)
- **Must Have:** RF01, RF02, RF03, RNF01, RNF02, RNF05.
- **Should Have:** RF05, RNF03, RNF04, RNF07.
- **Could Have:** RF04, RNF06, RNF08.
- **Won't Have (hoje):** Dashboard visual em tempo real, integração com Cloud Providers.
