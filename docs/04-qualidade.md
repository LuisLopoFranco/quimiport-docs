
# 4. Planejamento de Qualidade de Software

## 4.1. Estratégia Geral de Testes
Seguirei a Pirâmide de Testes:
1. **Testes Unitários (Maioria)**: Testar as regras de negócio isoladas (Entidades e Objetos de Valor) e os Casos de Uso com mocks.
2. **Testes de Integração (Média)**: Testar a comunicação com o banco de dados e APIs externas.
3. **Testes End-to-End (Poucos)**: Validar os fluxos completos do usuário (futuramente).

## 4.2. Cenários Críticos Obrigatórios para Teste
- [ ] Não permitir cadastro de produto químico sem classe de risco.
- [ ] Não permitir registro de carga com produto químico inativo.
- [ ] Não permitir liberação de carga sem documentação obrigatória válida.
- [ ] Não permitir movimentação de carga bloqueada.
- [ ] Permitir liberação de carga com documentação válida e em inspeção.
- [ ] Validar que a quantidade de carga é maior que zero.
- [ ] Validar transição de status (ex: `CANCELADA` não vai para `LIBERADA`).

## 4.3. Como aplicar Testes Unitários (Futuro)
Usarei frameworks como **Jest** ou **Vitest**. As regras de negócio (funções puras) e os Use Cases serão testados isoladamente, substituindo dependências (repositórios) por **Mocks**.

## 4.4. Como aplicar Testes de Integração (Futuro)
Usarei bancos de dados em memória (ex: `sqlite3` ou `mongodb-memory-server`) para testar a interação real entre a aplicação e o banco de dados.

## 4.5. Organização de Mocks e Dados Simulados
Criarei uma pasta `tests/fixtures/` para armazenar objetos "fakes" (ex: `produtoValido.ts`, `cargaComDocumentacao.ts`) que serão reutilizados em todos os testes para garantir consistência.

## 4.6. Estratégia para Fluxos Principais
Os fluxos de "Registrar -> Inspecionar -> Liberar" e "Registrar -> Bloquear" serão priorizados nos testes End-to-End (E2E) para garantir a felicidade do sistema operacional.
