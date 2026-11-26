# DeepEval Evaluation

N-run output determinism for `validate_gemara_artifact`. For each of the nine
tool scenarios in `corpus/scenarios.yaml` (tc-001 to tc-009), the test calls the
tool 20 times with the same artifact and fails if any response differs.

## Setup

```bash
pip install -r requirements.txt
```

## Run

```bash
# From this directory
python -m pytest test_output_determinism.py -v

# Or from repo root
make eval-deepeval
```

## Server Connection

The test connects to a live gemara-mcp server via MCP stdio transport using
`GemaraMCPClient` from `eval/shared/mcp_client.py`. It spawns one Docker
container and keeps the connection open for all scenarios.

Configure via environment variables:

- `GEMARA_MCP_IMAGE` — Docker image (default: `ghcr.io/gemaraproj/gemara-mcp:v0.1.0`)
- `CONTAINER_RUNTIME` — Container runtime (default: `docker`)

## Dependencies

- **Docker**: Required for MCP server connection

No LLM is involved: the test compares raw tool output, so it needs no Ollama
and no GEval scoring.

## Test files

- `test_output_determinism.py` — Are `validate_gemara_artifact` results identical across runs?
- `conftest.py` — Shared fixtures: corpus loading
