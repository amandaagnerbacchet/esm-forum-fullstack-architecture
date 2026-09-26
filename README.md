# ESM Forum — Engenharia de Software Moderna

Aplicação full-stack de fórum de dúvidas desenvolvida como projeto acadêmico para a disciplina de **Engenharia de Software Moderna**, com foco na aplicação prática de conceitos de arquitetura de software, princípios **SOLID**, **Design Patterns**, modelagem UML, desenvolvimento ágil e organização de código.

O projeto foi construído para demonstrar não apenas uma aplicação funcionando, mas principalmente **como um sistema de software pode ser projetado, organizado, documentado, evoluído e mantido**.

---

## Sobre o projeto

O **ESM Forum** é uma aplicação de fórum na qual usuários podem interagir por meio de perguntas, respostas e votos.

A aplicação foi utilizada como base prática para estudar e aplicar conceitos de Engenharia de Software durante diferentes etapas do desenvolvimento.

Entre os principais conceitos trabalhados estão:

* Desenvolvimento Full Stack;
* API REST;
* Node.js e Express;
* React;
* Banco de dados SQLite;
* Arquitetura em camadas;
* MVC;
* Princípios SOLID;
* Design Patterns;
* Modelagem UML;
* Kanban e desenvolvimento ágil;
* Separação de responsabilidades;
* Manutenibilidade e evolução de software.

---

# Qual é o objetivo deste projeto?

O objetivo do ESM Forum não é apenas disponibilizar um fórum funcionando.

O projeto serve como **laboratório prático de Engenharia de Software**.

A partir de uma aplicação relativamente simples, é possível analisar problemas reais de desenvolvimento e aplicar técnicas para melhorar sua estrutura.

Por exemplo:

* Como evitar que as rotas concentrem toda a lógica da aplicação?
* Como separar regras de negócio do acesso ao banco de dados?
* Como reduzir o acoplamento entre componentes?
* Como permitir que uma parte do sistema seja substituída sem modificar toda a aplicação?
* Como organizar o código para facilitar manutenção e evolução?
* Como aplicar padrões de projeto em situações reais?
* Como documentar a arquitetura de um sistema?
* Como utilizar princípios SOLID para produzir código mais organizado?

Dessa forma, o fórum funciona como uma **base prática para experimentar diferentes decisões arquiteturais e de desenvolvimento**.

---

# O que é possível fazer com o projeto?

Depois de executar o projeto localmente, o desenvolvedor pode utilizar o ESM Forum como uma aplicação real de estudo e também como base para novas implementações.

É possível, por exemplo:

* Executar o fórum localmente;
* Criar e testar funcionalidades;
* Analisar o código do backend;
* Analisar a interface React;
* Estudar a comunicação entre frontend e backend;
* Examinar as consultas ao banco SQLite;
* Modificar regras de negócio;
* Criar novos endpoints;
* Criar novas telas;
* Alterar a estrutura de persistência;
* Implementar novos padrões de projeto;
* Refatorar componentes existentes;
* Criar testes;
* Experimentar diferentes arquiteturas;
* Comparar diferentes formas de organização do código.

Ou seja, o repositório não foi criado para ser apenas baixado.

Ele foi estruturado para que o código possa ser **executado, estudado, modificado, testado e evoluído**.

---

# Tecnologias utilizadas

## Backend

* **Node.js**
* **Express**
* **SQLite**
* JavaScript
* API REST

## Frontend

* **React**
* JavaScript
* HTML
* CSS

## Engenharia de Software

* SOLID
* Design Patterns
* MVC
* Arquitetura em camadas
* UML
* Kanban
* Git e GitHub

---

# Arquitetura da aplicação

O projeto é dividido principalmente entre frontend e backend.

```text
                    ESM FORUM
                        │
          ┌─────────────┴─────────────┐
          │                           │
          ▼                           ▼
   FRONTEND REACT               BACKEND NODE.JS
          │                           │
          │       HTTP / JSON         │
          └──────────────►────────────┘
                                      │
                                      ▼
                              CAMADA DE NEGÓCIO
                                      │
                                      ▼
                              CAMADA DE DADOS
                                      │
                                      ▼
                                 SQLite
```

### Frontend

Responsável pela interface utilizada pelo usuário.

