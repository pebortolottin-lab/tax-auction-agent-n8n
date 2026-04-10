# Workflow Specifications

## Master Orchestrator

**Entrada:**
- `state` (required)
- `county` (required)
- `parcel_id` (optional)
- `run_mode` (`single_property` | `county_batch`)

**Responsabilidades:**
- inicializar contexto de execução
- chamar sub-workflows na ordem
- persistir status global
- tratar falhas e retry policy

## Step 1 - County Auction Rules

**Entrada:** `state`, `county`

**Lógica:**
1. lookup local (cache)
2. se inexistente/expirado: coletar fontes oficiais
3. estruturar resultado conforme schema
4. salvar e retornar

**Dependências externas:**
- `manual integration placeholder`
- `scraping layer placeholder`

## Step 2 - Property Core + Appraisal + GIS

**Entrada:** `state`, `county`, `parcel_id|folio|property_number`

**Lógica:**
1. resolver identificadores
2. coletar core + appraisal + GIS
3. aplicar hard filters de descarte
4. salvar com flags e motivação

## Step 3 - Location Context (planejado)

Validação visual/contextual sem dados sensíveis de pessoas.

## Step 4 - Environmental & Physical Risk (planejado)

Detecção de inviabilidade física/ambiental com score.

## Step 5 - Valuation (planejado)

Estimativa de faixa de valor e margem de segurança.

## Step 6 - Title / Liens / Public Records (planejado)

Risco jurídico pós-arrematação.

## Step 7 - Final Scoring + Human Review

**Entrada:** registros consolidados de Steps 1..6.

**Lógica:**
- normalizar pesos de score
- aplicar regras de veto
- classificar (`approved`, `watchlist`, `reject`)
- preparar links operacionais e sumário

