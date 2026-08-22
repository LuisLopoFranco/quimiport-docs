# 2. Casos de Uso Planejados

## 2.1. Lista de Casos de Uso
1. Cadastrar produto químico
2. Inativar produto químico
3. Registrar carga química
4. Validar documentação da carga
5. Solicitar inspeção
6. Liberar carga química
7. Bloquear carga química
8. Atualizar status da carga
9. Cancelar carga química
10. Consultar cargas por status
11. Consultar histórico da carga

---

## 2.2. Detalhamento dos Casos de Uso (Principais)

### Caso de Uso: Registrar Carga Química
- **Objetivo**: Cadastrar uma nova carga no sistema para iniciar o processo logístico.
- **Ator(es) envolvido(s)**: Operador Portuário.
- **Entrada esperada**: `idProduto`, `quantidade`, `idResponsavelTecnico`.
- **Saída esperada**: Dados da carga criada com status `REGISTRADA` e ID gerado.
- **Regras de negócio aplicadas**: 
  - Produto deve existir e estar `ativo`.
  - Quantidade deve ser > 0.
  - Responsável Técnico deve existir.
- **Possíveis erros/exceções**:
  - `Produto não encontrado`.
  - `Produto inativado não pode ser usado`.
  - `Quantidade inválida (menor ou igual a zero)`.

### Caso de Uso: Solicitar Inspeção
- **Objetivo**: Encaminhar a carga para análise técnica.
- **Ator(es)**: Operador Portuário / Gestor.
- **Entrada esperada**: `idCarga`.
- **Saída esperada**: Status alterado para `EM_INSPECAO`.
- **Regras**: A carga deve estar com status `REGISTRADA`.
- **Possíveis erros**: `Carga não está no status REGISTRADA`.

### Caso de Uso: Liberar Carga Química
- **Objetivo**: Liberar a carga para movimentação portuária.
- **Ator(es)**: Responsável Técnico.
- **Entrada esperada**: `idCarga`.
- **Saída esperada**: Status alterado para `LIBERADA`.
- **Regras de negócio aplicadas**:
  - Carga deve estar com status `EM_INSPECAO`.
  - Toda documentação obrigatória deve estar anexada e válida (dentro da validade).
- **Possíveis erros/exceções**:
  - `Carga não está em inspeção`.
  - `Documentação pendente ou vencida`.

### Caso de Uso: Bloquear Carga Química
- **Objetivo**: Impedir a movimentação de uma carga irregular.
- **Ator(es)**: Analista de Qualidade / Gestor.
- **Entrada esperada**: `idCarga`, `motivoBloqueio`.
- **Saída esperada**: Status alterado para `BLOQUEADA`.
- **Regras**: Carga não pode estar `CANCELADA` ou já `LIBERADA`.
- **Possíveis erros**: `Transição de status inválida`.

### Caso de Uso: Cancelar Carga Química
- **Objetivo**: Encerrar a operação da carga sem movimentá-la.
- **Ator(es)**: Gestor Operacional.
- **Entrada esperada**: `idCarga`.
- **Saída esperada**: Status alterado para `CANCELADA`.
- **Regras**: A carga não pode estar `LIBERADA`.
- **Possíveis erros**: `Carga já liberada não pode ser cancelada`.
