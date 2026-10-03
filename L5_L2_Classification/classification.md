# L5 Narrow / L2 General Classification — api-oss-analytics
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE
**Domain:** Sovereign analytics and telemetry for Anticloud API usage — all data stays local

## L5 Narrow
api-oss-analytics collects, stores, and visualizes Anticloud API telemetry: inference latency, token throughput, error rates, AIOSS chain growth. All data is local — no Datadog, no Grafana Cloud, no external metrics pipeline.

## L2 General
L2 General: every Anticloud deployment benefits from the same analytics without configuration. The same dashboards work for a hospital's clinical AI usage and a robotics lab's motion planning telemetry.

## PAX Integration
PAX 27B is invoked for anomaly detection in telemetry streams: identifying unusual latency patterns, unexpected error rate spikes, or AIOSS chain integrity anomalies.

## AIOSS Audit Relevance
Every telemetry snapshot (metrics hash + time window + anomaly flag) is AIOSS-chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Tamper-evident, offline-verifiable, zero cloud dependency.

## Regulatory / Compliance
GDPR Art. 5 (data minimisation, local only), ISO 27001 A.12.4 (event logging)
