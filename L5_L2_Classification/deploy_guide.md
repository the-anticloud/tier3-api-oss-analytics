# Deploy Guide — api-oss-analytics
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, SQLite, pandas, Prometheus (local), AIOSS_FORMAT

## Prerequisites
Python 3.11+, pandas 2.2+, SQLite (stdlib), prometheus_client 0.20+

## AIOSS Integration
```bash
aioss init --module api-oss-analytics --output ./api_oss_analytics.aioss
aioss append --chain ./api_oss_analytics.aioss --payload ./output.bin --module api-oss-analytics
aioss verify --chain ./api_oss_analytics.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="api-oss-analytics",
    aioss_chain="./api_oss_analytics.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./api_oss_analytics.aioss --verbose
python -m api_oss_analytics.tests.smoke
```
