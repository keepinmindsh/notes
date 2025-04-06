# Graph RAG 

기존 벡터기반의 RAG 한계점을 보완하기 위해서 지식 그래프 기반의 RAG가 점차 활용되고 있습니다.   
일반적인 벡터 검색을 기반한 RAG보다 경우에 따라 엔티티 간의 관계를 따라 가다보면 더 정확하고 상세한 답변을 도달할 수 있습니다. 

GraphRAG 는 **"그래프 기반의 정보 관계 모델링"**을 더한 것입니다.   



# Flow 

📄 Raw Documents
↓
📦 1. 정보 추출 (Entity + 관계 추출)
↓
🕸️ 2. Knowledge Graph 구성 (예: NetworkX, Neo4j)
↓
🔍 3. Graph-based Retriever (유사 노드/경로 탐색)
↓
🧠 4. LLM + Prompt Template (질의 기반 응답 생성)
↓
🤖 5. GraphRAG 파이프라인

## Graph 구축 

📄 Raw Documents
↓
🧠 Named Entity Recognition (NER)
↓
🔁 Entity Resolution & Alignment  ← 💡 지금 여기!
↓
📊 Graph 구축


## 관계가 중요한 문서 

- 보험, 법률, 의학 

## Entity 업데이트에 대한 고민 

- 도메인에 대한 이해에 대한 개념을 가지고 있는 도메인 전문가 필요 

## Vector Embedding 참조 

- 참조에 대한 References를 하기가 쉬움  

## Test Instance 에 대한 고민

## Entity Alignment, Resolution

## Graph Schema 

- UseCase를 시용해서 제안하는 방식임. 

## Ontology Evaluation

## 고민해야할 부분 

- 표를 그래프로 표현하는 방식 
  - 메타 데이터 통합 => Scalability & Heterogeneous graph 데이터 관리를 위해 적절한 GDBMS 선택 필요
  - 그래프의 깊이가 너무 깊어지지 않게 기획의 방향이 중요하다. 
  - Text2SQL 잘하는 곳에서 아이디를 차용해야 한다. 
- 문서를 그래프로 표현하는 방식 
  - 문서를 보는 도메인 전문가들의 Indexing& Search 방식을 이끌어 내어, 이를 keyword >> llm with prompt 발견해서 적용  
  - LLM Prompt를 활용해서 Meta Dictionary를 만들어내서 키워드를 추출하여 Entity를 만들어 내는 것 
  - [Neo4J Specific Graph Examples](https://neo4j.com/graphgists/?github-neo4j-contrib%2Fdesign-patterns%2F%2FDesign-Patterns.adoc)
- 온톨로지(entity, relationship)도 모델별로 가지각색 자체 POC와 프롬프트 엔지니어링을 거친뒤 각 domain에 맞는 LLM 활용이 필요함. 
- 그래프 검색 
  - Text2cypher 
    - VectorDB Input 
    - GraphDB Input 
    - [Link](https://graphrag.com/reference/graphrag/text2cypher/)
    - Graph COT Prompt 
    - graphqachain의 include, exclude parameters 기억 나시나요? schema tuning을 위해 활용합니다. 
    - multi-hop search 
      - Hop을 거듭할수록 조회해야할 Row 들이 많아진다. 
        - 조회를 위한 최적의 환경을 만들어주자! 
      - Neo4J 기준 
        - Memory Configuration 
        - Index Configuration 
        - Tuning of the garbage collector 
        - Bolt Thread Pool 
        - Linux File System 
        - Disk, Ram, ~ Caching Skill 
        - Statistics And Execution Plans 

## 실패를 하는 이유 

- 성능의 문제 

# 변화 흐름 

- GraphRAG
- G-Retrieval
- LightRAG
  - https://github.com/HKUDS/LightRAG
- LazyGraphRAG
- GFM-RAG
  - [Graph Foundation Model](https://github.com/RManLuo/gfm-rag)  
- PathRAG

# 활용 툴 

- [Neo4J](https://wikidocs.net/book/3724)
- [PIKE-RAG](https://github.com/microsoft/PIKE-RAG)
   - [Pipeline](https://github.com/microsoft/PIKE-RAG/blob/main/docs/source/images/readme/pipeline.png) 
- [Langchain Graph](https://python.langchain.com/docs/integrations/retrievers/graph_rag/)
- [Text2SQL](https://medium.com/pinterest-engineering/how-we-built-text-to-sql-at-pinterest-30bad30dabff)
- [Pinterest Query Book](https://github.com/pinterest/querybook/tree/1f14756b2ff08b6b9decb4b1d9f5561ac82d2ea3/querybook/server/lib/ai_assistant/prompts)
- [Example Selectors](https://python.langchain.com/docs/how_to/#example-selectors)
- [Neo4J Specific Graph Examples](https://neo4j.com/graphgists/?github-neo4j-contrib%2Fdesign-patterns%2F%2FDesign-Patterns.adoc)
- [Text2Cyphers](https://neo4j.com/labs/neodash/2.4/user-guide/extensions/natural-language-queries/)