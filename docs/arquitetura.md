# Arquitetura

## Visão geral

O Diretoria Gestão utiliza uma arquitetura local, com o computador da empresa funcionando como ponto central da operação.

```mermaid
flowchart LR
    Desktop["Desktop<br/>Electron + React"] --> API["API local<br/>Node.js + Express"]
    Mobile["Android<br/>Capacitor + React"] <-->|LAN / Wi-Fi| API
    API --> DB[("SQLite")]
```

O banco de dados existe apenas no computador. O aplicativo Android consulta e altera os mesmos dados por meio da API local.

## Por que uma base central?

Durante o desenho da solução, uma alternativa seria manter um banco no Desktop e outro no celular. Isso adicionaria sincronização, conflitos e necessidade de definir qual cópia seria autoritativa.

A arquitetura adotada evita esse problema:

- uma única fonte de verdade;
- funcionamento sem internet;
- implantação de baixo custo;
- backup concentrado em um banco principal;
- possibilidade de conectar mais dispositivos no futuro.

## Desktop

O Desktop combina:

- interface React;
- aplicação Electron;
- inicialização da API local;
- acesso ao banco SQLite.

A interface do Desktop se comunica com a API pelo próprio computador.

## Mobile

O aplicativo Android utiliza a mesma interface React empacotada com Capacitor.

O celular:

1. conecta-se ao Desktop pela rede local;
2. é pareado com o sistema;
3. recebe autorização de acesso;
4. passa a consumir a API central.

Não existe uma segunda base de dados autoritativa no celular.

## Persistência

O banco utiliza SQLite com recursos adequados às operações críticas do sistema, incluindo:

- chaves estrangeiras;
- WAL;
- índices;
- transações.

## Fluxo simplificado de venda

```mermaid
sequenceDiagram
    participant U as Atendente
    participant UI as Desktop/Mobile
    participant API as API
    participant DB as SQLite

    U->>UI: Finaliza venda
    UI->>API: Envia itens e pagamento
    API->>DB: Valida produtos e estoque
    API->>DB: Cria venda e itens
    API->>DB: Atualiza estoque
    alt pagamento normal
        API->>DB: Registra entrada de caixa
    else fiado
        API->>DB: Cria conta a receber
    end
    DB-->>API: Confirma transação
    API-->>UI: Venda concluída
```

As etapas relacionadas são tratadas em conjunto para reduzir o risco de uma venda ser registrada sem a respectiva atualização de estoque ou financeiro.

## Escopo de rede

A solução foi pensada para uso dentro da rede privada do estabelecimento. A internet não é requisito para a operação diária; Desktop e celular precisam apenas conseguir se comunicar pelo mesmo roteador.

## Evolução prevista

Para uma versão mais ampla/comercial, a arquitetura poderá receber camadas adicionais de autenticação, permissões, backup automatizado, distribuição de atualizações e proteção de transporte.
