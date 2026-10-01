---
name: melhorenvio-mcp-server
description: Configura, conecta ou diagnostica o servidor MCP oficial do Melhor Envio em clientes compatíveis, usando OAuth Bearer e transporte Streamable HTTP.
---

# Melhor Envio — servidor MCP oficial

Use esta skill quando a tarefa for conectar um agente ao MCP do Melhor Envio. Para implementar ou revisar a API REST e a lógica de fretes, use [melhorenvio-shipping](../melhorenvio-shipping/SKILL.md).

Fonte oficial: [MCP Server — Melhor Envio](https://docs.melhorenvio.com.br/docs/mcp-server-melhor-envio.md). URL do servidor: <https://docs.melhorenvio.com.br/mcp>. A página declara transporte MCP Streamable HTTP e autorização pelo mesmo token OAuth da API REST.

## Conexão

1. Verifique que o cliente suporta servidores MCP remotos por Streamable HTTP. Consulte a configuração específica desse cliente.
2. Obtenha token OAuth da conta e ambiente corretos, com os scopes necessários. Mantenha o token fora do repositório e de prompts.
3. Configure o header Authorization com esquema Bearer na conexão. Não exponha valor em logs, exemplos ou saídas de diagnóstico.
4. Recarregue o cliente e consulte a lista real de ferramentas do servidor. Não presuma nomes, quantidade nem schemas estáveis sem tools/list.
5. Teste primeiro uma consulta. Para comprar frete, adicionar saldo, gerar ou cancelar etiqueta, verifique autorização do usuário e confirme o resultado real no provedor.

Se falhar, confira URL, transporte, validade do token, ambiente, scopes e a resposta do servidor sem imprimir credenciais. O MCP não substitui a conferência de dados do pedido, estado de pagamento, documentos fiscais e etiqueta.

Leia [notas de configuração](references/configuracao.md) para limites e fontes.
