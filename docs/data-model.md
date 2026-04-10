# Estrutura de Dados

## Entidades centrais

1. `county_rules`
2. `auction_listings` *(planejada para batch ingestion)*
3. `property_core`
4. `location_context`
5. `env_risk`
6. `valuation`
7. `title_risk`
8. `approved_candidates`
9. `run_log`

## Chaves e relacionamentos

- Chave de execução: `run_id`
- Chave de jurisdição: `state + county`
- Chave de propriedade: `parcel_id` (com aliases `folio`, `property_number`)

Relacionamentos:
- `county_rules` (1) -> (N) `auction_listings`
- `auction_listings` (1) -> (1) `property_core` por `parcel_id`
- `property_core` (1) -> (1) `location_context`
- `property_core` (1) -> (1) `env_risk`
- `property_core` (1) -> (1) `valuation`
- `property_core` (1) -> (1) `title_risk`
- `property_core` (1) -> (1) `approved_candidates`
- `run_log` referencia todos via `run_id` e `step`

## Status globais por propriedade

- `new`
- `in_analysis`
- `discarded_early`
- `pending_manual_review`
- `approved`
- `watchlist`
- `rejected`
- `error`

## Campos mínimos de governança

Todas entidades devem incluir:
- `run_id`
- `state`
- `county`
- `parcel_id` (quando aplicável)
- `last_verified_at`
- `source_links`
- `data_quality_status` (`official`, `secondary`, `inferred`)

## Modelo Google Sheets (inicial)

Uma planilha com abas:
- `county_rules`
- `auction_listings`
- `property_core`
- `location_context`
- `env_risk`
- `valuation`
- `title_risk`
- `approved_candidates`
- `run_log`

Trade-off:
- **Sheets** acelera bootstrap e revisão humana.
- Limite: concorrência, performance e governança.
- Mitigação: schema estável para migração posterior a Airtable/PostgreSQL.
