# Developer Cookbook — api-oss-analytics
**Stack:** Python 3.11, SQLite, pandas, Prometheus (local), AIOSS_FORMAT
**Domain:** Sovereign analytics and telemetry for Anticloud API usage — all data stays local
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```python
from api_oss_analytics import AnalyticsCollector, Dashboard

collector = AnalyticsCollector("./analytics.db", aioss_chain="./analytics.aioss")

# Record an inference event
collector.record_inference(
    module="PAX_INFERENCE_CORE",
    latency_ms=508.3,
    tokens=142,
    chain_hash="8b4a8a4f..."
)

# Query stats
stats = collector.query(window="1h", module="PAX_INFERENCE_CORE")
print(f"P50: {stats.p50_latency_ms:.0f}ms, P99: {stats.p99_latency_ms:.0f}ms")
print(f"Throughput: {stats.avg_tokens_per_sec:.1f} tok/s")

# Prometheus metrics endpoint
from prometheus_client import start_http_server
start_http_server(9090)  # curl localhost:9090/metrics
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every api-oss-analytics output:
chain_hash = aioss_append("./api_oss_analytics.aioss",
                           result_bytes, "api-oss-analytics")
```

## Performance & Integration

SQLite WAL mode for concurrent writes. Pre-aggregate hourly stats to avoid full-table scans. Prometheus scrape interval: 15s. Integration: receives events from api-oss-logging, ai-oss-gateway, PAX_MONITOR (T2). Feeds dashboard-analytics.
