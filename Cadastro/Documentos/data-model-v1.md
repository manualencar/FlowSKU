# Data Model v1

Escopo: Estrutura mínima para produção de anúncios e governança de SKU.

---

## Core Entities

### Table: skus

- id (uuid, pk)
- internal_sku_code (string, unique, not null)
- supplier_sku (string, nullable)
- barcode (string, nullable)
- name (string, nullable inicialmente)
- brand_id (fk -> brands.id)
- supplier_id (fk -> suppliers.id)
- sku_type (enum)
- status (enum)
- created_at (timestamp)
- updated_at (timestamp)

---

### Table: suppliers

- id (uuid, pk)
- name (string)
- created_at (timestamp)

---

### Table: brands

- id (uuid, pk)
- name (string)
- created_at (timestamp)

---

## Pipeline

### Table: pipeline_items

- id (uuid, pk)
- sku_id (uuid, fk -> skus.id, not null)
- stage (enum)
- status (enum)
- raw_input (jsonb)
- structured_data (jsonb)
- copy_data (jsonb)
- validation_notes (text)
- created_at (timestamp)
- updated_at (timestamp)

Observações:
- raw_input armazena dados desestruturados.
- structured_data contém ficha técnica parcial.
- copy_data contém textos gerados.

---

## Listings

### Table: listings

- id (uuid, pk)
- sku_id (uuid, fk -> skus.id)
- marketplace (string)
- title (string)
- description (text)
- price (numeric)
- status (enum)
- created_at (timestamp)

---

## Inventory (estrutura inicial futura)

### Table: inventory_batches

- id (uuid, pk)
- sku_id (uuid, fk -> skus.id)
- quantity (integer)
- unit_cost (numeric)
- received_at (timestamp)

---

## Relationships Overview

SKU é a entidade central.

- SKU 1:N PipelineItems
- SKU 1:N Listings
- SKU 1:N InventoryBatches
- Supplier 1:N SKUs
- Brand 1:N SKUs
