---
id: agency-ev-2026-08-26-pipeline-test
client_id: spatial-port
record_type: evidence
service_path: company
status: proposed
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: internal
source_ref: notion://spatial-port-data-warehouse/05-brain-outbox/pipeline-test
schema_version: 1.1.0
created_at: 2026-08-26
updated_at: 2026-08-26
---

# Pipeline end-to-end test

## Source
Test sintetico creato da una sessione Cowork il 2026-08-26 per verificare la
pipeline Outbox → brain-outbox-sync → PR → merge → brain-update.

## Factual summary
La pipeline cloud del brain è stata installata: Action brain-outbox-sync ogni
15 minuti, trigger job brain-update su ogni push a main dei brain repo
(validate → mirror canon su Notion → deploy → Sync Log).

## Direct implications
Se questo record è stato fuso su main e una riga "Brain update" è comparsa nel
Sync Log, la pipeline è operativa end-to-end.

## Open questions
Nessuna: record di test, archiviabile in 99-archive al prossimo riordino.