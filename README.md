# TrustedRouter Provider for Agent Zero

Adds [TrustedRouter](https://trustedrouter.com) as a chat and embedding provider in
Agent Zero. TrustedRouter is an OpenAI-compatible router: one API key reaches models
from OpenAI, Anthropic, Google, DeepSeek, and others, with per-request routing and
failover.

## Install

Install from the Agent Zero plugin index, or manually:

```bash
mkdir -p usr/plugins/trustedrouter_provider
git clone https://github.com/Lore-Hex/agent-zero-trustedrouter.git usr/plugins/trustedrouter_provider
```

Then set your key — get one at [trustedrouter.com/keys](https://trustedrouter.com/keys):

```bash
TRUSTEDROUTER_API_KEY=sk-tr-...
```

Provider ID `trustedrouter` maps to `TRUSTEDROUTER_API_KEY` automatically.

## Use

**Settings → Model Configuration → Chat Provider → TrustedRouter**, then pick a model.

Model ids are namespaced — `anthropic/claude-opus-4-7`, `openai/gpt-5.4-mini`. A bare
`gpt-5.4-mini` is rejected.

Ids under `trustedrouter/` are routing policies rather than single models. Each picks an
upstream per request and fails over if one is down:

| Model | Picks for |
| ----- | --------- |
| `trustedrouter/auto` | Capability, with failover |
| `trustedrouter/fast` | Latency |
| `trustedrouter/cheap` | Cost |
| `trustedrouter/zdr` | Providers that retain no data |
| `trustedrouter/e2e` | Confidential-compute providers |
| `trustedrouter/eu` | EU-hosted providers |

`zdr`, `e2e`, and `eu` are the useful ones for an agent that reads private files or
internal systems: they constrain which upstreams may serve the request at all, so the
constraint holds even as routing changes underneath.

Embeddings work the same way — select **TrustedRouter** as the embedding provider and
use an id such as `openai/text-embedding-3-large` or `cohere/embed-v4.0`.

The full catalog is at
[api.trustedrouter.com/v1/models](https://api.trustedrouter.com/v1/models).

## License

MIT