O frontend é desenvolvido em React e realiza requisições HTTP para a API disponibilizada pelo backend.

### Backend

Responsável pela API, regras de negócio, processamento das requisições e comunicação com o banco de dados.

O backend utiliza Node.js com Express.

### Banco de dados

O projeto utiliza SQLite para persistência dos dados.

---

# Estrutura do projeto

A organização principal do projeto é:

```text
ESM-Forum/
│
├── esmforum/
│   ├── ...
│   └── Backend Node.js / Express
│
├── esmforum-react/
│   ├── ...
│   └── Frontend React
│
├── PADROES_PROPOSTOS.md
├── ARQUITETURA.md
├── PROPOSTA_ARQUITETURA.md
└── README.md
```

Os arquivos de documentação registram as decisões e propostas arquiteturais desenvolvidas durante o projeto.

---

# Design Patterns aplicados

Como parte da evolução do projeto, foram analisados três padrões de projeto.

## Factory Method

Utilizado como proposta para centralizar a criação de serviços e reduzir instanciações espalhadas pelo código.

A ideia é concentrar a criação dos objetos em uma estrutura responsável por fornecer o serviço adequado.

Exemplo conceitual:

```javascript
class ServiceFactory {
    static criarServico(tipo) {
        if (tipo === 'pergunta') {
            return new PerguntaService(new PerguntaRepository());
        }

        if (tipo === 'voto') {
            return new VotoService(new VotoRepository());
        }

        throw new Error('Serviço desconhecido');
    }
}
```

### Benefício estudado

Redução do acoplamento e centralização da criação de objetos.

---

## Adapter

Utilizado como proposta para isolar a aplicação da implementação específica do banco de dados.

Exemplo:

```javascript
class SQLiteAdapter {
    constructor(dbConnection) {
        this.db = dbConnection;
    }

    async query(sql, params) {
        return new Promise((resolve, reject) => {
            this.db.all(sql, params, (err, rows) => {
                if (err) {
                    reject(err);
                } else {
                    resolve(rows);
                }
            });
        });
    }
}
```

### Benefício estudado

A aplicação passa a depender de uma interface de acesso, reduzindo o impacto de uma eventual substituição da tecnologia de persistência.

---

## Observer

Utilizado como proposta para desacoplar eventos de negócio de mecanismos de notificação.

Exemplo:

```javascript
const EventEmitter = require('events');

class NotificacaoObserver extends EventEmitter {}

const notificacaoBus = new NotificacaoObserver();

notificacaoBus.on('respostaCriada', (perguntaId, autorId) => {
    console.log(
        `Enviando notificação para o usuário ${autorId} sobre a pergunta ${perguntaId}`
    );
});
```

### Benefício estudado

Permite que diferentes componentes reajam a acontecimentos do sistema sem que o serviço responsável pela operação precise conhecer diretamente todos os componentes interessados.

---

# Arquitetura proposta

Além dos padrões de projeto, foi proposta uma organização baseada na separação de responsabilidades.

```text
Requisição HTTP
       │
       ▼
   Controller
       │
       ▼
    Service
       │
       ▼
  Repository
       │
       ▼
    SQLite
```

### Controller

Responsável por receber as requisições HTTP, validar entradas básicas e devolver as respostas.

### Service

Responsável pelas regras de negócio da aplicação.

### Repository

Responsável pelo acesso e persistência dos dados.

Essa separação permite reduzir a concentração de responsabilidades e facilita futuras alterações.

---

# Documentação técnica

O repositório possui documentos específicos para registrar as decisões arquiteturais.

### `PADROES_PROPOSTOS.md`

Apresenta a proposta de utilização dos padrões:

* Factory Method;
* Adapter;
* Observer.

Também contém exemplos de implementação e diagrama UML.

### `ARQUITETURA.md`

Apresenta a análise da arquitetura atual do sistema, incluindo:

* arquitetura existente;
* divisão entre frontend e backend;
* camadas identificadas;
* comunicação entre os componentes;
* diagrama arquitetural.

### `PROPOSTA_ARQUITETURA.md`

Apresenta a proposta de evolução arquitetural utilizando:

* separação em camadas;
* Controllers;
* Services;
* Repositories/Models;
* MVC.

---

# Desenvolvimento Ágil

