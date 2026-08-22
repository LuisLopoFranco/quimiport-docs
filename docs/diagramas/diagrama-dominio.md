# Diagrama de Domínio - Agregados e Entidades

Este diagrama representa a modelagem do domínio do sistema QuimiPort, mostrando o agregado raiz **CargaQuimica** e seus relacionamentos com as demais entidades.

## Código Mermaid

```mermaid
classDiagram
    class CargaQuimica {
        +String id
        +Float quantidade
        +StatusCarga status
        +Date dataRegistro
        +liberar()
        +bloquear()
        +cancelar()
        +solicitarInspecao()
    }
    class ProdutoQuimico {
        +String id
        +String nome
        +ClasseRisco classe
        +Boolean ativo
        +inativar()
    }

    class ResponsavelTecnico {
        +String id
        +String nome
        +String registro
    }

    class Documento {
        +String id
        +String tipo
        +String url
        +Date dataValidade
        +Boolean isValido()
    }

    CargaQuimica "1" --> "1" ProdutoQuimico
    CargaQuimica "1" --> "1" ResponsavelTecnico
    CargaQuimica "1" --> "*" Documento
