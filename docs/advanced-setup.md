# Configuração Avançada do OpenClaude

Este guia é para usuários que desejam compilar a partir do código-fonte, fluxos de trabalho do Bun, perfis de provedor, diagnósticos ou mais controle sobre o comportamento em tempo de execução.

## Opções de Instalação

### Opção A: npm

```bash
npm install -g @gitlawb/openclaude
```

### Opção B: A partir do código-fonte com Bun

Use o Bun `1.3.11` ou mais recente para compilações a partir do código-fonte no Windows. Versões mais antigas do Bun podem falhar durante o `bun run build`.

```bash
git clone https://node.gitlawb.com/z6MkqDnb7Siv3Cwj7pGJq4T5EsUisECqR8KpnDLwcaZq5TPr/openclaude.git
cd openclaude

bun install
bun run build
npm link
```

### Opção C: Executar diretamente com Bun

```bash
git clone https://node.gitlawb.com/z6MkqDnb7Siv3Cwj7pGJq4T5EsUisECqR8KpnDLwcaZq5TPr/openclaude.git
cd openclaude

bun install
bun run dev
```

## Exemplos de Provedores

### OpenAI

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=sk-...
export OPENAI_MODEL=gpt-4o
```

### Codex via autenticação do ChatGPT

`codexplan` mapeia para o GPT-5.4 no backend do Codex com alto raciocínio.
`codexspark` mapeia para o GPT-5.3 Codex Spark para iterações mais rápidas.

Se você já usa a CLI do Codex, o OpenClaude lê o `~/.codex/auth.json` automaticamente. Você também pode apontá-lo para outro local com `CODEX_AUTH_JSON_PATH` ou substituir o token diretamente com `CODEX_API_KEY`.

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_MODEL=codexplan

# opcional se você ainda não tem ~/.codex/auth.json
export CODEX_API_KEY=...

openclaude
```

### DeepSeek

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=sk-...
export OPENAI_BASE_URL=https://api.deepseek.com/v1
export OPENAI_MODEL=deepseek-chat
```

### Google Gemini via OpenRouter

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=sk-or-...
export OPENAI_BASE_URL=https://openrouter.ai/api/v1
export OPENAI_MODEL=google/gemini-2.0-flash-001
```

A disponibilidade de modelos no OpenRouter muda com o tempo. Se um modelo parar de funcionar, tente outro modelo atual do OpenRouter antes de presumir que a integração está quebrada.

### Ollama

```bash
ollama pull llama3.3:70b

export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_BASE_URL=http://localhost:11434/v1
export OPENAI_MODEL=llama3.3:70b
```

### Atomic Chat (local, Apple Silicon)

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_BASE_URL=http://127.0.0.1:1337/v1
export OPENAI_MODEL=your-model-name
```

Nenhuma chave de API é necessária para os modelos locais do Atomic Chat.

Ou use o inicializador de perfil:

```bash
bun run dev:atomic-chat
```

Baixe o Atomic Chat em [atomic.chat](https://atomic.chat/). O aplicativo deve estar em execução com um modelo carregado antes de ser iniciado.

### LM Studio

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_BASE_URL=http://localhost:1234/v1
export OPENAI_MODEL=your-model-name
```

### Together AI

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=...
export OPENAI_BASE_URL=https://api.together.xyz/v1
export OPENAI_MODEL=meta-llama/Llama-3.3-70B-Instruct-Turbo
```

### Groq

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=gsk_...
export OPENAI_BASE_URL=https://api.groq.com/openai/v1
export OPENAI_MODEL=llama-3.3-70b-versatile
```

### Mistral

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=...
export OPENAI_BASE_URL=https://api.mistral.ai/v1
export OPENAI_MODEL=mistral-large-latest
```

### Azure OpenAI

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=your-azure-key
export OPENAI_BASE_URL=https://your-resource.openai.azure.com/openai/deployments/your-deployment/v1
export OPENAI_MODEL=gpt-4o
```

## Variáveis de Ambiente

| Variável | Obrigatório | Descrição |
|----------|----------|-------------|
| `CLAUDE_CODE_USE_OPENAI` | Sim | Defina como `1` para habilitar o provedor OpenAI |
| `OPENAI_API_KEY` | Sim* | Sua chave de API (`*` não é necessário para modelos locais como Ollama ou Atomic Chat) |
| `OPENAI_MODEL` | Sim | Nome do modelo, como `gpt-4o`, `deepseek-chat` ou `llama3.3:70b` |
| `OPENAI_BASE_URL` | Não | Endpoint da API, o padrão é `https://api.openai.com/v1` |
| `CODEX_API_KEY` | Apenas Codex | Substituição do token de acesso do Codex ou ChatGPT |
| `CODEX_AUTH_JSON_PATH` | Apenas Codex | Caminho para um arquivo `auth.json` da CLI do Codex |
| `CODEX_HOME` | Apenas Codex | Diretório inicial alternativo do Codex |
| `OPENCLAUDE_DISABLE_CO_AUTHORED_BY` | Não | Suprime o sufixo padrão `Co-Authored-By` em commits do git gerados |

