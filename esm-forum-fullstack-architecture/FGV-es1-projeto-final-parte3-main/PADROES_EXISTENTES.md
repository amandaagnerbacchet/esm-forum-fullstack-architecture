# Identificação de Padrões de Projeto - ESM Forum

Este documento analisa o código atual do ESM Forum para identificar padrões de projeto já aplicados (mesmo que parcialmente).

## 1. Padrões Identificados

### a) Padrão Arquitetural / Modular (Módulo Node.js)
- **Onde está aplicado:** Na estruturação de rotas (`routes/`) e conexão com o banco (`database/`).
- **Como se manifesta:** O projeto utiliza o sistema nativo de módulos do Node.js (`require` / `module.exports`) para separar responsabilidades de rotas e manipulação de dados.
- **Avaliação:** A implementação atende bem ao escopo básico didático, mas poderia ser melhorada aplicando padrões estruturais mais robustos para desacoplar a lógica de persistência das rotas HTTP.

### b) Padrão Middleware (Arquitetura do Express)
- **Onde está aplicado:** No núcleo do servidor Express (`index.js` ou arquivos de rotas).
- **Como se manifesta:** Interceptação de requisições HTTP antes de chegarem ao endpoint final para tratamento de requisições, parsing de JSON e controle de fluxo.
- **Avaliação:** Implementação completa e nativa provida pelo próprio framework Express, garantindo flexibilidade e extensibilidade.