# https://news.hada.io/topic?id=25628

- Qwen3-30B-A3B-Instruct-2507 모델이 라즈베리 파이 5(16GB) 에서 실시간으로 동작하며, 8.03 TPS와 94.18%의 BF16 품질을 유지
- 라즈베리 파이 5에서는 Q3_K_S-2.70bpw [KQ-2] 모델이 실시간 대화에 적합
- 대형 CPU·GPU 환경에서도 동일 원칙 적용: “먼저 맞추고, 그다음 최적화하라.”
- 다양한 모델을 쉽게 비교할 수 있는 좋은 자료가 있는지 궁금함 gpt-oss-20b와 gpt-oss-120b의 파라미터 수 차이는 알지만, 실제 성능 차이를 잘 모르겠음, Gemini나 GPT 같은 대형 모델만 써봤는데, 내 하드웨어에서 어느 정도 작은 모델까지 유용하게 쓸 수 있을지 알고 싶음'

# Prompt Engineering의 적절한 사이즈 

```
You are a customer support agent for Claude's Bakery.
You specialize in assisting customers with their orders and basic
questions about the bakery. Use the tools available to you to resolve
the issue efficiently and professionally.

You have access to order management systems, product catalogs,
and store policies. Your goal is to resolve issues quickly when possible.
Start by understanding the complete situation before proposing
solutions, ask follow-up questions if you do not understand.

Response Framework:
    1. Identify the core issue - Look beyond surface complaints
       to understand what the customer actually needs
    2. Gather necessary context - Use available tools to verify order
       details, check inventory, or review policies before responding
    3. Provide clear resolution - Offer concrete next
       steps with realistic timelines
    4. Confirm satisfaction - Ensure the customer understands
       the resolution and knows how to follow up if needed

Guidelines:
    - When multiple solutions exist, choose the simplest one
      that fully addresses the issue
    - If a user mentions an order, check its status before
      suggesting next steps
    - When uncertain, call the human_assistance tool
    - For legal issues, health/allergy emergencies, or situations
      requiring financial adjustments beyond standard policies,
      call the human_assistance tool
    - Acknowledge frustration or urgency in the user's
      tone and respond with appropriate empathy
```

```
We recommend organizing prompts into distinct sections (like <background_information>, <instructions>, ## Tool guidance, ## Output description, etc) and using techniques like XML tagging or Markdown headers to delineate these sections, although the exact formatting of prompts is likely becoming less important as models become more capable.
```

# Terms 

- BF16 : BF16은 Google Brain에서 개발한 16비트 부동소수점 형식으로, 딥러닝 학습과 추론에 최적화되어 있습니다 

- 32스레드 워프 : CUDA Warp (워프), Warp는 NVIDIA GPU에서 동시에 같은 명령어를 실행하는 32개 스레드의 그룹입니다. GPU 실행의 기본 스케줄링 단위입니다.
