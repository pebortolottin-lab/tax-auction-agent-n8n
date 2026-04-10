# Tax Auction Agent (n8n-first)

Sistema modular para análise de oportunidades de **tax deed / tax lien / redeemable deed** nos EUA, com foco em:

- Arquitetura profissional e não-monolítica no n8n
- Prioridade para fontes oficiais de condado/estado
- Descarte precoce de ativos ruins
- Persistência estruturada com rastreabilidade
- Aprovação final apenas para candidatos com risco controlado

## Entregáveis desta versão

- Arquitetura completa (Master + 7 sub-workflows)
- Estrutura de repositório pronta para evolução
- Esquemas de dados (JSON Schema)
- Especificações técnicas de cada workflow
- JSON base de workflows n8n:
  - `master_orchestrator`
  - `step1_county_rules`
  - `step2_property_core`
  - `step7_final_scoring`
- Modelo inicial para Google Sheets (com migração planejada para Airtable/PostgreSQL)
- Prompts auxiliares por agente
- Plano de implementação em 4 fases

## Princípios de arquitetura

1. **n8n-first**: orquestração e observabilidade centradas no n8n.
2. **Source-of-truth oficial**: IA organiza e interpreta, não substitui registro oficial.
3. **Fail-fast**: propriedades ruins são descartadas cedo para economizar custo operacional.
4. **Reuso**: sub-workflows desacoplados, com contratos de entrada/saída explícitos.
5. **Portabilidade de dados**: modelagem neutra para Sheets, Airtable ou PostgreSQL.

## Estrutura de pastas

Consulte `docs/repository-structure.md` para justificativa detalhada.

```text
/docs
/schemas
/prompts
/n8n/workflows
/n8n/subworkflows
/n8n/specs
/examples
/config
```

## Fluxo macro

1. Master recebe `{state, county, parcel_id?}` e cria `run_id`.
2. Executa Step 1 (regras do condado) com cache local.
3. Resolve lista de propriedades alvo (ou propriedade única) e executa Step 2.
4. Encadeia Steps 3–6 (especificados, a implementar no próximo ciclo).
5. Step 7 consolida, aplica filtros finais e classifica em `approved/watchlist/reject`.
6. Salva execução + logs + decisão final.

Detalhes em `docs/architecture.md`.

## Como usar os JSONs no n8n

1. Importe os arquivos em `n8n/workflows` e `n8n/subworkflows`.
2. Substitua IDs de sub-workflow nos nós `Execute Workflow`.
3. Configure credenciais de Google Sheets.
4. Ajuste placeholders de scraping/manual integration por condado.

## Placeholders explícitos

Toda integração sem API está marcada como:

- `manual integration placeholder`
- `scraping layer placeholder`

## Próximos passos recomendados

1. Implementar Steps 3, 4, 5 e 6 como sub-workflows reais.
2. Adicionar camada de normalização de identificadores (`parcel_id`, `folio`).
3. Definir scoring calibrado por estado/condado.
4. Criar suíte de testes com casos sintéticos em `examples/`.
