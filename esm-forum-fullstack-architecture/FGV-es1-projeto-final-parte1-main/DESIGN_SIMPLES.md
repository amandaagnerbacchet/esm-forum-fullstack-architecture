# Análise de Design Simples (YAGNI)

Este documento analisa o código atual do backend sob a ótica do princípio de Design Simples e do princípio **YAGNI** (*You Aren't Gonna Need It*).

## 1. Aspectos Positivos (Seguem o Design Simples)
- **Rotas Diretas:** O uso do Express com rotas bem delimitadas evita camadas desnecessárias de abstração.
- **Uso Direto do SQLite:** O acesso ao banco de dados é feito de forma direta, sem complexidades excessivas de ORMs pesados.
- **Estrutura Enxuta:** O projeto implementa apenas o essencial para listar, criar e detalhar perguntas e respostas.

## 2. Oportunidades de Simplificação
- **Duplicação de Lógica:** A abertura e fechamento de conexões com o banco poderiam ser centralizadas.
- **Tratamento de Erros:** Blocos `try/catch` repetitivos poderiam ser extraídos para um middleware global.