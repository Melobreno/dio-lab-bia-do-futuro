# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `questoes_simulados.csv` | CSV | Banco de questões para geração de testes e simulados. |
| `editais_certificacoes.json` | JSON | Mapeamento de pesos e tópicos obrigatórios (CPA-10, 20, CEA). |
| `historico_estudante.json` | JSON | Armazena erros e acertos para personalizar a trilha de estudos. |
| `glossario_tecnico.csv` | CSV | Base de termos técnicos para garantir precisão nas explicações. |

---

## Adaptações nos Dados

Os dados originais foram expandidos para incluir uma coluna de **"Grau de Dificuldade"** e **"Tags de Módulo"** em cada questão. Isso permite que o EduCaFIN filtre perguntas específicas de "Derivativos" ou "Ética" quando identificar uma fraqueza recorrente do estudante.

---

## Estratégia de Integração

### Como os dados são carregados?
A base de editais e o glossário são carregados via **RAG (Retrieval-Augmented Generation)** utilizando um banco vetorial. Já o histórico do estudante e as questões do simulado atual são carregados na memória da sessão para garantir agilidade na resposta.

### Como os dados são usados no prompt?
Os dados são consultados dinamicamente. Quando o usuário erra uma questão, o agente busca na base de editais qual a importância daquele tópico e insere no `System Prompt` a instrução: *"O usuário falhou em um tópico de peso 20% no edital. Priorize uma explicação técnica profunda antes de seguir para a próxima questão."*

---

## Exemplo de Contexto Montado

```text
Contexto do Estudante:
- Certificação Alvo: CPA-20
- Nível Atual: Intermediário
- Módulo Crítico: Instrumentos de Renda Variável (Taxa de acerto: 45%)

Último Erro:
- Questão ID: 402 (Tópico: Tributação de Ações)
- Resposta do Usuário: "Isenção para vendas até 15k"
- Fato Correto: Isenção de IR para PF em vendas até 20k no mês.
