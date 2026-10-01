# Conexão ao MCP do Melhor Envio

- URL documentada: https://docs.melhorenvio.com.br/mcp
- Transporte documentado: Streamable HTTP
- Autenticação documentada: Authorization Bearer com access token OAuth do Melhor Envio
- Fonte: [MCP Server — Melhor Envio](https://docs.melhorenvio.com.br/docs/mcp-server-melhor-envio.md), consultada em 01/10/2026.

A página mostra clientes configurados por arquivo JSON, mas caminho e formato dependem do cliente. Siga a documentação atual do cliente. Reinicie ou recarregue e confirme a conexão pelo inventário de ferramentas retornado pelo servidor.

O access token tem validade documentada de 30 dias e o refresh token de 45 dias. O MCP usa as permissões da conta e do aplicativo autorizados. Sandbox e produção possuem dados e credenciais separados; confira o destino de cada ferramenta antes de executar uma ação.

Para diagnosticar: confirme conectividade HTTPS, transporte Streamable HTTP, token válido, scopes e resposta de tools/list. Não faça testes de escrita em produção para provar somente a conexão. Não registre tokens ou cabeçalhos de autenticação em logs.

Fontes adicionais: [autenticação](https://docs.melhorenvio.com.br/docs/autenticacao-1.md), [ambiente Sandbox](https://docs.melhorenvio.com.br/docs/sandbox.md).
