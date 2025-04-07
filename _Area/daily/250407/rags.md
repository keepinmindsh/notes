# 8가지 RAG 기법 

## DeepRAG → ‘DeepRAG: Thinking to Retrieval Step by Step for Large Language Models’

- https://huggingface.co/papers/2502.01142?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

‘검색 증강 추론’ 작업을 마르코프 의사결정 과정으로 취급해서 검색을 전략적으로 수행합니다. 외부 지식을 검색할 시점, 파라미터 추론에 의존할 시점을 다이나믹하게 결정합니다.

## RealRAG → ‘Retrieval-Augmented Realistic Image Generation via Self-Reflective Contrastive Learning’

- https://huggingface.co/papers/2502.00848?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

실제의 이미지를 검색하고 ‘Self-Reflective Contractive Learning (자기 성찰적 대조 학습)’ 방법으로 지식의 갭을 채우면서 현실감을 개선, 왜곡을 줄이는 방식으로 새로운 객체를 자연스럽게 형성할 수 있도록 합니다.

## Chain-of-Retrieval Augmented Generation (CoRAG) → ‘Chain-of-Retrieval Augmented Generation’

- https://huggingface.co/papers/2501.14342?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

정보를 단계별로 검색하고 조정해서, 테스트 시점에 사용할 컴퓨팅 성능을 결정합니다 - 필요한 경우에는 질의를 재구성합니다.

## VideoRAG → ‘VideoRAG: Retrieval-Augmented Generation over Video Corpus’

- https://huggingface.co/papers/2501.05874?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

그래프 기반 텍스트 그라운딩, 그리고 멀티모달 컨텍스트 인코딩을 통합하는 이중의 채널 아키텍처를 사용해서, 길이의 제한 없이 비디오를 처리할 수 있습니다.

## CFT-RAG → ‘CFT-RAG: An Entity Tree Based Retrieval Augmented Generation Algorithm with Cuckoo Filter’

- https://huggingface.co/papers/2501.15098?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

Tree-RAG 가속화 기법은 개선된 Cuckoo 필터를 사용해서 ‘엔티티 지역화’를 최적화, 더 빠른 검색을 가능하게 합니다.

## Contextualized Graph RAG (CG-RAG) → ‘CG-RAG: Research Question Answering by Citation Graph Retrieval-Augmented LLMs’

- https://huggingface.co/papers/2501.15067?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

LeSeGR (Lexical-Semantic Graph Retrieval, 어휘-의미 그래프 검색)을 사용해서 그래프 구조 내의 희소 신호와 밀집 신호를 통합하고 인용 관계를 포착합니다.

## GFM-RAG → ‘GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation’

- https://huggingface.co/papers/2502.01113?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

그래프 신경망을 사용해서 질의-지식 간 연결을 개선해 주는 그래프 기반 모델입니다.

## URAG → ‘URAG: Implementing a Unified Hybrid RAG for Precise Answers in University Admission Chatbots — A Case Study at HCMUT’

- https://huggingface.co/papers/2501.16276?utm_source=turingpost.co.kr&utm_medium=referral&utm_campaign=8-rag

교육용 챗봇을 위해서 규칙 기반 기법과 RAG 기법을 결합한 하이브리드 시스템입니다.