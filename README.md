# melhorenvio-skills

Skills de agente para a API e o MCP oficiais do Melhor Envio: OAuth, cotação, carrinho, compra de fretes, etiquetas, rastreio, webhooks e diagnóstico de conexão.

Este repositório segue o formato de [mercadopago-skills](https://github.com/fabioKoruja00/mercadopago-skills): cada skill tem um SKILL.md curto para descoberta e referências lidas conforme a tarefa.

## Conteúdo

| Skill | Quando usar |
|---|---|
| [melhorenvio-shipping](melhorenvio-shipping/SKILL.md) | Implementar ou revisar integração REST de fretes e logística reversa |
| [melhorenvio-mcp-server](melhorenvio-mcp-server/SKILL.md) | Configurar ou diagnosticar o servidor MCP oficial Streamable HTTP |

As referências registram as URLs oficiais para conferir payloads e regras atuais. O índice de toda a documentação fica em <https://docs.melhorenvio.com.br/llms.txt>.

## Instalação global

Copie as duas pastas para o diretório de skills do agente, fora de qualquer repositório de projeto. No Codex, use o diretório de usuário ~/.codex/skills; no Claude Code, ~/.claude/skills. O suporte à descoberta automática depende do cliente.

Exemplo em um shell com Git e cópia de diretórios:

    git clone https://github.com/fabioKoruja00/melhorenvio-skills.git
    cp -r melhorenvio-skills/melhorenvio-shipping ~/.codex/skills/
    cp -r melhorenvio-skills/melhorenvio-mcp-server ~/.codex/skills/

O clone só é necessário quando você não tiver os arquivos instalados. O repositório é privado; é preciso acesso autorizado ao GitHub.

## Validade das informações

As páginas oficiais foram consultadas em 01/10/2026. A documentação contém uma divergência sobre expiração de itens no carrinho (7 ou 20 dias), registrada na skill. Antes de usar uma regra sujeita a mudança ou executar ação financeira, abra a fonte oficial atual e teste no ambiente apropriado.

Nenhum token, client secret, credencial ou dado de conta está incluído aqui. Este repositório não tem vínculo com o Melhor Envio.

## Licença

MIT — veja [LICENSE](LICENSE).
