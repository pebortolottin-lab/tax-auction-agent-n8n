# Repository Structure

## Visão geral

```text
.
├── README.md
├── config/
│   ├── google_sheets_tabs.md
│   └── scoring_weights.example.json
├── docs/
│   ├── architecture.md
│   ├── data-model.md
│   ├── implementation-phases.md
│   ├── repository-structure.md
│   └── workflow-specifications.md
├── examples/
│   ├── input_single_property.json
│   └── input_county_batch.json
├── n8n/
│   ├── specs/
│   │   ├── master_orchestrator.md
│   │   ├── step1_county_rules.md
│   │   ├── step2_property_core.md
│   │   └── step7_final_scoring.md
│   ├── subworkflows/
│   │   ├── step1_county_rules.json
│   │   ├── step2_property_core.json
│   │   └── step7_final_scoring.json
│   └── workflows/
│       └── master_orchestrator.json
├── prompts/
│   ├── step1_county_rules_prompt.md
│   ├── step2_property_core_prompt.md
│   ├── step3_location_context_prompt.md
│   ├── step4_env_risk_prompt.md
│   ├── step5_valuation_prompt.md
│   ├── step6_title_risk_prompt.md
│   └── step7_final_scoring_prompt.md
└── schemas/
    ├── county_rules.schema.json
    ├── property_core.schema.json
    ├── location_context.schema.json
    ├── env_risk.schema.json
    ├── valuation.schema.json
    ├── title_risk.schema.json
    ├── approved_candidates.schema.json
    └── run_log.schema.json
```

## Decisões de organização

- `docs/`: decisões arquiteturais e especificações operacionais.
- `schemas/`: contrato de dados versionável e independente de banco.
- `n8n/workflows`: entrypoints (orquestração principal).
- `n8n/subworkflows`: módulos de etapa (reuso + debug simples).
- `n8n/specs`: contrato funcional por workflow, separado do JSON bruto.
- `prompts/`: instruções de IA por agente, com guardrails e critérios de descarte.
- `config/`: parâmetros de ambiente e templates de configuração.
- `examples/`: payloads de entrada para testes manuais e QA.
