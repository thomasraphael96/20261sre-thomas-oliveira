# Contexto do Problema

O negócio Olist precisa processar ~100 mil pedidos do marketplace, carregar em um banco analítico e gerar dashboards diários. O ETL deve ser confiável: idempotente, observável e resiliente a falhas parciais. Não podemos perder dados, nem duplicá-los, nem sofrer silenciosamente.

# Canvas de Modelagem do Problema - Olist ETL

| Seção | Perguntas-guia | Preenchimento para o Problema Olist |
| :--- | :--- | :--- |
| **Stakeholders** | Quem ganha? Quem perde? Quem opera? | **Ganha:** Consumidores de dashboards (Diretoria, Vendas, Comercial), Operação Olist.<br>**Opera/Mantém:** Time de Engenharia de Dados, SRE. |
| **Fluxos críticos** | Quais caminhos de sucesso precisam existir? | 1. Ingestão de ~100k pedidos do marketplace para a área de stage.<br>2. Transformação e carga (Load) no banco analítico.<br>3. Atualização e renderização dos dashboards diários.<br>4. Emissão e coleta de logs/alertas (Observabilidade). |
| **Modos de falha** | O que pode dar errado? | Arquivo de origem parcial ou corrompido; Rerun do ETL duplicando dados no banco; Queda do banco de dados durante a ingestão; Falha de memória no processamento; Falha em um lote específico derrubando todo o pipeline. |
| **Riscos sistêmicos** | Que falhas só aparecem no todo? | **Perda silenciosa de linhas:** pedidos ignorados sem gerar erro crítico.<br>**Dados Stale:** pipeline roda com sucesso aparente, mas o dashboard exibe dados do dia anterior.<br>Inconsistência agregada: métricas do dashboard não batem com o volume total de pedidos. |
| **Propriedades emergentes alvo** | Que qualidades do todo precisamos proteger? | **Idempotência:** garantir o mesmo estado final independente do número de execuções.<br>**Observabilidade:** logs claros e auditoria (não sofrer silenciosamente).<br>**Resiliência:** suportar falhas parciais (ex: isolar linhas corrompidas e processar o restante). |
| **Fora de escopo (hoje)** | O que explicitamente deixamos para depois? | Definição exata de SLAs/SLOs (métricas quantitativas).<br>Escolha final da infraestrutura de nuvem ou stack tecnológica específica (arquitetura concreta). |
