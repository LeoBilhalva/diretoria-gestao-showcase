# Decisões técnicas

O Diretoria Gestão foi construído em torno das restrições reais do cenário de uso: baixo custo, operação local, simplicidade para o usuário e disponibilidade mesmo sem internet.

## SQLite como banco central

### Decisão

Utilizar SQLite no computador da empresa como banco principal.

### Motivo

Para um único estabelecimento, um banco embutido reduz a complexidade de instalação e manutenção. O Mobile não precisa manter outra base e, portanto, não existe um processo de sincronização entre bancos.

## API local

### Decisão

O Android acessa os dados por uma API Node.js/Express executada no computador.

### Motivo

Isso mantém as regras de negócio e a persistência centralizadas. Desktop e Mobile trabalham sobre os mesmos dados.

## React compartilhado entre Desktop e Mobile

### Decisão

Utilizar a mesma aplicação React como base de interface para os dois ambientes.

### Motivo

Reduz duplicação de código e facilita manter os fluxos consistentes. Electron fornece o ambiente Desktop e Capacitor empacota a experiência para Android.

## Backend separado do processo Electron

### Problema encontrado

O projeto utiliza `better-sqlite3`, que contém um módulo nativo. Durante o desenvolvimento, houve incompatibilidade entre a ABI do Node.js utilizado para instalar o módulo e a versão do Node embarcada no Electron.

### Decisão

Executar o backend em um processo Node.js separado, iniciado pelo Desktop.

### Resultado

A API continua integrada à experiência do aplicativo, mas o acesso ao SQLite usa o ambiente Node para o qual a dependência nativa foi instalada.

## Transações em operações críticas

### Decisão

A finalização de uma venda é tratada como uma operação composta.

### Motivo

Registrar apenas parte da operação poderia gerar inconsistências, por exemplo:

- venda criada sem baixa de estoque;
- baixa de estoque sem lançamento financeiro;
- venda fiada sem conta a receber.

As alterações relacionadas são confirmadas juntas.

## Persistência do carrinho

### Decisão

Manter a venda em andamento ao navegar entre áreas da aplicação.

### Motivo

Durante o atendimento, o usuário pode precisar consultar outro cadastro ou tela. Perder os itens já adicionados ao PDV prejudicaria o fluxo operacional.

## Operação sem internet

### Decisão

Não depender de serviços em nuvem para o fluxo principal.

### Motivo

A disponibilidade da internet não deve impedir vendas e consultas dentro do estabelecimento. A rede local continua sendo necessária para a comunicação entre celular e computador.

## Limitações conscientes

A arquitetura atual prioriza um único estabelecimento e uma implantação simples. Uma evolução para múltiplas lojas ou acesso remoto exigiria rever aspectos como sincronização, autenticação, distribuição e infraestrutura.
