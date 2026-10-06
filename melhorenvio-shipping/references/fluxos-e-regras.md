# Fluxos e regras do Melhor Envio

Fontes oficiais consultadas em 01/10/2026. Reconfirme fatos sujeitos a mudança antes de publicar código ou movimentar saldo.

## OAuth e HTTP

- Um aplicativo pode atender os usuários de uma plataforma. A URL de callback registrada deve coincidir exatamente com a enviada na autorização; divergência pode causar Client invalid. O callback é estático. State permite identificar a loja de origem e deve ser validado pelo integrador.
- Solicite só os scopes necessários. A referência oficial lista cart-read, cart-write, orders-read, shipping-calculate, shipping-checkout, shipping-generate, shipping-print, shipping-tracking e outros por operação.
- Access token vale 30 dias e refresh token, 45 dias. Na resposta Unauthenticated, conserve a chamada, renove e repita; se a autorização foi revogada ou a segunda chamada falhar, peça reconexão.
- API usa HTTPS, User-Agent com aplicação e e-mail de suporte, Accept e Content-Type JSON. As rotas OAuth têm formato próprio.

Fontes: [autenticação](https://docs.melhorenvio.com.br/docs/autenticacao-1.md), [fluxo de autorização](https://docs.melhorenvio.com.br/reference/fluxo-de-autorização.md), [permissão](https://docs.melhorenvio.com.br/docs/permissao-autorizacao.md), [introdução da API](https://docs.melhorenvio.com.br/reference/introducao-api-melhor-envio.md).

## Cotação, carrinho e frete

- Cotação aceita products, para o Melhor Envio calcular os pacotes, ou volumes já calculados. CEPs de origem e destino são obrigatórios. Medidas: cm, kg e reais. Uma chamada calcula um envio para um vendedor e um par de CEPs.
- Valor e prazo personalizados aparecem como custom_price e custom_delivery_time. Conserve os dados da cotação selecionada para criar o frete correspondente no carrinho.
- A inclusão no carrinho cria o ID usado no checkout e nas operações de etiqueta. O frete não é editável após inclusão.
- Correios, J&T e Loggi não aceitam vários volumes em uma inclusão; crie uma etiqueta por pacote. A documentação de compra também exclui o serviço centralizado de pacote, ID 27, desse formato.
- Pessoa física envia CPF em document; pessoa jurídica envia company_document e campos fiscais aplicáveis. Envio comercial requer chave da NF e inscrição estadual. Em não comercial, não envie options.invoice; confirme a regra de isenção de inscrição estadual.
- A documentação informa integração da DCe com a SEFAZ desde 06/04/2026. Produtos devem estar completos; se a DCe já existir, use a chave no campo documentado. Revalide as regras fiscais antes de implementar.
- Seguro deve refletir o valor declarado. A seleção de agência depende da transportadora e do modo de autorização; confira a referência atual.

Fontes: [cotação](https://docs.melhorenvio.com.br/docs/cotacao-de-fretes.md), [compra de fretes](https://docs.melhorenvio.com.br/docs/compra-de-fretes.md), [inclusão no carrinho](https://docs.melhorenvio.com.br/reference/inserir-fretes-no-carrinho.md).

## Compra, etiqueta, cancelamento e reversa

- Checkout compra IDs já presentes no carrinho e normalmente precisa de saldo suficiente na carteira. Há fluxo alternativo por gateway e redirecionamento. Confirme o estado do envio; a URL de retorno não confirma compra por si só.
- Geração ocorre após compra e é assíncrona. Só peça impressão quando a etiqueta estiver gerada. O modo público de impressão abre acesso a qualquer pessoa com o link.
- Logística reversa usa PAC/SEDEX dos Correios, um volume por solicitação. No Sandbox, é possível inserir e comprar, mas não concluir a geração do código de devolução.
- Antes de cancelar, consulte a elegibilidade. O cancelamento documenta reason_id igual a 2 e descrição. Após geração, transportadora avisada ou pacote postado podem impedir cancelamento; estorno aprovado é descrito como ocorrendo em até 12 horas.
- A validade do item no carrinho está contraditória: [compra](https://docs.melhorenvio.com.br/docs/compra-de-fretes.md) diz 7 dias; [FAQ](https://docs.melhorenvio.com.br/reference/faq.md) diz 20 dias. A FAQ também indica 20 dias para gerar após pagamento e, após geração, 20 dias para postagem privada ou 7 para Correios. Não codifique o prazo controverso sem confirmação.

Fontes: [checkout](https://docs.melhorenvio.com.br/reference/compra-de-fretes-1.md), [geração e impressão](https://docs.melhorenvio.com.br/docs/geracao-e-impressao-de-etiquetas-de-envio.md), [cancelamento](https://docs.melhorenvio.com.br/reference/cancelamento-de-etiquetas.md), [logística reversa](https://docs.melhorenvio.com.br/docs/logistica-reversa-carrinho.md).

## Webhooks e testes

- Webhook é registrado no aplicativo que gerou a etiqueta; envios de outro aplicativo não o disparam. X-ME-Signature é HMAC-SHA256 do corpo bruto com o segredo do aplicativo. Compare em tempo constante antes de processar JSON.
- Eventos documentados: order.created, order.pending, order.released, order.generated, order.received, order.posted, order.delivered, order.cancelled, order.undelivered, order.paused e order.suspended.
- O provedor informa timeout de 6 segundos e até 5 tentativas com intervalos de 15 minutos. Responda rápido após registrar o evento de forma durável. Tracking pode surgir até um dia útil depois da postagem.
- Sandbox e produção são separados. Sandbox simula transportadoras limitadas, não gera postagem real e tem limitações para reversa e impressão. Parceria verificada é opcional para consumir a API; é exigida para catálogo e benefícios da parceria.

Fontes: [webhooks](https://docs.melhorenvio.com.br/docs/webhooks.md), [Sandbox](https://docs.melhorenvio.com.br/docs/sandbox.md), [verificação](https://docs.melhorenvio.com.br/docs/verificacao.md), [FAQ](https://docs.melhorenvio.com.br/reference/faq.md).

## Observado em produção

Medido em 06/10/2026 numa loja real, com aplicativo próprio e etiqueta paga por link. Não está na documentação; reconfirme antes de depender.

- **Escopo faltando dá 403 sem citar o escopo.** A resposta é `This action is unauthorized.`. Sem `orders-read`, `GET /me/orders/{id}` falha. Sem `cart-read`, `GET /me/cart` e `/me/cart/{id}` falham. Sem `shipping-preview`, `/shipment/preview` falha. Sem `shipping-cancel`, `/shipment/cancel` e `/shipment/cancellable` falham. Escopo novo só vale depois de o usuário autorizar o aplicativo de novo.
- **Tracking do token do aplicativo pode ficar parado.** Depois do pagamento por link, `POST /shipment/tracking` com o token OAuth do aplicativo seguiu `pending` por horas. A mesma consulta pela sessão do painel dizia `released`, na mesma conta. `/shipment/generate` com o token do aplicativo foi aceito. Use o webhook assinado como fonte de verdade do pagamento e guarde o status dele.
- **Pagamento por link: só `yapay`.** `gateway: "mercado-pago"` no `/shipment/checkout` devolveu "O meio de pagamento escolhido não está disponível para sua conta". `gateway: "yapay"` com `redirect` devolveu o link de pagamento (Pix, boleto ou cartão); o Pix pode ser pago pelo app do Mercado Pago. Não há API que debite outra carteira sozinha.
- **Webhook chega antes do tracking.** `order.released` chegou no mesmo segundo de `paid_at`, com o tracking ainda `pending`. Responda não-2xx quando o trabalho não terminou; o reenvio vem em 15 minutos.
- **Status do aviso nem sempre acompanha o evento.** Um `order.generated` chegou com `data.status: "released"` e `data.tracking` já com o código dos Correios. Gerar de novo devolve `status: false` com "O envio já está gerado". Código da transportadora em `data.tracking` indica etiqueta gerada. `data` também traz `self_tracking` e `tracking_url`.
- **Webhook só se cadastra pelo painel:** Integrações, Área Dev, aplicativo, Novo Webhook. A API não tem rota para isso.
- **Cancelamento confirmado vem por ID:** a resposta é `{ "<id>": { "canceled": true } }`. Exija esse `true` antes de estornar o comprador. Etiqueta `posted` ou `delivered` já está com a transportadora.