O desenvolvimento do projeto foi acompanhado utilizando **GitHub Projects** e organização de tarefas em formato Kanban.

## Quadro Kanban

[Visualizar o Kanban do projeto](https://github.com/users/amandaagnerbacchet/projects/3/views/1)

O quadro permite acompanhar as atividades planejadas, em desenvolvimento e concluídas.

---

# Como executar o projeto

## Pré-requisitos

Antes de executar o projeto, é necessário ter instalado:

* Node.js;
* npm;
* Git.

Verifique as instalações:

```bash
node --version
npm --version
git --version
```

---

## 1. Clonar o repositório

No terminal:

```bash
git clone https://github.com/amandaagnerbacchet/esm-forum-fullstack-architecture.git
```

Entre na pasta do projeto:

```bash
cd esm-forum-fullstack-architecture
```

---

# 2. Executar o Backend

Entre na pasta do backend:

```bash
cd esmforum
```

Instale as dependências:

```bash
npm install
```

Depois execute o servidor:

```bash
npm start
```

O backend ficará disponível na porta definida pela configuração do projeto.

Caso o projeto utilize a porta `3000`, por exemplo:

```text
http://localhost:3000
```

> A porta efetiva deve ser conferida nos scripts/configurações do backend caso seja diferente.

---

# 3. Executar o Frontend

Mantenha o backend executando e abra **outro terminal**.

Volte para a raiz do projeto:

```bash
cd esm-forum-fullstack-architecture
```

Entre na pasta do frontend:

```bash
cd esmforum-react
```

Instale as dependências:

```bash
npm install
```

Execute a aplicação:

```bash
npm run dev
```

Caso o projeto esteja configurado para utilizar outro script de inicialização, utilize o comando definido no `package.json`.

Após iniciar o frontend, o terminal exibirá o endereço local da aplicação, normalmente semelhante a:

```text
http://localhost:5173
```

---

# Fluxo de execução

Com os dois serviços funcionando, o fluxo esperado é:

```text
Usuário
   │
   ▼
React
   │
   │ HTTP / JSON
   ▼
Express / Node.js
   │
   ▼
Regras da aplicação
   │
   ▼
SQLite
```

O frontend atua como interface para o usuário, enquanto o backend processa as requisições e realiza as operações necessárias no banco de dados.

---

# Como utilizar este projeto para estudo

Depois de executar a aplicação, o projeto pode ser utilizado como um ambiente de experimentação.

Algumas atividades possíveis:

### 1. Entender a aplicação

Comece pelo frontend e identifique:

* componentes;
* páginas;
* chamadas à API;
* gerenciamento de dados.

Depois analise o backend:

* rotas;
* controllers;
* regras de negócio;
* acesso ao banco.

### 2. Criar uma nova funcionalidade

Por exemplo:

```text
Nova funcionalidade
       │
       ├── Frontend
       │
       ├── Endpoint
       │
       ├── Regra de negócio
       │
       └── Persistência
```

### 3. Refatorar o código

Identifique trechos que possuem responsabilidades misturadas e experimente separá-los em:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

### 4. Experimentar Design Patterns

Os padrões apresentados no projeto podem ser implementados de forma incremental e comparados com a implementação original.

### 5. Evoluir a arquitetura

Também é possível experimentar:

* novos repositories;
* novos services;
* validação de dados;
* tratamento de erros;
* testes automatizados;
* autenticação;
* autorização;
* novos endpoints;
* novos componentes React.

---

# Objetivos acadêmicos

O projeto foi desenvolvido para demonstrar, de forma prática, conhecimentos relacionados a:

* análise de sistemas;
* arquitetura de software;
* princípios SOLID;
* padrões de projeto;
* modelagem UML;
* desenvolvimento frontend;
* desenvolvimento backend;
* APIs REST;
* persistência de dados;
* desenvolvimento ágil;
* documentação técnica;
* refatoração;
* separação de responsabilidades.

A proposta é demonstrar a evolução do pensamento de desenvolvimento: **não apenas fazer o código funcionar, mas compreender como estruturar um software para que ele possa ser mantido e evoluído.**

---

# Licença

Este projeto foi desenvolvido para fins acadêmicos e de estudo.

---

# Autoria

**Amanda Agner Bacchet**

Projeto desenvolvido para a disciplina de **Engenharia de Software Moderna**.
