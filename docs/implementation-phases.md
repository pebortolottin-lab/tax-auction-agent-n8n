# Plano Incremental de Implementação

## Fase 1 — Base funcional mínima

Escopo:
- Master Orchestrator
- Step 1, Step 2 e Step 7 funcionais
- Persistência em Google Sheets
- run_log e status por propriedade

Critério de pronto:
- pipeline roda fim-a-fim em `single_property`
- descarte precoce ativo no Step 2

## Fase 2 — Enriquecimento de fontes e filtros

Escopo:
- implementação dos Steps 3, 4, 5, 6
- conectores por condado com priorização oficial
- cache por `state/county` para regras recorrentes

Critério de pronto:
- pipeline roda em `county_batch` com governança mínima de fontes

## Fase 3 — Scoring e alertas

Escopo:
- calibragem de pesos por tipo de sale
- classificação robusta approved/watchlist/reject
- alertas (email/slack/webhook) para aprovados

Critério de pronto:
- redução de falsos positivos com validação humana amostral

## Fase 4 — Hardening e escala

Escopo:
- retry/backoff por fonte
- dead-letter queue lógica
- migração opcional para Airtable/PostgreSQL
- observabilidade avançada (dashboards de erro e throughput)

Critério de pronto:
- operação contínua em múltiplos condados com SLA definido
