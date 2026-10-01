# Mapa da API do Melhor Envio

Rotas confirmadas nas páginas oficiais consultadas em 01/10/2026. Abra cada página para conferir parâmetros e respostas. O [índice oficial](https://docs.melhorenvio.com.br/llms.txt) tem o catálogo completo e pode mudar.

## Carrinho, compra e etiqueta

| Ação | Método e caminho | Fonte |
|---|---|---|
| Cotar produtos ou volumes | Confira o caminho na referência atual; o guia descreve os dois modelos | [Cotação](https://docs.melhorenvio.com.br/docs/cotacao-de-fretes.md) |
| Inserir envio no carrinho | POST /api/v2/me/cart | [Inserir fretes](https://docs.melhorenvio.com.br/reference/inserir-fretes-no-carrinho.md) |
| Ler item do carrinho | GET /api/v2/me/cart/{id} | [Ler item](https://docs.melhorenvio.com.br/reference/exibir-informacoes-de-item-do-carrinho.md) |
| Remover item | DELETE /api/v2/me/cart/{id} | [Remover item](https://docs.melhorenvio.com.br/reference/remocao-de-itens-do-carrinho.md) |
| Comprar envios | POST /api/v2/me/shipment/checkout | [Checkout](https://docs.melhorenvio.com.br/reference/compra-de-fretes-1.md) |
| Adicionar saldo | POST /api/v2/me/balance | [Saldo](https://docs.melhorenvio.com.br/reference/inserir-saldo-na-carteira-do-usuario.md) |
| Pré-visualizar | POST /api/v2/me/shipment/preview | [Pré-visualização](https://docs.melhorenvio.com.br/reference/pre-visualizacao-de-etiquetas.md) |
| Gerar etiqueta | POST /api/v2/me/shipment/generate | [Geração](https://docs.melhorenvio.com.br/reference/geracao-de-etiquetas.md) |
| Imprimir link | POST /api/v2/me/shipment/print | [Impressão](https://docs.melhorenvio.com.br/reference/impressao-de-etiquetas.md) |
| Imprimir arquivo | GET /api/v2/me/imprimir/{arquivo}/{id} | [Arquivo](https://docs.melhorenvio.com.br/reference/impressao-de-etiquetas-em-arquivo.md) |
| Imprimir DACE | GET /api/v2/me/imprimir/dace/{arquivo}/{order_id} | [DACE](https://docs.melhorenvio.com.br/reference/impressao-dace.md) |
| Conferir cancelamento | POST /api/v2/me/shipment/cancellable | [Elegibilidade](https://docs.melhorenvio.com.br/reference/verificar-se-etiqueta-pode-ser-cancelada.md) |
| Cancelar | POST /api/v2/me/shipment/cancel | [Cancelamento](https://docs.melhorenvio.com.br/reference/cancelamento-de-etiquetas.md) |
| Rastrear | POST /api/v2/me/shipment/tracking | [Rastreio](https://docs.melhorenvio.com.br/reference/rastreio-de-envios.md) |

## Consulta e cadastro

| Ação | Método e caminho | Fonte |
|---|---|---|
| Listar envios | GET /api/v2/me/orders | [Etiquetas](https://docs.melhorenvio.com.br/reference/listar-etiquetas.md) |
| Consultar envio | GET /api/v2/me/orders/{id} | [Detalhes](https://docs.melhorenvio.com.br/reference/listar-informacoes-de-uma-etiqueta.md) |
| Pesquisar envio | GET /api/v2/me/orders/search | [Pesquisa](https://docs.melhorenvio.com.br/reference/pesquisar-etiqueta.md) |
| Transportadoras | GET /api/v2/me/shipment/companies e GET /api/v2/me/shipment/companies/{companyId} | [Lista](https://docs.melhorenvio.com.br/reference/listar-transportadoras.md), [detalhes](https://docs.melhorenvio.com.br/reference/listar-informacoes-de-uma-transportadora.md) |
| Serviços | GET /api/v2/me/shipment/services e GET /api/v2/me/shipment/services/{serviceId} | [Lista](https://docs.melhorenvio.com.br/reference/listar-servicos.md), [detalhes](https://docs.melhorenvio.com.br/reference/listar-informacoes-de-um-servico.md) |
| Agências | GET /api/v2/me/shipment/agencies e GET /api/v2/me/shipment/agencies/{agencyId} | [Lista](https://docs.melhorenvio.com.br/reference/listar-agencias-e-opcoes-de-filtro.md), [detalhes](https://docs.melhorenvio.com.br/reference/listar-informacoes-de-uma-agencia.md) |
| Cadastrar e consultar loja | POST /api/v2/me/companies e GET /api/v2/me/companies/{storeId} | [Cadastro](https://docs.melhorenvio.com.br/reference/cadastrar-loja.md), [consulta](https://docs.melhorenvio.com.br/reference/visualizar-loja.md) |
| Endereços da loja | POST e GET /api/v2/me/companies/{storeId}/addresses | [Cadastro](https://docs.melhorenvio.com.br/reference/cadastrar-endereco-de-uma-loja.md), [lista](https://docs.melhorenvio.com.br/reference/listar-enderecos-de-uma-loja.md) |
| Telefones da loja | POST e GET /api/v2/me/companies/{storeId}/phones | [Cadastro](https://docs.melhorenvio.com.br/reference/cadastrar-telefones-de-uma-loja.md), [lista](https://docs.melhorenvio.com.br/reference/listar-telefones-de-uma-loja.md) |

O índice consultado tem 65 páginas e 27 operações descritas em OpenAPI. Os guias de cotação, OAuth, carrinho e logística reversa acrescentam contexto, mas algumas dessas rotas não aparecem no recorte OpenAPI; confirme caminho e schema na documentação ao vivo antes de implementar. IDs de serviço/transportadora podem diferir da versão anterior; não use nome exibido como identificador estável.
