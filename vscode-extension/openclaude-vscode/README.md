# Extensão OpenClaude para VS Code

Um companheiro prático do VS Code para o OpenClaude com um **Centro de Controle** (Control Center) sensível ao projeto, comportamento previsível de inicialização do terminal e acesso rápido a fluxos de trabalho úteis do OpenClaude.

## Recursos

- **Status real do Centro de Controle** na Barra de Atividades (Activity Bar):
  - se o comando `openclaude` configurado está instalado
  - o comando de inicialização sendo usado
  - se o shim de inicialização injeta `CLAUDE_CODE_USE_OPENAI=1`
  - a pasta da área de trabalho (workspace) atual
  - o diretório de trabalho atual (cwd) de inicialização que será usado para as sessões do terminal
  - se `.openclaude-profile.json` existe na raiz da área de trabalho atual
  - um resumo conservador do provedor derivado do perfil da área de trabalho ou de flags de ambiente conhecidas
- **Comportamento de inicialização sensível ao projeto**:
  - `Launch OpenClaude` é iniciado a partir da área de trabalho do editor ativo, quando possível
  - recorre à primeira pasta da área de trabalho quando necessário
  - evita iniciar a partir de um diretório de trabalho padrão arbitrário quando um projeto está aberto
- **Ações práticas na barra lateral**:
  - Launch OpenClaude (Iniciar OpenClaude)
  - Launch in Workspace Root (Iniciar na Raiz da Área de Trabalho)
  - Open Workspace Profile (Abrir Perfil da Área de Trabalho)
  - Open Repository (Abrir Repositório)
  - Open Setup Guide (Abrir Guia de Configuração)
  - Open Command Palette (Abrir Paleta de Comandos)
- **Tema escuro integrado**: `OpenClaude Terminal Black`

## Requisitos

- VS Code `1.95+`
- `openclaude` disponível no PATH do seu terminal (`npm install -g @gitlawb/openclaude`)

## Comandos

- `OpenClaude: Open Control Center`
- `OpenClaude: Launch in Terminal`
- `OpenClaude: Launch in Workspace Root`
- `OpenClaude: Open Repository`
- `OpenClaude: Open Setup Guide`
- `OpenClaude: Open Workspace Profile`

## Configurações

- `openclaude.launchCommand` (padrão: `openclaude`)
- `openclaude.terminalName` (padrão: `OpenClaude`)
- `openclaude.useOpenAIShim` (padrão: `false`)

`openclaude.useOpenAIShim` apenas injeta `CLAUDE_CODE_USE_OPENAI=1` nos terminais iniciados pela extensão. Ele não adivinha ou configura um provedor por si próprio.

## Notas sobre a Detecção de Status

- O status do provedor prefere o arquivo `.openclaude-profile.json` real da área de trabalho, quando presente.
- Se não houver nenhum perfil salvo, a extensão recorre a flags de ambiente conhecidas, disponíveis para o host de extensão do VS Code.
- Se a fonte da verdade não for clara, a extensão mostra `unknown` (desconhecido) em vez de adivinhar.

## Desenvolvimento

A partir desta pasta:

```bash
npm run test
npm run lint
```

Para empacotar (opcional):

```bash
npm run package
```
