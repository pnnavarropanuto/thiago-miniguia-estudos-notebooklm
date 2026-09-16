# 🧠 Miniguia de Estudos: IA Generativa e Engenharia de Prompts com NotebookLM

> Projeto do desafio da \*\*DIO\*\* — uso do \*\*NotebookLM\*\* como ferramenta de aprendizagem ativa: curadoria de fontes, perguntas estratégicas, registro de tentativas e um miniguia de revisão.

![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen)
![Ferramenta](https://img.shields.io/badge/ferramenta-NotebookLM-blue)
![Tema](https://img.shields.io/badge/tema-IA%20Generativa-purple)

\---

## 📑 Sumário

1. [Contexto e Objetivos](#1-contexto-e-objetivos)
2. [Curadoria de Fontes](#2-curadoria-de-fontes)
3. [Engenharia de Prompts e "Cicatrizes"](#3-engenharia-de-prompts-e-cicatrizes)
4. [Miniguia de Estudo (Entrega Final)](#4-miniguia-de-estudo-entrega-final)

   * [4.1 Resumos estruturados](#41-resumos-estruturados)
   * [4.2 Glossário](#42-glossário)
   * [4.3 Prompts reutilizáveis](#43-prompts-reutilizáveis)
5. [Lições aprendidas](#5-lições-aprendidas)

\---

## 1\. Contexto e Objetivos

### Por que este tema?

Uso IA generativa no dia a dia para automações, desenvolvimento web e análise de dados. Percebi que eu usava os modelos de forma intuitiva, sem entender **por que** um prompt funciona melhor que outro, **de onde vêm as alucinações** e **quando vale usar RAG** em vez de só "perguntar ao chat". Este caderno foi montado para transformar esse uso intuitivo em conhecimento estruturado.

### Objetivos de estudo

|#|Objetivo|Como vou saber que aprendi|
|-|-|-|
|1|Entender a arquitetura **Transformer** e o mecanismo de **atenção** em nível conceitual|Explicar em 5 frases, sem jargão, como um LLM gera texto|
|2|Dominar técnicas de prompt: **zero-shot, few-shot, chain-of-thought, papéis e delimitadores**|Reescrever um prompt ruim aplicando pelo menos 3 técnicas|
|3|Entender **RAG** e por que ele reduz alucinações|Desenhar o fluxo recuperação → contexto → geração|
|4|Reconhecer tipos de **alucinação** e estratégias de mitigação|Listar 3 causas e 3 mitigações com base nas fontes|
|5|Criar um **kit de prompts reutilizáveis** para revisão|Seção 4.3 deste README|

### Por que NotebookLM?

O NotebookLM responde **com base apenas nas fontes carregadas** e mostra **citações numeradas** apontando o trecho usado. Isso o torna ideal para estudo: dá para conferir cada afirmação e ele próprio é um exemplo prático de **RAG** — o tema que estou estudando.

\---

## 2\. Curadoria de Fontes

Critérios usados na seleção:

* ✅ **Abertas** (acesso gratuito, sem paywall)
* ✅ **Primárias ou oficiais** (artigos originais e documentação de quem desenvolve os modelos)
* ✅ **Complementares** (fundamentos → técnica → prática → riscos)
* ✅ Formatos compatíveis com o NotebookLM (PDF e página web)

|#|Fonte|Tipo|Papel no caderno|Link|
|-|-|-|-|-|
|1|Vaswani et al. (2017) — *Attention Is All You Need*|PDF (arXiv)|Fundamento: arquitetura Transformer|[arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)|
|2|Wei et al. (2022) — *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*|PDF (arXiv)|Técnica: raciocínio passo a passo|[arxiv.org/abs/2201.11903](https://arxiv.org/abs/2201.11903)|
|3|Lewis et al. (2020) — *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*|PDF (arXiv)|Técnica: RAG|[arxiv.org/abs/2005.11401](https://arxiv.org/abs/2005.11401)|
|4|Ji et al. (2022) — *Survey of Hallucination in Natural Language Generation*|PDF (arXiv)|Riscos: alucinações e mitigação|[arxiv.org/abs/2202.03629](https://arxiv.org/abs/2202.03629)|
|5|Anthropic — *Prompt engineering overview* (documentação oficial)|Página web|Prática: boas práticas de prompt|[docs.claude.com — Prompt engineering](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview)|

> 💡 \*\*Dica de curadoria:\*\* misturei 4 artigos acadêmicos (densos, em inglês) com 1 guia prático. Só com artigos, as respostas ficavam teóricas demais; a documentação trouxe exemplos aplicáveis.

\---

## 3\. Engenharia de Prompts e "Cicatrizes"

Aqui documento **o raciocínio**, não só o resultado. Cada experimento tem: objetivo → prompt v1 → problema → prompt refinado → resultado → lição.

### 🔬 Experimento 1 — Visão geral do caderno

**Objetivo:** mapear o conteúdo das 5 fontes antes de aprofundar.

**Prompt v1:**

```
Resuma as fontes.
```

**Problema:** resposta genérica, um parágrafo por fonte, sem conexão entre elas. Pouco útil para estudar.

**Prompt v2 (refinado):**

```
Atue como um professor preparando uma aula introdutória.
Com base SOMENTE nas fontes deste caderno, organize o conteúdo em uma trilha
de aprendizagem com 4 etapas, do conceito mais básico ao mais avançado.
Para cada etapa informe: (1) conceito central, (2) quais fontes tratam dele,
(3) uma pergunta que eu deveria saber responder ao final.
Responda em português do Brasil.
```

**Resultado:** trilha em 4 etapas — *Transformer e atenção* (fonte 1) → *Técnicas de prompt e chain-of-thought* (fontes 2 e 5) → *RAG* (fonte 3) → *Alucinações e mitigação* (fontes 3 e 4), com citações em cada item.

**Lição:** definir **papel + formato + restrição de fontes + idioma** transformou um resumo solto em um plano de estudo.

\---

### 🔬 Experimento 2 — Entender o mecanismo de atenção

**Prompt v1:**

```
Explique self-attention.
```

**Problema:** a resposta reproduziu a fórmula `Attention(Q, K, V) = softmax(QKᵀ/√dₖ)V` com termos em inglês e pouca intuição. Correta, mas difícil para quem está começando.

**Prompt v2:**

```
Explique o mecanismo de self-attention do artigo "Attention Is All You Need"
para alguém que programa, mas não é da área de machine learning.
Use uma analogia do cotidiano, depois conecte a analogia aos termos
Query, Key e Value. Máximo de 200 palavras. Cite o trecho do artigo usado.
```

**Resultado:** analogia de uma busca: a *Query* é o que a palavra atual "procura", as *Keys* são as "etiquetas" das outras palavras e os *Values* são o conteúdo que ela "absorve", ponderado pela semelhança entre Query e Key. O modelo cita a seção de *Scaled Dot-Product Attention*.

**Lição:** pedir **público-alvo + analogia + limite de palavras** gera explicações mais didáticas sem perder a precisão, e a citação permite conferir.

\---

### 🔬 Experimento 3 — Chain-of-thought na prática

**Prompt v1:**

```
O que é chain of thought?
```

**Problema:** definição correta, mas sem mostrar **quando** a técnica funciona.

**Prompt v2:**

```
Com base no artigo de Wei et al.:
1. Defina chain-of-thought prompting em 2 frases.
2. Em que tipos de tarefa o artigo mostrou ganhos?
3. O artigo relaciona o ganho ao tamanho do modelo? Explique.
4. Crie um exemplo meu, fora do artigo, de prompt few-shot com chain-of-thought
   para calcular comissão de vendas.
Deixe claro o que vem das fontes e o que é exemplo criado por você.
```

**Resultado:** o modelo explicou que a técnica consiste em incluir **exemplos com passos intermediários de raciocínio**; que os ganhos aparecem em **aritmética, senso comum e raciocínio simbólico**; e que o efeito é **emergente em modelos grandes** (em modelos menores pode até piorar). O exemplo de comissão veio separado e sinalizado como não proveniente das fontes.

**Lição:** perguntas **numeradas** geram respostas mais completas. Pedir para **separar fonte x criação própria** evita misturar o que é evidência com o que é invenção.

\---

### 🔬 Experimento 4 — Conectar RAG e alucinação

**Prompt v1:**

```
RAG resolve alucinação?
```

**Problema:** pergunta fechada → resposta "sim, ajuda a reduzir", superficial.

**Prompt v2:**

```
Cruze as fontes de Lewis et al. (RAG) e Ji et al. (alucinações).
Monte uma tabela com colunas: "Causa da alucinação" | "Como o RAG ajuda" |
"Limitação que continua existindo". Depois, em um parágrafo, diga se as fontes
permitem afirmar que RAG ELIMINA alucinações.
```

**Resultado:** tabela relacionando falta de conhecimento atualizado/específico (RAG ajuda ao buscar documentos externos) e necessidade de rastrear a origem (RAG permite apontar a fonte), com a limitação de que **recuperar um documento errado ou irrelevante ainda leva a respostas erradas** e o modelo pode não seguir fielmente o contexto. Conclusão: as fontes sustentam **redução**, não **eliminação**.

**Lição:** pedir para **cruzar fontes** e usar **formato de tabela** é o melhor uso do NotebookLM — é algo difícil de fazer lendo os PDFs separadamente.

\---

### 🩹 Troubleshooting — dificuldades encontradas

|Dificuldade|O que aconteceu|Como contornei|
|-|-|-|
|**Idioma misto**|Fontes em inglês faziam respostas virem em inglês ou com termos sem tradução|Incluir "Responda em português do Brasil, mantendo termos técnicos em inglês entre parênteses"|
|**Fórmulas quebradas**|Equações dos PDFs do arXiv às vezes aparecem mal formatadas na extração|Pedir a explicação "em palavras" e conferir a fórmula direto no PDF original|
|**Perguntas fora das fontes**|Ao perguntar sobre modelos recentes, o NotebookLM informou que as fontes não cobrem o assunto|Não é erro — é o comportamento esperado. Anotei como limitação e mantive o foco no escopo|
|**Respostas longas demais**|Perguntas abertas geravam textos extensos|Definir limite ("máx. 150 palavras", "5 tópicos")|
|**Mistura de fonte e opinião**|Em pedidos de exemplos, ficava difícil saber o que era do artigo|Pedir explicitamente para separar "segundo as fontes" x "exemplo criado"|
|**Citação genérica**|Às vezes a citação apontava para um trecho amplo|Perguntar "em qual seção do artigo isso aparece?" e validar no PDF|

\---

## 4\. Miniguia de Estudo (Entrega Final)

### 4.1 Resumos estruturados

#### 🧩 Módulo 1 — Como um LLM funciona (Transformer)

* **Ideia central:** o Transformer dispensa recorrência (RNNs) e usa apenas **mecanismos de atenção** para relacionar todas as palavras de uma sequência entre si.
* **Self-attention:** cada token calcula o quanto deve "prestar atenção" em cada outro token, usando vetores **Query, Key e Value**.
* **Multi-head attention:** várias atenções em paralelo capturam relações diferentes (sintaxe, referência, significado).
* **Positional encoding:** como não há ordem implícita, a posição de cada token é adicionada à sua representação.
* **Vantagem prática:** processamento **paralelo** → treino muito mais eficiente, o que viabilizou modelos enormes.
* **Conexão com LLMs:** modelos generativos atuais derivam dessa arquitetura e geram texto **prevendo o próximo token**.

#### 🧩 Módulo 2 — Engenharia de prompts

* **Zero-shot:** só a instrução, sem exemplos.
* **Few-shot:** a instrução + alguns exemplos de entrada/saída para o modelo imitar o padrão.
* **Chain-of-thought:** exemplos (ou instrução) com **raciocínio passo a passo**; melhora tarefas de lógica e matemática, com efeito mais forte em modelos grandes.
* **Boas práticas (guia oficial):**

  * Seja claro, direto e específico — explique contexto e objetivo.
  * Dê um **papel** ao modelo quando fizer sentido.
  * Use **delimitadores/tags** para separar instruções, dados e exemplos.
  * Mostre exemplos do formato desejado.
  * Divida tarefas complexas em etapas (encadeamento de prompts).
  * Permita que o modelo diga "não sei" para reduzir invenções.

#### 🧩 Módulo 3 — RAG (Retrieval-Augmented Generation)

* **Problema que resolve:** o conhecimento do modelo fica "congelado" nos parâmetros; é difícil atualizar e rastrear a origem.
* **Como funciona:**

  1. A pergunta é usada para **buscar** trechos relevantes em uma base de documentos.
  2. Os trechos recuperados entram como **contexto**.
  3. O modelo **gera** a resposta condicionada nesse contexto.
* **Benefícios:** respostas mais específicas e factuais, base atualizável sem retreinar, possibilidade de citar fontes.
* **Limites:** a qualidade depende da **recuperação** — documento errado gera resposta errada.
* **Exemplo real:** o próprio **NotebookLM** aplica essa lógica sobre as fontes do caderno.

#### 🧩 Módulo 4 — Alucinações

* **Definição:** conteúdo gerado que é **sem sentido ou infiel** à fonte/realidade, mas apresentado com fluência.
* **Tipos:**

  * **Intrínseca:** contradiz a fonte fornecida.
  * **Extrínseca:** acrescenta informação que a fonte não permite verificar.
* **Causas comuns:** ruído nos dados de treino, divergência entre fonte e referência, forma de treino e decodificação.
* **Mitigações:** melhorar dados, ancorar em fontes externas (RAG), ajustes de treino/decodificação, verificação pós-geração e avaliação humana.
* **Regra de ouro para o usuário:** **confie, mas verifique** — peça citações e confira as afirmações críticas.

#### 🗺️ Mapa mental

```
                    IA GENERATIVA
                         │
     ┌───────────────┬───┴──────────┬────────────────┐
 Transformer      Prompts          RAG          Alucinações
     │               │              │                │
 Atenção        Zero/Few-shot   Busca→Contexto   Intrínseca
 Q, K, V        Chain-of-thought  →Geração       Extrínseca
 Multi-head     Papéis/Tags     Cita fontes      Mitigação ◄── RAG
 Posições       Encadeamento    Limite: busca    Verificação
```

\---

### 4.2 Glossário

|Termo|Definição|
|-|-|
|**LLM** (*Large Language Model*)|Modelo de linguagem com bilhões de parâmetros treinado para prever o próximo token em grandes volumes de texto.|
|**Token**|Unidade de texto processada pelo modelo (palavra, pedaço de palavra ou símbolo).|
|**Transformer**|Arquitetura de rede neural baseada em atenção, sem recorrência; base dos LLMs atuais.|
|**Self-attention**|Mecanismo em que cada token pondera a relevância de todos os outros tokens da sequência.|
|**Query, Key, Value**|Vetores usados na atenção: a Query "busca", as Keys são comparadas a ela e os Values são combinados conforme essa comparação.|
|**Multi-head attention**|Várias camadas de atenção em paralelo, cada uma capturando relações diferentes.|
|**Positional encoding**|Informação de posição adicionada aos tokens, já que a atenção não considera ordem por si só.|
|**Embedding**|Representação numérica (vetor) de um token ou texto que captura seu significado.|
|**Janela de contexto**|Quantidade máxima de tokens que o modelo considera de uma vez.|
|**Prompt**|Instrução e contexto enviados ao modelo para orientar a resposta.|
|**Engenharia de prompts**|Prática de projetar e iterar prompts para obter respostas mais úteis e confiáveis.|
|**Zero-shot**|Prompt sem exemplos, apenas com a instrução.|
|**Few-shot**|Prompt com alguns exemplos de entrada e saída esperada.|
|**Chain-of-thought (CoT)**|Técnica que induz o modelo a mostrar passos intermediários de raciocínio.|
|**Prompt de papel** (*role prompting*)|Atribuir uma persona/função ao modelo ("atue como professor...").|
|**Delimitadores / tags**|Marcadores (`"""`, `<documento>`) que separam instruções de dados.|
|**Encadeamento de prompts**|Dividir uma tarefa em várias chamadas, usando a saída de uma como entrada da próxima.|
|**RAG**|Técnica que recupera documentos externos e os usa como contexto para a geração.|
|**Retriever**|Componente do RAG que busca os trechos mais relevantes para a pergunta.|
|**Conhecimento paramétrico**|O que o modelo "sabe" armazenado nos próprios pesos.|
|**Conhecimento não paramétrico**|Conhecimento em base externa consultada no momento (ex.: documentos no RAG).|
|**Alucinação**|Resposta fluente, mas falsa, sem sentido ou infiel à fonte.|
|**Alucinação intrínseca**|Contradiz a fonte fornecida.|
|**Alucinação extrínseca**|Adiciona informação que não pode ser verificada pela fonte.|
|**Grounding** (ancoragem)|Basear a resposta em fontes verificáveis.|
|**Citação**|Referência ao trecho da fonte que sustenta a resposta — recurso central do NotebookLM.|

\---

### 4.3 Prompts reutilizáveis

Copie, troque o que está entre `\[colchetes]` e use em qualquer caderno do NotebookLM.

#### 📌 1. Trilha de estudo

```
Atue como professor. Com base SOMENTE nas fontes deste caderno, crie uma trilha
de aprendizagem em \[N] etapas, do básico ao avançado. Para cada etapa:
conceito central, fontes que tratam dele e uma pergunta de verificação.
Responda em português do Brasil.
```

#### 📌 2. Explicação para iniciantes

```
Explique \[CONCEITO] para alguém que \[PERFIL, ex.: programa mas não é de ML].
Use uma analogia do cotidiano e depois conecte a analogia aos termos técnicos.
Máximo de \[N] palavras. Cite o trecho da fonte usado.
```

#### 📌 3. Cruzamento de fontes

```
Compare o que as fontes \[FONTE A] e \[FONTE B] dizem sobre \[TEMA].
Monte uma tabela: "Ponto" | "Fonte A" | "Fonte B" | "Concordam?".
Finalize apontando lacunas que nenhuma das fontes cobre.
```

#### 📌 4. Autoavaliação (quiz)

```
Crie 10 perguntas de múltipla escolha sobre \[TEMA] com base nas fontes,
com 4 alternativas cada e nível de dificuldade crescente.
NÃO mostre as respostas agora. Quando eu responder, corrija e explique
cada erro citando a fonte.
```

#### 📌 5. Técnica de Feynman

```
Vou explicar \[CONCEITO] com minhas palavras: "\[MINHA EXPLICAÇÃO]".
Com base nas fontes, aponte: (1) o que está correto, (2) o que está impreciso
ou errado, (3) o que faltou. Seja direto e cite as fontes.
```

#### 📌 6. Flashcards

```
Gere \[N] flashcards sobre \[TEMA] no formato "Frente: pergunta | Verso: resposta
em até 2 frases", usando apenas informações das fontes.
```

#### 📌 7. Aplicação prática

```
Com base nas técnicas descritas nas fontes, crie um exemplo de uso de \[TÉCNICA]
aplicado a \[CONTEXTO DO MEU TRABALHO]. Separe claramente o que vem das fontes
e o que é exemplo criado por você.
```

#### 📌 8. Checagem de afirmação

```
Verifique a afirmação: "\[AFIRMAÇÃO]".
As fontes confirmam, contradizem ou não abordam isso? Classifique e justifique
com citação. Se não houver base nas fontes, diga explicitamente.
```

#### 📌 9. Revisão relâmpago

```
Faça uma revisão de 5 minutos sobre \[TEMA]: 5 tópicos-chave, 3 termos do
glossário que eu não posso esquecer e 1 erro comum a evitar.
```

\---

## 5\. Lições aprendidas

* 🎯 **Prompt vago = resposta vaga.** Papel, formato, limite e restrição de fontes fazem toda a diferença.
* 🔗 **O ponto forte do NotebookLM é cruzar fontes** com citações — algo lento de fazer manualmente.
* 🔍 **Citação não substitui leitura:** conferir no PDF original evitou pelo menos um mal-entendido com fórmulas.
* 🧪 **Estudar RAG usando uma ferramenta de RAG** tornou o conceito concreto: vi na prática tanto os benefícios (respostas ancoradas) quanto os limites (não responde fora do escopo).
* 🔁 **Iterar é parte do processo:** quase todo prompt bom veio de uma v1 insatisfatória.

\---

## 🛠️ Ferramentas

* [NotebookLM](https://notebooklm.google.com/) — caderno temático e perguntas às fontes
* GitHub — documentação e portfólio

## 👤 Autor

**Thiago Navarro Panuto**
Projeto desenvolvido para o desafio da [DIO](https://www.dio.me/).

