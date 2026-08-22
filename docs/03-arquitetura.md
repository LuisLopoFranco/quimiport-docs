# 3. Arquitetura Proposta e Decisões Técnicas

## 3.1. Visão Geral da Arquitetura (Camadas)
Utilizarei uma arquitetura baseada em **Clean Architecture / DDD** para garantir baixo acomplamento e alta coesão. 
As camadas serão: 

1. **Domínio (Domain)**: O "coração" do sistema. Contém Entidades, Objeto de Valor, Agregados e Interfaces de Repositórios. **Não depende de nada externo**.
2. **Aplicação (Application)**: Orquestra os Casos de Uso. Contém os comandos (DTOs) e handlers. Depende apenas do Domínio.
3. **Infraestrutura (Infraestructure)**: Implementações concretas (bancos de dados, APIs externas). Depende do Domínio e da Aplicação.
4. **Apresentação (Presentation)**: Interface com o usuário (Frontend Web/Mobile) e/ou API REST (Backend). Depende da Aplicação.

## 3.2. Organização de Pastas (Estrutura futura em TypeScript)
```text
src/
├── domain/
│   ├── entities/          # Carga, Produto, Responsavel, Documento
│   ├── value-objects/     # StatusCarga, ClassificacaoRisco, Quantidade
│   ├── aggregates/        # CargaQuímica (Agregado Raiz)
│   ├── repositories/      # Interfaces (ex: ICargaRepository)
│   └── validators/        # Regras de negócio isoladas (funções puras)
├── application/
│   ├── use-cases/         # RegistrarCarga, LiberarCarga, etc.
│   ├── dtos/              # Input e Output models
│   └── interfaces/        # Interfaces para serviços externos
├── infrastructure/
│   ├── database/          # TypeORM/Prisma models e repositórios concretos
│   └── services/          # Implementações de serviços externos
└── presentation/
    ├── controllers/       # REST/GraphQL endpoints (futuro)
    └── middlewares/       # Autenticação, validação, etc.
```

## 3.3. Decisões Arquiteturais (ADR - Architecture Decision Records)

Abaixo estão registradas as principais decisões técnicas tomadas para o projeto QuimiPort, seguindo o formato padronizado de ADR (Contexto / Decisão / Consequências).

---

### ADR 001: Separação do Domínio, Aplicação e Infraestrutura

- **Contexto:** O sistema precisa lidar com regras de negócio complexas (transição de status, validação de documentos) que podem mudar com o tempo. Também precisamos de flexibilidade para trocar banco de dados ou serviços externos no futuro.
- **Decisão:** Adotar a arquitetura Clean / DDD, separando o código em 3 camadas principais: **Domínio** (regras puras), **Aplicação** (casos de uso) e **Infraestrutura** (banco de dados, APIs). O Domínio não pode depender de nada externo.
- **Consequências:**
- *Positivo:* Isolamento total das regras de negócio. Se o banco de dados mudar de PostgresSQL para MongoDB, as regras de "não liberar sem documento" continuam funcionando sem alteração.
- *Negativo:* Exige um pouco mais de código inicial (interfaces, injeção de dependencias) e maior curva de aprendizado.

---

### ADR 002: Concentração de Regras de Negócio no Domínio (e não nos Controladores)

- **Contexto:** Em muitas arquiteturas, as regras acabam "vazando" para os controllers ou para o banco de dados, dificultando a manutenção.
- **Decisão:** Toda regra crítica (ex: `Carga só pode ser liberada se estiver EM_INSPECAO` e `Documentos válidos`) será encapsulada dentro das Entidades e Agregados do Domínio. Os Casos de Uso (Application) apenas orquestram, mas não contêm lógica de negócio.
- **Consequências:**
  - *Positivo:* Facilidade extrema para testar as regras via testes unitários (Jest), sem precisar subir o servidor ou banco de dados.
  - *Negativo:* Os desenvolvedores precisam ter disciplina para não colocar regras nos lugares errados.

---

### ADR 003: Utilização de TypeScript como linguagem principal

- **Contexto:** A aplicação precisa ser robusta, com baixa incidência de erros em produção, e precisa evoluir para Backend e Frontend nas próximas fases.
- **Decisão:** Utilizar TypeScript (superset do JavaScript) em todo o projeto (backend e frontend futuramente).
- **Consequências:**
  - *Positivo:* Tipagem estática (interfaces, enums, generics) reduz drasticamente erros em tempo de execução. Facilita a refatoração e funciona como documentação viva do código.
  - *Negativo:* Exige um passo de compilação (transpilar para JS) e um pouco mais de código comparado ao JavaScript puro.

---

### ADR 004: Estratégia de Evolução para Backend (API REST) e Frontend (Web/Mobile)

- **Contexto:** O projeto atual é apenas documentação, mas precisa ser claramente evoluível para uma aplicação real nas fases seguintes do curso.
- **Decisão:** 
  - **Backend:** A camada de `Presentation` terá Controllers REST que expõem endpoints (ex: `POST /cargas`). Os Use Cases serão chamados a partir desses controllers.
  - **Frontend Web:** Será construído com React ou Angular, consumindo a API REST do backend.
  - **Mobile:** Será construído com React Native ou Flutter, consumindo a **mesma API REST** do backend, reutilizando 100% da lógica de negócio já testada.
- **Consequências:**
  - *Positivo:* Reuso total da camada de Domínio e Aplicação. O mobile e web compartilham a mesma lógica.
  - *Negativo:* A API precisa ser bem desenhada (versionamento) para não quebrar os clientes (web/mobile) quando sofrer alterações.

---

### ADR 005: Prevenção de Acoplamento Excessivo (Inversão de Dependência)

- **Contexto:** Se o código ficar muito acoplado (ex: o Caso de Uso instanciar diretamente o banco de dados), qualquer mudança na infraestrutura quebrará a aplicação.
- **Decisão:** Utilizar o princípio de **Inversão de Dependência (DIP)**. As camadas superiores (Domínio/Aplicação) definirão **interfaces** (contratos). As camadas inferiores (Infraestrutura) implementarão essas interfaces. Usaremos **Injeção de Dependência (DI)** para fornecer as implementações concretas aos Use Cases.
- **Consequências:**
  - *Positivo:* Máximo desacoplamento. Podemos trocar o repositório de banco de dados ou serviços de e-mail sem alterar uma linha dos Casos de Uso.
  - *Negativo:* Aumenta o número de arquivos (é preciso criar a interface e a implementação concreta), mas a manutenção futura compensa esse custo.
