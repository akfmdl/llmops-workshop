# LLMOps Workshop

> **소형 LLM(Qwen 3.5-2B)을 합성 데이터로 SFT → 평가 → 양자화 → 배포 → Agentic RAG까지** 한 번에 체험하는 한국어 LLMOps 실습 워크샵.

Colab 무료 GPU(T4)에서 끝까지 돌아가도록 설계되어 있고, 각 단계가 **하나의 노트북**으로 독립 실행 가능합니다.

---

## 워크샵 구성

| 노트북 | 주제 | 소요 시간 | 핵심 학습 포인트 |
|--------|------|-----------|------------------|
| `00_colab_gemini_guide.ipynb` | Colab 기본 조작 | ~10분 | Colab 환경, 런타임, 드라이브 연동 |
| `01_synthetic_data.ipynb` | 합성 데이터 생성 | ~25분 | GPT-4o-mini로 도메인 특화 SFT 데이터 만들기 |
| `02_sft_train.ipynb` | LoRA SFT 학습 | ~25분 | Unsloth + TRL로 Qwen 3.5-2B 파인튜닝 |
| `03_sft_eval.ipynb` | 4조건 평가 + Langfuse | ~30분 | LLM-as-a-Judge, Catastrophic Forgetting 체크 |
| `04_quantization.ipynb` | GGUF 양자화 | ~25분 | llama.cpp로 FP16 → Q4_K_M, 품질 손실 정량화 |
| `05_gradio.ipynb` | Gradio 데모 배포 | ~20분 | Hugging Face Spaces에 추론 API 공개 |
| `06_agentic_rag.ipynb` | Agentic RAG | ~30분 | Tool calling + 벡터 검색 + Langfuse 트레이스 |

전체 시나리오는 **가상의 이커머스 마켓플레이스 "쇼핑몰"**(자체 PB 라인: 베이직 · 프리미엄 · 라이트 · 한정판)의 고객지원 상담원 톤을 학습시키는 것입니다.

---

## 사전 준비물

| 항목 | 비고 |
|------|------|
| Google 계정 | Colab 무료 GPU(T4) 사용 |
| OpenAI API Key | `01`·`03` 노트북에서 GPT-4o-mini 호출 (~$1) |
| Hugging Face 계정 + Read/Write 토큰 | `02`·`04`·`05`에서 모델 업로드 |
| Langfuse Cloud 계정 (Japan 리전) | `03`·`05`·`06` 트레이싱 (`jp.cloud.langfuse.com`, **한국 사용자는 Japan 리전 필수**) |

> 💡 **Langfuse 리전** — `cloud.langfuse.com`(EU 기본값) 대신 `jp.cloud.langfuse.com`을 사용하세요. 도쿄 리전이 한국에서 가장 낮은 지연을 보이며, 노트북 코드는 `LANGFUSE_HOST`를 명시적으로 jp로 고정합니다.

---

## 빠른 시작

1. 본 저장소를 fork 또는 clone
2. Colab에서 `00_colab_gemini_guide.ipynb` 열고 환경 확인
3. `01` → `06` 순서로 실행

```bash
git clone https://github.com/<your-username>/llmops-workshop.git
```

각 노트북은 **이전 노트북의 산출물(데이터·모델)**을 Google Drive 또는 Hugging Face Hub에서 자동으로 불러오도록 작성되어 있습니다.

---

## 데이터·콘텐츠 정책

- 노트북에 포함된 모든 시나리오·문서·대화·상품 정보는 **가상의 "쇼핑몰"**을 배경으로 한 합성 콘텐츠입니다.
- **외부 사이트 크롤링 코드가 포함되어 있지 않으며**, 모든 학습용 원본 문서는 GPT-4o-mini가 실시간으로 생성합니다.
- 어떤 실제 기업·상품·정책과도 무관합니다. 우연한 유사성이 있더라도 의도된 바 아닙니다.
- 노트북 안에서 SFT 학습된 모델은 "쇼핑몰 고객지원 상담원" 페르소나로 답합니다 → 실명 기업 사칭 우려 없음.

---

## 라이선스

- **코드(Jupyter 노트북, 스크립트)**: [Apache License 2.0](LICENSE)
- 자유롭게 학습·수정·재배포 가능하며 상업적 이용도 허용됩니다.
- 저작권 고지와 변경 사항을 명시해주세요.

본 저장소를 학습·연구에 사용하셔도 좋고, 변형해서 자체 워크샵을 운영하셔도 좋습니다. 어떻게 활용하시든 출처(원 저장소 링크)를 함께 적어주시면 감사하겠습니다.

---

## Trademarks

본 저장소에 등장하는 "쇼핑몰"·"베이직"·"프리미엄"·"라이트"·"한정판" 등은 **교육용 가상 상품 라인**의 명칭이며, 실재하는 상표나 회사를 지칭하지 않습니다.

이 저장소에서 사용된 외부 도구(Hugging Face, OpenAI, Google Colab, Langfuse, Unsloth, llama.cpp 등)의 상표·서비스마크는 각 권리자의 소유입니다. 본 저장소는 해당 권리자와 제휴·후원·승인 관계가 없습니다.

---

## 면책 조항

- 본 자료는 **교육 목적**으로 제공됩니다.
- 노트북을 그대로 또는 변형하여 실제 서비스에 사용함으로써 발생하는 모든 결과는 사용자의 책임입니다.
- 외부 유료 API(OpenAI, Hugging Face Inference 등)를 호출하므로 비용이 발생합니다. 키 관리에 주의하세요.
- 학습된 모델은 합성 데이터로 만들어졌으며, 사실관계 검증 없이 의료·법률·금융 등 중요 의사결정에 사용하지 마세요.

---

## Contributing

이슈·PR 환영합니다. 다음과 같은 기여가 특히 도움이 됩니다:

- 실행 환경별 호환성 보고 (Colab Free/Pro, 로컬 GPU 등)
- 한국어 외 언어 시나리오 추가
- 노트북 셀의 더 명확한 설명·다이어그램
- 새 카테고리(예: DPO, RLHF, multi-agent) 노트북 추가
