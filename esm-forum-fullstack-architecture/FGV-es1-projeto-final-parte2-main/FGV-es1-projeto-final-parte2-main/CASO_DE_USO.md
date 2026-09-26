# Caso de Uso: Votar em Pergunta

## Visão Geral
Este documento detalha o caso de uso referente à funcionalidade de votação (upvote/downvote) em perguntas do ESM Forum.

- **Nome do Caso de Uso:** Votar em Pergunta
- **Atores:** Usuário autenticado
- **Pré-condições:**
  - O usuário deve estar logado no sistema.
  - A pergunta alvo deve existir no banco de dados.

---

## Fluxos de Execução

### Fluxo Principal
1. O sistema exibe a lista de perguntas contendo o contador atual de votos e os botões de upvote e downvote.
2. O usuário autenticado seleciona o botão de upvote ou downvote em uma pergunta específica.
3. O sistema valida se o usuário possui sessão ativa e permissão para votar.
4. O sistema verifica se o usuário já registrou voto anterior para esta mesma pergunta.
5. O sistema registra o novo voto na base de dados.
6. O sistema atualiza o contador consolidado de votos da pergunta.
7. O sistema retorna a confirmação visual atualizada para a interface do usuário.

### Fluxos Alternativos
- **Fluxo Alternativo 1: Usuário já votou no mesmo sentido**
  - 3a. O sistema detecta que o usuário já havia votado exatamente com a mesma opção anteriormente.
  - 3b. O sistema remove o voto existente (anula a ação anterior).
  - 3c. O sistema atualiza o contador e retorna ao passo 7 do fluxo principal.

- **Fluxo Alternativo 2: Usuário altera o tipo de voto (ex: de upvote para downvote)**
  - 4a. O sistema detecta um voto prévio em sentido oposto.
  - 4b. O sistema atualiza o registro substituindo o voto anterior pelo novo.
  - 4c. O sistema recalcula o contador e prossegue para o passo 7 do fluxo principal.

---

## Pós-condições
- O voto do usuário fica devidamente gravado ou atualizado no banco de dados.
- O contador exibido reflete o saldo atualizado de votos da pergunta.