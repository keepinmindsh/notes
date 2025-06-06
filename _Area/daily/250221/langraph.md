# Langgraph 

LLM 기반 다중 에이전트 프레임워크 

- 핵심 장점 
  - 사이클 지원 
  - 세밀한 제어 
  - 내장 지속성 
- 특징 
  - 세밀한 에이전트 워크 플로우 생성 
  - 순환 구조 가능 ( DAG와 차별화 )
  - 상태 및 흐름 정밀 제어 
  - 고급 휴먼 인더 루프 / 메모리 가능


# Langgraph에서 영감을 받은 프레임워크 

- Pregel 
- NetworkX
- ApacheBeam

# Langraph 구조 

- 에이전트 
- 노드 
- 엣지 

# Langraph 코드 구성 


- State(상태) 정의 
  - 에이전트 간에 어떤 정보를 주가 받을지 '틀'을 정의 

```python 
class State(TypedDict):
  messages: Annotated[list, add_messages] 
  
graph_builder = StateGraph(State)
```

- Node(노드) 선언 

```python 
llm = ChatAnthropic(model="claude-3.5-sonnet") 

def chatbot(state: State):
  return { "messages" : [llm.invoke(state["messages"])]}
```

- Edge(엣지) 및 그래프 선언 

```python
graph_builder.add_edge(START, "chatbot") 
graph_builder.add_edge("chatbot",  END)
graph = graph_builder.compile()
```

## Supervisor Architecture

# AI Agent 

