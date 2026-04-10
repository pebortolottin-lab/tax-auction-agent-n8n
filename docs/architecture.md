# Arquitetura do Sistema (n8n-first)

## 1) Componentes principais

### A. Master Orchestrator (workflow raiz)
Responsável por:
- Receber payload de entrada (`state`, `county`, `parcel_id?`, `run_mode`)
- Gerar `run_id` e metadados de execução
- Decidir fluxo (`single_property` vs `county_batch`)
- Encadear Steps 1..7 via `Execute Workflow`
- Centralizar logs e status por propriedade

### B. 7 Sub-workflows especializados
1. **County Auction Rules Agent**
2. **Property Core + Appraisal + GIS Agent**
3. **Location Context Agent** *(especificado; implementação futura)*
4. **Environmental & Physical Risk Agent** *(especificado; implementação futura)*
5. **Valuation Agent** *(especificado; implementação futura)*
6. **Title/Liens/Public Records Agent** *(especificado; implementação futura)*
7. **Final Scoring + Human Review Agent**

### C. Camada de Persistência
- **Inicial:** Google Sheets (aba por entidade)
- **Futuro:** Airtable/PostgreSQL sem quebrar contratos de dados

### D. Camada de Integração de Fontes
- Fontes oficiais de condado/estado (prioridade absoluta)
- `manual integration placeholder` para portais sem API
- `scraping layer placeholder` para HTML inconsistente

### E. Camada de Governança
- JSON Schemas para validação de payload
- logs de execução (`run_log`)
- flags de descarte precoce (`discard_reason`, `manual_review_required`)

## 2) Fluxo ponta-a-ponta

1. **Input Ingestion**
   - Entrada manual/API com estado, condado e opcional parcel.
2. **Step 1 (rules cache-first)**
   - Busca `county_rules` local; se ausente/expirado, coleta oficial e persiste.
3. **Property candidate loop**
   - Para cada candidato, executa Step 2 e aplica filtros hard-stop.
4. **Risk enrichment (Steps 3–6)**
   - Enriquecimento contextual, ambiental, valuation e título.
5. **Step 7 consolidation**
   - Score final, decisão, links e resumo para revisão humana.
6. **Output + Alerting**
   - Grava em `approved_candidates` e envia alerta apenas para `approved/watchlist`.

## 3) Estratégia de descarte precoce

Hard filters aplicados cedo (Step 2):
- `UND/fractional/parte ideal` => rejeição imediata
- `legal_description` incompleta => rejeição imediata
- indício forte de `landlocked` sem exceção => `manual_review_required` ou rejeição
- categorias inviáveis (cemetery, roadway, drainage, common area) => rejeição

Trade-off:
- **Pró:** reduz custo e ruído nas etapas caras.
- **Contra:** risco de falso negativo em casos limítrofes.
- **Mitigação:** categoria intermediária `watchlist/manual_review`.

## 4) Estratégia de reuso

- Contrato I/O homogêneo por sub-workflow.
- `run_id`, `state`, `county`, `parcel_id` em todos os passos.
- Normalizador de campos antes de persistir.

## 5) Observabilidade e debugging

- Cada sub-workflow grava status:
  - `queued`, `running`, `completed`, `discarded`, `error`
- `run_log` armazena timestamps, erro técnico e evidências (`source_links`).
- Debug por etapa sem reprocessar pipeline inteiro.

## 6) Segurança e conformidade operacional

- IA não inventa fato de registro público.
- Toda inferência deve carregar `confidence` + `source_links`.
- Sem uso de atributos sensíveis/protegidos de pessoas (especialmente no Step 3).
