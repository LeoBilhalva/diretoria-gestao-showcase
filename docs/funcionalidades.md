# Funcionalidades

Esta página resume as funcionalidades implementadas no MVP do Diretoria Gestão.

## Dashboard

Visão rápida da operação, incluindo informações como vendas do dia, saldo, fiados e itens com estoque baixo.

## Produtos e estoque

- cadastro e edição de produtos;
- código de barras;
- entrada, saída e ajuste de estoque;
- baixa automática após venda;
- sugestões de categoria durante o cadastro;
- sugestão de preço com base em produtos da mesma categoria.

No Android, a câmera pode ser utilizada para leitura de códigos. No Desktop, leitores USB que funcionam como teclado podem ser utilizados no fluxo de atendimento.

## Vendas / PDV

- inclusão de produtos no carrinho;
- desconto;
- pagamentos em dinheiro, Pix, cartão ou fiado;
- baixa de estoque;
- lançamento financeiro;
- persistência da venda em andamento durante a navegação;
- opção para reiniciar a venda atual.

## Clientes e fiados

- cadastro de clientes;
- geração automática de conta a receber em venda fiada;
- pagamentos parciais;
- quitação total;
- histórico de recebimentos.

## Fornecedores

Cadastro e consulta de fornecedores para apoiar o controle de produtos e futuras evoluções do processo de compras.

## Caixa

- entradas;
- saídas;
- lançamentos provenientes de vendas;
- consulta de detalhes da movimentação.

## Relatórios

Relatórios de vendas por período e visão dos produtos de maior faturamento.

## Desktop + Android

O sistema pode ser utilizado nos dois ambientes compartilhando a mesma base de dados.

### Desktop

- aplicação Electron;
- servidor local;
- banco SQLite central;
- gerenciamento de dispositivos;
- leitor físico de código de barras;
- exportação de dados.

### Android

- aplicativo via Capacitor;
- leitura de código de barras pela câmera;
- pareamento com o Desktop;
- consulta e operação pela rede local.

## Pareamento

O dispositivo móvel é associado ao Desktop antes de acessar a operação. O objetivo é evitar que qualquer dispositivo presente na rede local tenha acesso automático ao sistema.

## Fora do escopo atual

Alguns itens foram deliberadamente deixados para etapas posteriores:

- NF-e/NFC-e;
- TEF;
- integração bancária Pix;
- lote e validade;
- compras completas com nota de fornecedor;
- multiempresa;
- sincronização entre lojas pela internet;
- e-commerce.

Esses recursos exigem novos requisitos de negócio e técnicos antes de serem implementados.
