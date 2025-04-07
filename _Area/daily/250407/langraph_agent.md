# Agentic AI

- 프로덕션의 어려움이 존재함. 
- Linear Structured RAG의 한계 

## LangGraph

- Cycle & Branching 
  - 루프 및 조건문을 구현 
- Persistence 
  - 그래프의 각 단계 후에 자동으로 상태 저장 
  - 노드 - 노드 간의 상태 값을 전달 
- Low Level Control 
  - LangGraph는 Lanchain 없이도 사용 가능 

### 그래프 기반 파이프라인 구성 

- networkx의 개념을 사용하여, 복잡한 파이프라인을 그래프 형태로 구성함으로써 흐름 관리가 용이 

### 모듈 단위 분산 개발 

- 협업시 장점 
  - Base Template 정의 
  - 도메인 전문자 : 노드의 개발에 집중 
  - 흐름 엔지니어 : 작성된 노드로 흐름을 구성 

### 워크플로우 일부 단계의 실험에 용이 

- Retrieval 방식에 대해서 다양한 모델등을 쉽게 교체하여 활용이 가능하다. 

### 모듈 단위 개발 : 조립형 모듈 

- 마치 LEGO 블록처럼 세부 모듈(Sub Module)을 연결하여 구성 
  - 모듈의 추가/변경 용이 
- 독립적인 모듈의 장점을 활용 
  - 각 Sub-Module의 교체가 쉬움 
  - 단계의 전/후에 Sub-Module을 추가 

## Langgraph

### Cycle & Branching 

#### 분기 처리 

- 조건부 분기 처리 
  - 조건에 따른 분기(Conditional Edge)로직을 도입하여, 직관적이고 유연한 라우팅 처리를 지원

``` 
1. 에이전트의 개념을 요약해줘 
2. 오늘 뉴스 요약 해줘 
3. 지난 회의 결과 요약 해줘 
```

위의 질문에 대해서 각 질문에 따라서 Routing(조건부 분기)을 처리하게 된다. 

- Agent Routing 
  - 도구 선택 
  - 도구를 추가하는 것만으로 쉽게 Routing 옵션을 추가할 수 있음 
- Function Calling 
  - More link "Rule Based Selection"
    - 선택의 주체인 LLM의 시스템 프롬프트에서 상세히 정의, 또한 Schema 에도 각 옵션의 선택 가이드를 상세하게 작성 


# References 

> https://www.youtube.com/watch?v=edsshVochqM