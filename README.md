# 🏦 FIAP Bank ATM

Simulador de Terminal de Autoatendimento (Caixa Eletrônico) desenvolvido em Java para a disciplina **Domain Driven Design - Java** do curso de Engenharia de Software da FIAP.

> Aluna: Giovana Gaspar Larocca — RM564965  
> Turma: 2ESPG | Professor: Eduardo dos Santos Ramos

---

## 📋 Sobre o Projeto

O FIAP Bank ATM simula as operações de um caixa eletrônico via console, aplicando os princípios de **Domain Driven Design**, **Orientação a Objetos** e **Arquitetura em Camadas**.

O projeto foi evoluído em dois checkpoints:

- **CP2:** Arquitetura base com entidades, classes abstratas, polimorfismo e Value Objects
- **CP3:** Contratos via Interfaces, Exceções de Domínio customizadas e resiliência na camada de apresentação

---

## ✨ Funcionalidades

- Cadastro de cliente com senha forte
- Autenticação com bloqueio após 3 tentativas incorretas
- Abertura de Conta Corrente ou Conta Poupança
- Consulta de saldo
- Depósito e Saque com validações de domínio
- Histórico de movimentações
- Mensagens de erro amigáveis (sem crash da aplicação)

---

## 🏗️ Arquitetura

O projeto segue a estrutura de pacotes de Domain Driven Design:

```
src/br/fiap/bank/atm/
│
├── model/                  # Domínio — entidades, value objects, regras de negócio
│   ├── interfaces/
│   │   └── Autorizavel.java          # Contrato de autenticação
│   ├── exceptions/
│   │   ├── SaldoInsuficienteException.java
│   │   ├── ValorInvalidoException.java
│   │   └── SenhaInvalidaException.java
│   ├── Conta.java                    # Classe abstrata (Template Method)
│   ├── ContaCorrente.java
│   ├── ContaPoupanca.java
│   ├── ContaAcesso.java              # implements Autorizavel
│   ├── Cliente.java
│   ├── Dinheiro.java                 # Value Object
│   ├── Movimentacao.java
│   ├── BaseEntity.java
│   ├── StatusConta.java
│   └── TipoMovimentacao.java
│
├── application/            # Serviços de aplicação — orquestração
│   ├── AutorizacaoService.java
│   ├── ContaService.java
│   └── ContaFactory.java
│
├── infrastructure/         # Repositório em memória
│   └── ContaRepository.java
│
├── presentation/           # Interface com o usuário (console)
│   └── TerminalBancarioController.java
│
└── Main.java
```

---

## 🔑 Decisões Arquiteturais (CP3)

### Interface `Autorizavel`
Define o contrato de segurança do banco. Qualquer mecanismo de autenticação futuro deverá implementá-la, garantindo extensibilidade sem quebrar o sistema atual.

```java
public interface Autorizavel {
    Boolean autorizar(String senha);
    Boolean isBloqueado();
}
```

### Exceções de Domínio (Unchecked)
Herdam de `RuntimeException` para refletir a Linguagem Ubíqua do negócio sem poluir as assinaturas dos métodos com `throws`:

| Exceção | Quando é lançada |
|---|---|
| `SaldoInsuficienteException` | Saque maior que o saldo disponível |
| `ValorInvalidoException` | Depósito ou saque com valor zero ou negativo |
| `SenhaInvalidaException` | Senha não atende aos critérios de segurança |

### Resiliência na Apresentação
A camada `presentation` captura todas as exceções de domínio com `try/catch`, exibe mensagens amigáveis e mantém o loop do menu ativo — a aplicação nunca encerra abruptamente.

---

## 🚀 Como Executar

1. Clone o repositório:
```bash
git clone https://github.com/giovanagasparlarocca/fiap-ddd-java-checkpoint2-atm.git
```

2. Abra o projeto no **IntelliJ IDEA**

3. Execute a classe `Main.java`

4. Siga as instruções no console para cadastrar sua conta e realizar operações

---

## 🔒 Regras de Negócio

- **Senha forte:** mínimo 8 caracteres, 1 número, 1 letra maiúscula e 1 caractere especial
- **Bloqueio:** conta bloqueada após 3 tentativas incorretas de senha
- **Conta Corrente:** taxa de manutenção de R$ 25,00 por saque
- **Conta Poupança:** rendimento mensal de 1,1% sobre o saldo
- Todos os valores monetários usam `Double` (Wrapper) — primitivos são proibidos

---

## 🛠️ Tecnologias

- **Java** (sem frameworks externos)
- **IntelliJ IDEA**
- **Git / GitHub**
