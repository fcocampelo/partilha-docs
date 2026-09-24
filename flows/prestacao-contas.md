# Diagrama Comportamental – Prestação de Contas de Convites

A prestação de contas foi escolhida como jornada crítica porque envolve controle de convites, validações de negócio, formas de pagamento e movimentações financeiras.

```mermaid
sequenceDiagram
    actor U as Usuário
    participant W as Aplicação Web
    participant A as Backend/API
    participant D as Banco de Dados

    U->>W: Acessa Prestação de Contas
    W->>A: Solicita dados do vendedor
    A->>D: Consulta convites e acertos realizados
    D-->>A: Retorna dados
    A-->>W: Retorna máximo disponível

    U->>W: Informa quantidade e pagamento
    W->>A: Envia prestação de contas

    A->>D: Consulta saldo de convites
    D-->>A: Retorna saldo disponível

    alt Quantidade inválida
        A-->>W: Informa erro
        W-->>U: Exibe mensagem de validação
    else Prestação válida
        A->>D: Registra prestação de contas

        opt Pagamento em dinheiro
            A->>D: Verifica existência de caixa aberto

            alt Caixa fechado
                A-->>W: Bloqueia operação
                W-->>U: Informa necessidade de abrir caixa
            else Caixa aberto
                A->>D: Registra movimentação no caixa
                A-->>W: Confirma operação
                W-->>U: Exibe sucesso
            end
        end
    end
```

## Regras representadas

1. O sistema consulta os convites e acertos existentes antes de apresentar o máximo disponível.
2. A quantidade informada na prestação de contas deve respeitar o saldo disponível.
3. Uma prestação inválida deve ser bloqueada e o usuário informado.
4. Quando houver recebimento em dinheiro, a operação deve considerar a situação do caixa.
5. Movimentações financeiras em dinheiro não devem ser registradas em um caixa inexistente ou fechado.

## Ajustes realizados após o uso da GenAI

A geração inicial foi revisada para evitar uma representação excessivamente simplificada da prestação de contas. Foram explicitadas as validações de quantidade disponível e a dependência do caixa para movimentações em dinheiro.

Também foi mantido apenas o comportamento conhecido do sistema, evitando que o modelo adicionasse integrações, serviços ou decisões arquiteturais que não estejam documentados.
