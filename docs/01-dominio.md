# 1. Entendimento do Domínio e Modelagem (DDD)

## 1.1. Contexto do Domínio
A gestão de cargas químicas no Porto de Santos envolve produtos perigosos (inflamáveis, corrosivos, tóxicos).
O sistema QuimiPort visa garantir que nenhuma carga seja movimentada sem que o produtos esteja cadastrado, a
classificação de risco esteja definida, os documentos obrigatórios estejam anexados e válidos, e um responsável
técnico tenha autorizado a liberação.

## 1.2. Linguagem Ubíqua (Glossário)
- **Carga Química**: Conjunto de uma quantidade específica de um produto químico, com um status definido, aguardando
movimentação portuária.
- **Produto Químico**: Substância química cadastrada, com nome, classe de risco e indicador de atividade (ativo/inativo).
- **Classificação de Risco**: Categoria do produto (ex: Inflamável, Corrosivo, Tóxico, Radiotivo).
- **Responsável Técnico**: Profissional habilitado (ex: químico) responsável por analisar e liberar a carga.
- **Status da Carga**: Estados possíveis: REGISTRADA, EM_INSPECAO, LIBERADA, BLOQUEADA, CANCELADA.
- **Documentação Obrigatória**: Arquivos exigidos por lei para cada tipo de produto (ex: FISPQ, Laudo Técnico, Cartificado de Análise).

---

## 1.3. Entidades
| Nome da Entidade | Responsabilidade | Principais Atributos | Regras Relacionadas | Relacionamentos |
| :--- | :--- | :--- | :--- | :--- |
| **Produto Químico** | Armazenar dados do produto químico cadastrado. | `id`, `nome`, `claseRisco`, `ativo` | Não pode ser cadastrado sem nome e sem classe de risco. Inativo não pode ser vinculado a novas cargas. | Uma carga possui 1 Produto. |
| **Carga Química** (Agregado Raiz) | Gerenciar o ciclo de vida da carga, controlando status e regras de segurança. | `id`, `quantidade`, `status`, `dataRegistro` | Quantidade deve > 0. Só pode ser liberada se documentação estiver ok. | Possui 1 produto, 1 Responsável, e vários Documentos. |
| **Responsável Técnico** | Responsável pela análise e liberação da carga. | `id`, `nome`, `registroProfissional` | Toda carga deve possuir um. | Vinculado a várias Cargas. |
| **Documento da Carga** | Armazenar os documentos obrigatórios anexados. | `id`, `tipo`, `url`, `dataValidade` | Documentos devem estar dentro da validade para liberar a carga. | Pertence a uma única Carga. |

---

## 1.4. Objetos de Valor (Value Objects)
- **ClassificacaoRisco**: Enum com valores `INFLAMAVEL`, `CORROSIVO`, `TOXICO`, `RADIOATIVO`, `OUTROS`. Não possui identidade própria.
- **Quantidade**: Valor numérido positivo (ex: `1500` litros). A regra é que deve ser sempre > 0.
- **StatusCarga** Enum com valores `REGISTRADA`, `EM_INSPECAO`, `LIBERADA`, `BLOQUEADA`, `CANCELADA`.

---

