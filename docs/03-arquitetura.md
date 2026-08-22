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
