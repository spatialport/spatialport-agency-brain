---
id: sp-operation-ads-analytics-ingestion
client_id: spatial-port
record_type: policy
service_path: company
status: accepted
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: internal
source_ref: manual://implementation-handoff
schema_version: 1.0.0
created_at: 2026-08-11
updated_at: 2026-08-11
---
# Ads Analytics Ingestion

1. Land raw platform data in the AWS data layer.
2. Normalize by client, account, campaign, ad set, ad, creative and date.
3. Generate approved interpretation and anomalies for Obsidian.
4. Expose only client-approved summaries, never platform credentials or internal optimization notes.
