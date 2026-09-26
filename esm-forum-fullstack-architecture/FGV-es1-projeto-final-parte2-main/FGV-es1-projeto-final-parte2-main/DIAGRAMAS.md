# Diagramas UML - ESM Forum

Este documento consolida os diagramas UML desenvolvidos para o sistema ESM Forum.

## 1. Diagrama de Classes
```mermaid
classDiagram
    class Usuario {
        +int id
        +string nome
        +string email
        +string senha
        +criarPergunta()
        +votar()
    }

    class Pergunta {
        +int id
        +string titulo
        +string conteudo
        +string tag
        +int votos
        +int autorId
        +adicionarResposta()
    }

    class Resposta {
        +int id
        +string conteudo
        +int perguntaId
        +int autorId
    }

    class Voto {
        +int id
        +int usuarioId
        +int perguntaId
        +int tipoVoto
    }

    Usuario "1" --> "*" Pergunta : escreve
    Usuario "1" --> "*" Resposta : responde
    Pergunta "1" --> "*" Resposta : possui
    Usuario "1" --> "*" Voto : realiza
    Pergunta "1" --> "*" Voto : recebe



sequenceDiagram
    actor Usuario
    participant Frontend
    participant API
    participant Database

    Usuario->>Frontend: Clica no botão de Upvote/Downvote
    Frontend->>API: Envia requisição POST /api/perguntas/:id/voto
    API->>Database: Verifica se o usuário já votou na pergunta
    Database-->>API: Retorna status do voto anterior
    alt Usuário não votou
        API->>Database: Insere novo registro de voto
        API->>Database: Atualiza contador na tabela de perguntas
    else Usuário já votou no mesmo sentido
        API->>Database: Remove o voto existente (anula)
        API->>Database: Atualiza contador na tabela de perguntas
    end
    Database-->>API: Confirmação de alteração
    API-->>Frontend: Retorna novo total de votos atualizado
    Frontend-->>Usuario: Atualiza visualmente o contador na tela

graph TD
    A[Início: Usuário clica em votar] --> B{Usuário está autenticado?}
    B -- Não --> C[Exibir tela de login]
    B -- Sim --> D{Já votou nesta pergunta?}
    D -- Não --> E[Registrar novo voto no banco]
    D -- Sim --> F{É o mesmo voto?}
    F -- Sim --> G[Remover/Anular o voto]
    F -- Não --> H[Atualizar tipo do voto]
    E --> I[Recalcular total de votos]
    G --> I
    H --> I
    I --> J[Atualizar interface em tempo real]
    J --> K[Fim]




stateDiagram-v2
    [*] --> Criada: Usuário envia nova pergunta
    Criada --> Publicada: Sistema valida e exibe no feed
    Publicada --> Editada: Autor altera o conteúdo
    Editada --> Publicada: Alterações salvas
    Publicada --> Votada: Usuários interagem com up/downvote
    Votada --> Publicada: Votos computados
    Publicada --> [*]: Pergunta arquivada ou excluída