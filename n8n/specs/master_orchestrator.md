# Master Orchestrator Spec

## Objetivo
Orquestrar Steps 1..7 com rastreabilidade por `run_id`.

## Entradas
- `state` (required)
- `county` (required)
- `parcel_id` (optional)
- `run_mode` (`single_property` default)

## Saídas
- status final da execução
- IDs/links de registros gravados

## Falhas
- erro em sub-workflow -> log `error` + parada controlada
