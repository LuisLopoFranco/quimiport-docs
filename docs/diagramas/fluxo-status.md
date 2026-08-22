```mermaid
stateDiagram-v2
    [*] --> REGISTRADA : Registrar Carga
    
    REGISTRADA --> EM_INSPECAO : Solicitar Inspeção
    REGISTRADA --> CANCELADA : Cancelar Carga
    
    EM_INSPECAO --> LIBERADA : Validar Documentos e Liberar
    EM_INSPECAO --> BLOQUEADA : Bloquear por Irregularidade
    EM_INSPECAO --> CANCELADA : Cancelar Carga
    
    BLOQUEADA --> EM_INSPECAO : Reabrir para Inspeção (após correção)
    BLOQUEADA --> CANCELADA : Cancelar Carga
    
    LIBERADA --> [*] : Movimentação Concluída
    CANCELADA --> [*] : Fim do ciclo
    
    note right of BLOQUEADA
        Carga não pode ser movimentada
        enquanto estiver Bloqueada.
    end note
    
    note right of LIBERADA
        Único status que permite 
        movimentação portuária.
    end note