Você também pode usar `ANTHROPIC_MODEL` para substituir o nome do modelo. `OPENAI_MODEL` tem prioridade.

## Fortalecimento em Tempo de Execução (Runtime Hardening)

Use esses comandos para validar sua configuração e detectar erros antecipadamente:

```bash
# verificação rápida de sanidade na inicialização
bun run smoke

# validar ambiente do provedor + acessibilidade
bun run doctor:runtime

# imprimir diagnósticos de tempo de execução legíveis por máquina
bun run doctor:runtime:json

# persistir um relatório de diagnóstico em reports/doctor-runtime.json
bun run doctor:report

# verificação completa de fortalecimento local (smoke + runtime doctor)
bun run hardening:check

# fortalecimento rigoroso (inclui verificação de tipos em todo o projeto)
bun run hardening:strict
```

Notas:

- `doctor:runtime` falha rapidamente se `CLAUDE_CODE_USE_OPENAI=1` estiver com uma chave de espaço reservado ou com a chave ausente para provedores não locais.
- Provedores locais como `http://localhost:11434/v1`, `http://10.0.0.1:11434/v1` e `http://127.0.0.1:1337/v1` podem ser executados sem `OPENAI_API_KEY`.
- Perfis do Codex validam o `CODEX_API_KEY` ou o arquivo de autenticação da CLI do Codex e testam `POST /responses` em vez de `GET /models`.

## Perfis de Inicialização de Provedor

Use inicializadores de perfil para evitar a repetição da configuração do ambiente:

```bash
# inicialização (bootstrap) única de perfil (prefere o Ollama local viável, caso contrário, OpenAI)
bun run profile:init

# visualizar o melhor provedor/modelo para seu objetivo
bun run profile:recommend -- --goal coding --benchmark

# aplicar automaticamente o melhor provedor/modelo local/openai disponível para seu objetivo
bun run profile:auto -- --goal latency

# inicialização (bootstrap) do codex (o padrão é codexplan e ~/.codex/auth.json)
bun run profile:codex

# inicialização (bootstrap) do openai com chave explícita
bun run profile:init -- --provider openai --api-key sk-...

# inicialização (bootstrap) do ollama com modelo personalizado
bun run profile:init -- --provider ollama --model llama3.1:8b

# inicialização (bootstrap) do ollama com seleção automática inteligente de modelo
bun run profile:init -- --provider ollama --goal coding

# inicialização (bootstrap) do atomic-chat (detecta automaticamente o modelo em execução)
bun run profile:init -- --provider atomic-chat

# inicialização (bootstrap) do codex com um alias de modelo rápido
bun run profile:init -- --provider codex --model codexspark

# iniciar usando o perfil persistido (.openclaude-profile.json)
bun run dev:profile

# perfil codex (usa CODEX_API_KEY ou ~/.codex/auth.json)
bun run dev:codex

# perfil OpenAI (requer OPENAI_API_KEY no seu shell)
bun run dev:openai

# perfil Ollama (padrões: localhost:11434, llama3.1:8b)
bun run dev:ollama

# perfil Atomic Chat (LLMs locais em Apple Silicon em 127.0.0.1:1337)
bun run dev:atomic-chat
```

O `profile:recommend` classifica os modelos instalados no Ollama para `latency` (latência), `balanced` (equilibrado) ou `coding` (codificação), e o `profile:auto` pode persistir a recomendação diretamente.

Se nenhum perfil existir ainda, o `dev:profile` usa os mesmos padrões sensíveis a objetivos ao escolher o modelo inicial.

Use `--provider ollama` quando desejar um caminho apenas local. O modo automático volta para o OpenAI quando nenhum modelo de chat local viável está instalado.

Use `--provider atomic-chat` quando desejar o Atomic Chat como o provedor local do Apple Silicon.

Use `profile:codex` ou `--provider codex` quando desejar o backend do ChatGPT Codex.

`dev:openai`, `dev:ollama`, `dev:atomic-chat` e `dev:codex` executam o `doctor:runtime` primeiro e só iniciam o aplicativo se as verificações passarem.

Para o `dev:ollama`, certifique-se de que o Ollama esteja em execução localmente antes de iniciar.

Para o `dev:atomic-chat`, certifique-se de que o Atomic Chat esteja em execução com um modelo carregado antes de iniciar.
