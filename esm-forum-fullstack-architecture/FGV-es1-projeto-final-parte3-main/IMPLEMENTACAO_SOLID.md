# Implementação com Princípios SOLID - ESM Forum

Este documento documenta a aplicação prática dos princípios SOLID em uma funcionalidade do sistema (Sistema de Votação).

## 1. Funcionalidade Implementada
Implementação do endpoint de **Votação em Perguntas**, separando responsabilidades de controle, regra de negócio e persistência de dados.

## 2. Aplicação dos Princípios SOLID

- **Single Responsibility Principle (SRP):**
  - O código foi desacoplado. A rota HTTP cuida apenas do recebimento e retorno da API; a classe `VotoService` encapsula toda a lógica de validação de votos (se o usuário já votou ou se vai alterar o voto); e a classe `VotoRepository` gerencia isoladamente as operações SQL no banco de dados.

- **Open/Closed Principle (OCP):**
  - O sistema de pontuação/votação foi estruturado permitindo que novas regras de cálculo de relevância sejam adicionadas por meio de extensão de classes de cálculo sem modificar o código base existente de contabilização.

- **Dependency Inversion Principle (DIP):**
  - Os serviços de negócio recebem suas dependências de acesso a dados por meio de parâmetros no construtor (Injeção de Dependência), evitando acoplamento rígido com implementações concretas do SQLite.

## 3. Exemplo de Estrutura de Código Aplicada

```javascript
// Exemplo conceitual da separação em camadas aplicando SRP e DIP
class VotoRepository {
    async buscarVoto(usuarioId, perguntaId) { /* Consulta SQL */ }
    async salvarVoto(usuarioId, perguntaId, tipo) { /* Insere no BD */ }
}

class VotoService {
    constructor(votoRepository) {
        this.votoRepository = votoRepository; // Injeção de Dependência (DIP)
    }

    async processarVoto(usuarioId, perguntaId, tipo) {
        // Regra de negócio isolada (SRP)
        const votoExistente = await this.votoRepository.buscarVoto(usuarioId, perguntaId);
        if (votoExistente) {
            // Lógica de atualização/remoção
        }
        return await this.votoRepository.salvarVoto(usuarioId, perguntaId, tipo);
    }
}