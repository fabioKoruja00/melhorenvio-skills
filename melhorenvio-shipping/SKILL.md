---
name: melhorenvio-shipping
description: Integra ou revisa a API de fretes do Melhor Envio, incluindo OAuth, cotação, carrinho, compra, etiquetas, rastreio, webhooks e sandbox. Use ao implementar ou depurar envios e logística reversa.
---

# Melhor Envio — integração de fretes

Use esta skill para implementar ou revisar uma integração REST do Melhor Envio. Leia [fluxos e regras](references/fluxos-e-regras.md) para decisões de negócio e [mapa de API](references/api-reference.md) para rotas. O índice oficial está em <https://docs.melhorenvio.com.br/llms.txt>; as páginas aceitam sufixo .md.

Estas notas foram conferidas em 01/10/2026. Abra a página oficial atual antes de decidir campos, rotas, prazos e disponibilidade. Quando duas páginas oficiais divergirem, registre a divergência; não fixe uma resposta por palpite.

## Sequência de integração

1. Confirme o ambiente. Sandbox e produção têm cadastros, aplicativos, tokens, saldo e envios separados. Sandbox não gera postagem real.
2. Crie o aplicativo da plataforma e peça somente os scopes necessários. Mantenha client secret, access token e refresh token no servidor. O callback registrado deve ser idêntico ao redirect URI enviado; use state para correlacionar e validar o retorno.
3. Implemente renovação do OAuth. O access token vale 30 dias e o refresh token, 45 dias. Salve o novo par retornado. Após Unauthenticated, preserve a operação, renove e repita uma vez; se persistir, peça nova autorização.
4. Use HTTPS, User-Agent com nome da aplicação e e-mail técnico, Accept e Content-Type JSON; confira a exceção das rotas OAuth.
5. Cote por produtos ou volumes. Origem e destino são CEPs; dimensões usam cm, peso kg e valor reais. Uma cotação cobre um vendedor e um par origem/destino. Quando retornados, apresente custom_price e custom_delivery_time.
6. Guarde a cotação escolhida e seus volumes. Envie os dados completos de remetente, destinatário, produtos, documentos e volumes ao criar o frete no carrinho. Guarde o ID devolvido: o envio não pode ser editado depois.
7. Compre os IDs presentes no carrinho após conferir a carteira ou o fluxo alternativo de pagamento. Não trate redirecionamento de gateway como prova de pagamento.
8. Depois da compra confirmada, solicite geração da etiqueta. A geração é assíncrona; confira o estado antes de imprimir. Links são privados por padrão; modo público permite acesso a quem tiver o link.
9. Acompanhe por consulta ou webhook. Valide X-ME-Signature com HMAC-SHA256 do corpo bruto e segredo do mesmo aplicativo. Persistir a notificação e tornar a atualização idempotente ajuda a lidar com retentativas.

## Operações reais

Compra de frete, adição de saldo, geração e cancelamento alteram estado e podem movimentar saldo. Antes de executá-las em conta real, confirme escopo e autorização do usuário. Não exponha tokens, segredos ou dados pessoais em logs e prompts.

A FAQ informa 250 requisições por minuto por usuário autenticado. A documentação diverge sobre a expiração de item no carrinho: a página de compra informa 7 dias e a FAQ, 20 dias. Consulte o estado real e a documentação atual antes de construir temporizadores.

## Referências

- [Fluxos e regras com fontes](references/fluxos-e-regras.md); a seção "Observado em produção" traz o que a API faz e a doc não diz (403 por escopo, tracking parado, gateway, webhook)
- [Rotas e fontes](references/api-reference.md)
- [Índice oficial](https://docs.melhorenvio.com.br/llms.txt)
