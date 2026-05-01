# Requisitos Funcionais - Olist ETL

Este documento detalha as funcionalidades que o sistema de ETL deve realizar para atender aos objetivos de negócio descritos no problema Olist.

## Lista de Requisitos Funcionais (RF)

| ID | Requisito | Descrição | Prioridade (MoSCoW) |
| :--- | :--- | :--- | :--- |
| **RF01** | Ingestão de Dados (Extract) | O sistema deve ser capaz de extrair dados de arquivos CSV (mínimo de 9 arquivos do dataset Olist) localizados em uma área de stage inicial. | Must Have |
| **RF02** | Tratamento de Tipos e Esquema | O sistema deve validar e converter tipos de dados (Datas, Floats, Inteiros) durante o processamento, garantindo a integridade do esquema no banco de dados. | Must Have |
| **RF03** | Carga no Banco Analítico (Load) | O sistema deve persistir os dados processados em um banco de dados relacional (SQLite/PostgreSQL) pronto para consumo analítico. | Must Have |
| **RF04** | Mecanismo de Idempotência | O sistema deve realizar a carga utilizando operações de `UPSERT` ou lógica equivalente para garantir que execuções repetidas não dupliquem registros. | Must Have |
| **RF05** | Isolamento de Registros Corrompidos | O sistema deve identificar linhas de dados inválidas ou corrompidas e movê-las para uma área de erro/quarentena, permitindo que o restante do lote seja processado. | Should Have |
| **RF06** | Registro de Logs de Execução | O sistema deve registrar metadados de cada execução (timestamp, status final, volume de linhas lidas/escritas, erros encontrados) para auditoria. | Must Have |
| **RF07** | Geração de Visões Agregadas | O sistema deve criar tabelas de fatos e dimensões (Star Schema ou similar) para facilitar a geração de dashboards diários. | Should Have |
| **RF08** | Alerta de Falha Crítica | O sistema deve emitir uma notificação (log de erro explícito com código de saída não-zero) caso o banco de dados esteja indisponível ou ocorra erro de memória. | Must Have |
| **RF09** | Validação de Consistência Agregada | O sistema deve realizar uma verificação final de soma (checksum) ou contagem de linhas para garantir que o total no destino bata com o total extraído. | Should Have |

## Rastreabilidade com Fluxos Críticos
- **RF01, RF02** suportam o fluxo: *Ingestão de ~100k pedidos*.
- **RF03, RF04, RF07** suportam o fluxo: *Transformação e carga e Dashboards*.
- **RF06, RF08, RF09** suportam o fluxo: *Emissão e coleta de logs/alertas (Observabilidade)*.
