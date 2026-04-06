# OpenClaude para Usuários Não Técnicos

Este guia é para pessoas que desejam o caminho de configuração mais fácil.

Você não precisa compilar a partir do código-fonte. Você não precisa do Bun. Você não precisa entender toda a base de código.

Se você consegue copiar e colar comandos em um terminal, você consegue configurar isso.

## O Que o OpenClaude Faz

O OpenClaude permite que você use um assistente de codificação de IA com diferentes provedores de modelo, como:

- OpenAI
- DeepSeek
- Gemini
- Ollama
- Codex

Para a maioria dos usuários de primeira viagem, o OpenAI é a opção mais fácil.

## Antes de Começar

Você precisa de:

1. Node.js 20 ou mais recente instalado
2. Uma janela de terminal
3. Uma chave de API do seu provedor, a menos que você esteja usando um modelo local como o Ollama

## Caminho Mais Rápido

1. Instale o OpenClaude com o npm
2. Defina 3 variáveis de ambiente
3. Execute `openclaude`

## Escolha Seu Sistema Operacional

- Windows: [Guia de Início Rápido para Windows](quick-start-windows.md)
- macOS / Linux: [Guia de Início Rápido para macOS / Linux](quick-start-mac-linux.md)

## Qual Provedor Você Deve Escolher?

### OpenAI

Escolha este se:

- você quer a configuração mais fácil
- você já tem uma chave de API da OpenAI

### Ollama

Escolha este se:

- você quer executar modelos localmente
- você não quer depender de uma API na nuvem para testes

### Codex

Escolha este se:

- você já usa o Codex CLI
- você já tem a autenticação do Codex ou do ChatGPT configurada

## O Que é Considerado Sucesso

Depois que você executar o `openclaude`, a CLI deve iniciar e aguardar o seu prompt.

Nesse ponto, você pode pedir para ele:

- explicar o código
- editar arquivos
- executar comandos
- revisar alterações

## Problemas Comuns

### Comando `openclaude` não encontrado

Causa:

- o npm instalou o pacote, mas o seu terminal ainda não foi atualizado

Correção:

1. Feche o terminal
2. Abra um novo terminal
3. Execute `openclaude` novamente

### Chave de API inválida

Causa:

- a chave está errada, expirada ou copiada incorretamente

Correção:

1. Obtenha uma nova chave do seu provedor
2. Cole-a novamente com cuidado
3. Execute `openclaude` novamente

### Ollama não está funcionando

Causa:

- o Ollama não está instalado ou não está em execução

Correção:

1. Instale o Ollama em `https://ollama.com/download`
2. Inicie o Ollama
3. Tente novamente

## Quer Mais Controle?

Se você quiser compilações de origem, perfis de provedor avançados, diagnósticos ou fluxos de trabalho baseados no Bun, use:

- [Configuração Avançada](advanced-setup.md)
