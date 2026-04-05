# OpenClaude

O OpenClaude é uma CLI de coding-agent open-source para provedores de modelos em cloud e locais.

Use APIs OpenAI-compatible, Gemini, GitHub Models, Codex, Ollama, Atomic Chat e outros backends suportados mantendo um fluxo de trabalho terminal-first: prompts, tools, agents, MCP, slash commands e streaming output.

[![PR Checks](https://github.com/Gitlawb/openclaude/actions/workflows/pr-checks.yml/badge.svg?branch=main)](https://github.com/Gitlawb/openclaude/actions/workflows/pr-checks.yml)
[![Release](https://img.shields.io/github/v/tag/Gitlawb/openclaude?label=release&color=0ea5e9)](https://github.com/Gitlawb/openclaude/tags)
[![Discussions](https://img.shields.io/badge/discussions-open-7c3aed)](https://github.com/Gitlawb/openclaude/discussions)
[![Security Policy](https://img.shields.io/badge/security-policy-0f766e)](SECURITY.md)
[![License](https://img.shields.io/badge/license-MIT-2563eb)](LICENSE)

[Quick Start](#quick-start) | [Guias de Setup](#guias-de-setup) | [Provedores Suportados](#provedores-suportados) | [Source Build e Desenvolvimento Local](#source-build-e-desenvolvimento-local) | [Extensão VS Code](#extensão-vs-code) | [Comunidade (Community)](#comunidade-community)

## Por que OpenClaude

- Use uma única CLI entre cloud APIs e backends de modelos locais
- Salve provider profiles dentro do app com `/provider`
- Execute com serviços OpenAI-compatible, Gemini, GitHub Models, Codex, Ollama, Atomic Chat e outros provedores suportados
- Mantenha os workflows de coding-agent em um só lugar: bash, file tools, grep, glob, agents, tasks, MCP e web tools
- Use a extensão VS Code incluída para integração de launch e suporte a temas

## Quick Start

### Instalação

```bash
npm install -g @gitlawb/openclaude
```

Se a instalação relatar posteriormente `ripgrep not found`, instale o ripgrep em todo o sistema (system-wide) e confirme se `rg --version` funciona no mesmo terminal antes de iniciar o OpenClaude.

### Iniciar

```bash
openclaude
```

Dentro do OpenClaude:

- execute `/provider` para um setup guiado do provedor e perfis salvos
- execute `/onboard-github` para o onboarding do GitHub Models

### Setup mais rápido para OpenAI

macOS / Linux:

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_API_KEY=sk-your-key-here
export OPENAI_MODEL=gpt-4o

openclaude
```

Windows PowerShell:

```powershell
$env:CLAUDE_CODE_USE_OPENAI="1"
$env:OPENAI_API_KEY="sk-your-key-here"
$env:OPENAI_MODEL="gpt-4o"

openclaude
```

### Setup local mais rápido para Ollama

macOS / Linux:

```bash
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_BASE_URL=http://localhost:11434/v1
export OPENAI_MODEL=qwen2.5-coder:7b

openclaude
```

Windows PowerShell:

```powershell
$env:CLAUDE_CODE_USE_OPENAI="1"
$env:OPENAI_BASE_URL="http://localhost:11434/v1"
$env:OPENAI_MODEL="qwen2.5-coder:7b"

openclaude
```

## Guias de Setup

Guias amigáveis para iniciantes (beginner-friendly):

- [Setup Não-Técnico (Non-Technical Setup)](docs/non-technical-setup.md)
- [Quick Start Windows](docs/quick-start-windows.md)
- [Quick Start macOS / Linux](docs/quick-start-mac-linux.md)

Guias avançados e de source-build:

- [Setup Avançado (Advanced Setup)](docs/advanced-setup.md)
- [Instalação Android (Android Install)](ANDROID_INSTALL.md)

## Provedores Suportados

| Provider                   | Setup Path              | Notes                                                                                                 |
| -------------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------- |
| OpenAI-compatible          | `/provider` ou env vars | Funciona com OpenAI, OpenRouter, DeepSeek, Groq, Mistral, LM Studio e outros servidores `/v1` compatíveis |
| Gemini                     | `/provider` ou env vars | Suporta API key, access token ou workflow ADC local na `main` atual                                 |
| GitHub Models              | `/onboard-github`       | Onboarding interativo com credenciais salvas                                                        |
| Codex                      | `/provider`             | Usa credenciais Codex existentes quando disponíveis                                                   |
| Ollama                     | `/provider` ou env vars | Inferência local (local inference) sem API key                                                      |
| Atomic Chat                | setup avançado          | Backend local Apple Silicon                                                                           |
| Bedrock / Vertex / Foundry | env vars                | Integrações de provedores adicionais para ambientes suportados                                        |

## O Que Funciona

- **Workflows de coding baseados em tools**: Bash, file read/write/edit, grep, glob, agents, tasks, MCP e slash commands
- **Respostas em streaming (Streaming responses)**: Saída de tokens em tempo real e progresso das tools
- **Chamada de tools (Tool calling)**: Loops de tools de múltiplas etapas com chamadas de modelo, execução de tools e respostas de acompanhamento
- **Imagens**: Inputs de URL e imagens em base64 para provedores que suportam visão (vision)
- **Perfis de provedor (Provider profiles)**: Setup guiado mais suporte a perfis `.openclaude-profile.json` salvos
- **Backends de modelos locais e remotos**: Cloud APIs, servidores locais e inferência local Apple Silicon

## Notas do Provedor

O OpenClaude suporta vários provedores, mas o comportamento não é idêntico em todos eles.

- Recursos específicos da Anthropic podem não existir em outros provedores
- A qualidade da tool depende fortemente do modelo selecionado
- Modelos locais menores podem ter dificuldades com fluxos de tools longos de múltiplas etapas
- Alguns provedores impõem limites de saída (output caps) mais baixos do que os padrões da CLI, e o OpenClaude se adapta onde possível

Para melhores resultados, use modelos com forte suporte a chamadas de tools/funções (tool/function calling).

## Roteamento de Agentes (Agent Routing)

O OpenClaude pode rotear diferentes agentes para diferentes modelos através de um roteamento baseado em configurações (settings-based routing). Isso é útil para otimização de custos ou para dividir o trabalho com base na capacidade do modelo.

Adicione ao `~/.claude/settings.json`:

```json
{
  "agentModels": {
    "deepseek-chat": {
      "base_url": "https://api.deepseek.com/v1",
      "api_key": "sk-your-key"
    },
    "gpt-4o": {
      "base_url": "https://api.openai.com/v1",
      "api_key": "sk-your-key"
    }
  },
  "agentRouting": {
    "Explore": "deepseek-chat",
    "Plan": "gpt-4o",
    "general-purpose": "gpt-4o",
    "frontend-dev": "deepseek-chat",
    "default": "gpt-4o"
  }
}
```

Quando nenhuma correspondência de roteamento é encontrada, o provedor global permanece como fallback.

> **Nota:** Os valores de `api_key` em `settings.json` são armazenados em texto plano (plaintext). Mantenha este arquivo privado e não o envie para o controle de versão (version control).

## Busca e Coleta Web (Web Search and Fetch)

Por padrão, o `WebSearch` funciona em modelos não-Anthropic usando DuckDuckGo. Isso dá ao GPT-4o, DeepSeek, Gemini, Ollama e outros provedores OpenAI-compatible um caminho de web search gratuito e pronto para uso.

> **Nota:** O fallback do DuckDuckGo funciona fazendo scraping de resultados de busca e pode sofrer rate-limit, ser bloqueado ou estar sujeito aos Termos de Serviço do DuckDuckGo. Se quiser uma opção suportada mais confiável, configure o Firecrawl.

Para backends nativos da Anthropic e respostas do Codex, o OpenClaude mantém o comportamento nativo de web search do provedor.

O `WebFetch` funciona, mas seu fluxo básico de HTTP e conversão de HTML-para-markdown ainda pode falhar em sites renderizados por JavaScript ou sites que bloqueiam requests HTTP simples.

Configure uma API key do Firecrawl se você quiser o comportamento de search/fetch provido pelo Firecrawl:

```bash
export FIRECRAWL_API_KEY=your-key-here
```

Com o Firecrawl ativado:

- `WebSearch` pode usar a API de busca do Firecrawl enquanto o DuckDuckGo permanece como o caminho gratuito padrão para modelos não-Claude
- `WebFetch` usa o endpoint de scrape do Firecrawl ao invés de HTTP puro, lidando corretamente com páginas renderizadas por JS

O plano gratuito (Free tier) em [firecrawl.dev](https://firecrawl.dev) inclui 500 créditos. A chave (key) é opcional.

## Source Build e Desenvolvimento Local

```bash
bun install
bun run build
node dist/cli.mjs
```

Comandos úteis:

- `bun run dev`
- `bun test`
- `bun run test:coverage`
- `bun run security:pr-scan -- --base origin/main`
- `bun run smoke`
- `bun run doctor:runtime`
- `bun run verify:privacy`
- execuções focadas de `bun test ...` para as áreas que você alterar

## Testes e Cobertura (Testing And Coverage)

O OpenClaude usa o test runner integrado do Bun para testes unitários (unit tests).

Execute a suíte completa de unidades (full unit suite):

```bash
bun test
```

Gere a cobertura de testes unitários (unit test coverage):

```bash
bun run test:coverage
```

Abra o relatório visual de cobertura (visual coverage report):

```bash
open coverage/index.html
```

Se você já possui `coverage/lcov.info` e quer apenas reconstruir a UI:

```bash
bun run test:coverage:ui
```

Use execuções de teste focadas quando você alterar apenas uma área:

- `bun run test:provider`
- `bun run test:provider-recommendation`
- `bun test path/to/file.test.ts`

Validação recomendada para contribuidores antes de abrir um PR:

- `bun run build`
- `bun run smoke`
- `bun run test:coverage` para uma cobertura unitária mais ampla quando sua mudança afetar runtime compartilhado ou lógica de provedor
- execuções focadas de `bun test ...` para os arquivos e fluxos que você alterou

A saída de cobertura (coverage output) é escrita em `coverage/lcov.info`, e o OpenClaude também gera um heatmap no estilo de atividade do git em `coverage/index.html`.

## Estrutura do Repositório (Repository Structure)

- `src/` - core CLI/runtime
- `scripts/` - scripts de build, verificação e manutenção
- `docs/` - documentação de setup, contribuição e projeto
- `python/` - helpers Python independentes (standalone) e seus testes
- `vscode-extension/openclaude-vscode/` - Extensão VS Code
- `.github/` - automação do repo, templates e configuração de CI
- `bin/` - entrypoints do launcher da CLI

## Extensão VS Code

O repo inclui uma extensão VS Code em [`vscode-extension/openclaude-vscode`](vscode-extension/openclaude-vscode) para integração de launch do OpenClaude, interface de control-center ciente do provedor e suporte a temas.

## Segurança (Security)

Se você acredita ter encontrado um problema de segurança, veja [SECURITY.md](SECURITY.md).

## Comunidade (Community)

- Use [GitHub Discussions](https://github.com/Gitlawb/openclaude/discussions) para perguntas e respostas (Q&A), ideias e conversas da comunidade
- Use [GitHub Issues](https://github.com/Gitlawb/openclaude/issues) para bugs confirmados e trabalhos acionáveis em features

## Contribuindo (Contributing)

Contribuições são bem-vindas.

Para mudanças maiores, abra uma issue primeiro para que o escopo fique claro antes da implementação. Comandos úteis de validação incluem:

- `bun run build`
- `bun run test:coverage`
- `bun run smoke`
- execuções focadas de `bun test ...` para as áreas alteradas

## Aviso Legal (Disclaimer)

O OpenClaude é um projeto comunitário independente e não é afiliado, endossado ou patrocinado pela Anthropic.

O OpenClaude se originou da base de código do Claude Code e, desde então, foi substancialmente modificado para suportar múltiplos provedores e uso aberto. "Claude" e "Claude Code" são marcas registradas da Anthropic PBC. Veja a licença ([LICENSE](LICENSE)) para mais detalhes.

## Licença (License)

Veja a licença ([LICENSE](LICENSE)).
