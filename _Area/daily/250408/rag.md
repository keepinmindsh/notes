# LLM Ops

- 내장 LLM 기능 
  - AIaas & 사전 학습 / 오픈소스 모델
- 프롬프트 엔지니어링 
  - AIaas & 오픈 소스 / instruction-tuned
- 내부지식 추가 
  - LLM이 답변에 사용학 위한 컨텐츠의 사전 준비 
- 고급기법 
  - 일련의 모델에 의한 파인튜닝 / 오케스트레이션 


# RAG 검증 개선 

- RAG 성능 개선을 위한 방법론은 정말 너무나도 많다. 
- 랭체인이나 라마인덱스에 구현되어 있는 것들은 빙산의 일각 


# RAG 평가 방법 

- 질문 
- 단락
  - Retrieval 평가 
  - Retrieve 된 단락 A, B, C가 retrieval gt A와 일치하는가?
- 생성 
  - 생성한 답변 평가 
  - 생성한 답변 A가 generation gt B와 유사한가 


## Context Recall 

## Context Precision

## Ground Truth를 어떻게 만드는가? 

- Simple Gen
- RAGAS Gen 


# 좋은 RAG 평가 데이터를 만드는 방법 


- 데이터 생성을 전부 자동화하는 것은 거의 불가능 
- 가장 좋은 방법은 "실제 유저 데이터 활용"
- 두번째로는 "도메인 전문가와 만드는 것"
- 세번째 옵션이 "Human In the Loop" 전략 
- 평가 데이터 셋을 만드는 일은 고통 스럽지만, 많은 시간과 노력을 투입해야함 

# Human in the loop QA Gen

- LLM으로 QA 생성 
- 사람의 검증 
- 프롬프트 재작성 

# LLM as Judge 

- 평가의 목적
  - Prompt for Evaluation 
    - 언어모델에 의해서 평가하자 

- The default prompt for pairwise comparison

``` 
System
Please act as an impartial judge and evaluate the quality of the responses provided by two AI assistants to the user question below. 
Please read the user’s question and the provided responses. 
Then follow the instructions and answer the user’s question better. 
Provide explanations for your reasons such as the helpfulness, 
relevance, accuracy, depth, creativity, and level of detail of their responses. 
Begin your evaluation by comparing the two responses and provide a short explanation. 
Avoid any position biases and ensure that the order in which the responses were presented does not influence your decision. 
Do not allow the length of the responses to influence your evaluation. 
Do not favor certain names of the assistants. Be as objective as possible. After providing your explanation, 
output your final verdict by strictly using the format: “[answerA]” if assistant A is better, “[answerB]” if assistant B is better, and “[C]” for a tie.

User Question
[question]

The Start of Assistant A’s Answer
A
[The End of Assistant A’s Answer]

The Start of Assistant B’s Answer
B
[The End of Assistant B’s Answer]

Figure 5: The default prompt for evaluation
```

``` 
[System]
You are a helpful assistant in evaluating the quality of the outputs for a given instruction. Your goal is to select the best output for the given instruction.

[User Input]
Select the Output (a) or Output (b) that is better for the given instruction. 
You should answer only using "Output (a)" or "Output (b). DO NOT output any other words.

Here are some rules of the evaluation:
(1) You should interpret whether the output honestly/precisely/closely follows the instruction, consider its helpfulness, accuracy, level of detail, harmlessness, etc.
(2) Outputs should not contain more/less than what the instruction asks for, as such outputs do NOT precisely execute the instruction.
(3) You should avoid any potential bias and your judgment should be as objective as possible. Do not presume that the order in which the outputs were presented should not affect your judgment, as Output (a) and Output (b) are **equally likely** to be the better one.

DO NOT provide any explanation for your choice.
DO NOT say both are nearly good.
You should answer using only "Output (a)" or "Output (b). DO NOT output any other words.

# Instruction:
{context}

# Output (a):
{output1}

# Output (b):
{output2}

# Which is better, Output (a) or Output (b)? Your response should be either "Output (a)" or "Output (b)".
```

> https://lmarena.ai/


