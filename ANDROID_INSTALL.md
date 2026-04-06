# OpenClaude no Android (Termux)

Guia completo para executar OpenClaude no Android usando Termux + Ubuntu proot.

---

## Pré-requisitos

- Telefone Android com ~700MB de armazenamento livre
- [Termux](https://f-droid.org/en/packages/com.termux/) instalado do **F-Droid** (não do Play Store)
- Uma chave [API OpenRouter](https://openrouter.ai) (gratuita, sem necessidade de cartão de crédito)

---

## Por que este setup?

OpenClaude requer [Bun](https://bun.sh) para compilar, e o Bun não tem suporte nativo para Android. A solução é executar um ambiente real de Ubuntu dentro do Termux via `proot-distro`, onde o binário Linux do Bun funciona corretamente.

---

## Instalação

### Passo 1 — Atualizar o Termux

```bash
pkg update && pkg upgrade
```

Pressione `N` ou Enter para qualquer conflito de arquivo de configuração.

### Passo 2 — Instalar dependências

```bash
pkg install nodejs-lts git proot-distro
```

Verifique a versão do Node.js:

```bash
node --version  # deve ser v20+
```

### Passo 3 — Clonar OpenClaude

```bash
git clone https://github.com/Gitlawb/openclaude.git
cd openclaude
npm install
npm link
```

### Passo 4 — Instalar Ubuntu via proot

```bash
proot-distro install ubuntu
```

Isso baixa ~200–400MB. Aguarde até completar.

### Passo 5 — Instalar Bun dentro do Ubuntu

```bash
proot-distro login ubuntu
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
bun --version  # deve mostrar 1.3.11+
```

### Passo 6 — Compilar OpenClaude

```bash
cd /data/data/com.termux/files/home/openclaude
bun run build
```

Você deve ver:

```
✓ Compilado openclaude v0.1.6 → dist/cli.mjs
```

### Passo 7 — Salvar variáveis de ambiente permanentemente

Ainda dentro do Ubuntu, adicione sua configuração OpenRouter para `.bashrc`:

```bash
echo 'export CLAUDE_CODE_USE_OPENAI=1' >> ~/.bashrc
echo 'export OPENAI_API_KEY=sua_chave_openrouter_aqui' >> ~/.bashrc
echo 'export OPENAI_BASE_URL=https://openrouter.ai/api/v1' >> ~/.bashrc
echo 'export OPENAI_MODEL=qwen/qwen3.6-plus-preview:free' >> ~/.bashrc
source ~/.bashrc
```

Substitua `sua_chave_openrouter_aqui` pela sua chave real em [openrouter.ai/keys](https://openrouter.ai/keys).

### Passo 8 — Executar OpenClaude

```bash
node dist/cli.mjs
```

Selecione **3** (plataforma 3º) na tela de login. Suas variáveis de ambiente serão detectadas automaticamente.

---

## Reiniciando após fechar o Termux

Cada vez que você reabrir o Termux após fechar, execute:

```bash
proot-distro login ubuntu
cd /data/data/com.termux/files/home/openclaude
node dist/cli.mjs
```

---

## Modelo gratuito recomendado

**`qwen/qwen3.6-plus-preview:free`** — Melhor modelo gratuito no OpenRouter até abril de 2026.

- Janela de contexto de 1M tokens
- Superia o Claude 4.5 Opus no Terminal-Bench 2.0 para codificação agente (61.6 vs 59.3)
- Raciocínio em cadeia de pensamento integrado
- Uso nativo de ferramentas e chamadas de função
- $0/M tokens (período de pré-venda)

> ⚠️ O status gratuito pode mudar quando o período de pré-venda terminar. Verifique [openrouter.ai](https://openrouter.ai/qwen/qwen3.6-plus-preview:free) para preços atuais.

---

## Modelos gratuitos alternativos (OpenRouter)

| ID do Modelo                             | Contexto | Observações                                          |
| ---------------------------------------- | -------- | ---------------------------------------------------- |
| `qwen/qwen3-coder:free`                  | 262K     | Melhor para tarefas puras de código                  |
| `openai/gpt-oss-120b:free`               | 131K     | Modelo aberto da OpenAI, forte em chamadas de função |
| `nvidia/nemotron-3-super-120b-a12b:free` | 262K     | Modelo híbrido MoE, bom para uso geral               |
| `meta-llama/llama-3.3-70b-instruct:free` | 66K      | Reliável, amplamente testado                         |

Mude os modelos a qualquer momento:

```bash
export OPENAI_MODEL=qwen/qwen3-coder:free
node dist/cli.mjs
```

---

## Por que não Groq ou Cerebras?

Ambos foram testados e falham devido ao prompt de sistema grande do OpenClaude (~50K tokens):

- **Groq free tier**: Limites de taxa de requisição (TPM) muito baixos (6K–12K tokens/min)
- **Cerebras free tier**: Limites de TPM excedidos, mesmo com `llama3.1-8b`

Modelos gratuitos do OpenRouter não têm limites de TPM — apenas 20 requisições/min e 200 requisições/dia.

---

## Dicas

- **Não deslize o Termux para fora das recentes durante a sessão** — use o botão de home para minimizar em vez disso.
- O ambiente Ubuntu persiste entre sessões do Termux; sua compilação e configuração são salvos.
- Execute `bun run build` novamente apenas se você atualizar o repositório do OpenClaude.