# 평가에 대한 가이드라인이 너무 모호해서는 안된다.

``` 
Consider the input prompt in the <input> tags and the output answer in the <output> tags, the context in the <prompt_criteria> tags, and the answer evaluation criteria in the <answer_criteria> tags.

<prompt_criteria>
The prompt should be clear, direct, and detailed.
The question, task, or goal should be well explained and be grammatically correct.
The prompt is better if containing examples.
The prompt is better if it specifies a role or sets a context.
The prompt is better if it provides details about the format and tone of the expected answer.
</prompt_criteria>
```

``` 
answer-score : 답변 평가 점수 ( 0 ~ 100 ) - 환각이 있으면 점수가 크게 감소 
prompt-score : 프롬프트 평가 점수 ( 0 ~ 100 ) 
justification : 평가에 대한 이유 
input 및 output : 평가 대상 콘텐츠를 그대로 반환 
prompt-recommendations : 프롬프트 개선을 위한 구체적인 권장 사항 
```

# 평가 환경 설정 

- Function Call 함수 정의

# 모델별로 좋아하는 점수 범위가 따로 있을 수 있음 

- LLM은 연속적인 범위 평가에 약하기 때문에 LLM 분류 평가에 방식을 사용하는 것이 좋음

# 모델이 작을 수록 편향이 더 많이 생긴다. 

Prompt의 내용에 따라서 결과가 많이 달라질수 있다. 

# SoftMax Function 

- https://syj9700.tistory.com/38

# 📝 AI-based Paraphrasing Techniques

Paraphrasing is the task of expressing the same meaning using different words or sentence structures. It is a key component in many natural language processing (NLP) applications such as summarization, chatbot response generation, question generation, and more.

---

## 🚀 Methods of Paraphrasing in AI

### 1. Rule-based Paraphrasing
- Utilizes grammar rules, synonym replacement, and voice transformation (active ⇄ passive).
- **Example**:  
  `He wrote a book.` → `A book was written by him.`
- ✅ Simple and controllable
- ❌ Limited diversity, prone to semantic distortion

---

### 2. Machine Learning-based Paraphrasing
- Learns sentence structures and patterns from large parallel datasets.

#### Techniques:
- **Seq2Seq (RNN/LSTM)**: Encodes and decodes sequences to generate paraphrases.
- **Back-Translation**: Translates the original sentence into another language, then back to the original language.
    - Example: English → French → English

---

### 3. Pretrained Language Model-based Paraphrasing ✅ *Most commonly used today*
- Leverages large models like **BERT**, **GPT**, **T5**, **Pegasus**, etc.

#### Examples:
- **T5 (Text-to-Text Transfer Transformer)**  
  Input: `paraphrase: The weather is great today.`  
  Output: `It's a beautiful day.`

- **GPT-4 / ChatGPT**  
  Prompt-based generation:
  > `"Paraphrase this sentence: 'She completed the project on time.'"`  
  → `"The project was finished by her within the deadline."`

---

### 4. Embedding-based Semantic Paraphrasing
- Converts sentences into embedding vectors and retrieves semantically similar sentences from a vector database.

#### Applications:
- Semantic search (e.g., FAQ matching)
- Retrieving diverse question formulations

# Tips 

- 대문자 쓰지 말 것 
- 부정어 쓰지 말 것 
- 서비스를 운영하면서 행동 지표를 보는 것이 많이 도움이 될 수 있음 
- SLM의 경우에도 다양한 References를 SLM Prompt 기준으로 참조 해야함. 
- 작은 모델일 수록 Reasoning Step에 더 민감하다. 

# References 

> https://smith.langchain.com/hub/homanp/human-in-the-loop?organizationId=6722a1b7-48f3-4ec9-a5ac-e04155487f6a&tab=0    
> https://docs.smith.langchain.com/prompt_engineering/how_to_guides/manage_prompts_programatically   
> https://wikidocs.net/262593  
> https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/  
> https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/#how-does-resuming-from-an-interrupt-work  
> https://github.com/UpstageAI/solar-prompt-cookbook