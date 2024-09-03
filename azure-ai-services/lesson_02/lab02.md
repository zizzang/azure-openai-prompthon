---
title: 'Lab 02: LLMs Orchestration Flows 구축하기'
layout: default
nav_order: 3
---
#### LLMs Orchestration Flows 구축하기

LLM 앱을 위한 프롬프트 플로우 오케스트레이션 구축 방법을 배워보세요.

#### 사전 요구 사항

AI 허브 리소스와 AI 검색 서비스를 생성할 수 있는 Azure 구독이 필요합니다.

#### 설정

이 랩을 수행하기 전에 이미 1번 레슨을 마쳤다면 이 단계는 걱정하지 않아도 됩니다. 그렇지 않은 경우, **1번 레슨**의 **설정**을 따라 Azure AI Studio에서 프로젝트와 관련 리소스를 생성하고 GPT-4 모델을 배포하세요.

#### 랩 단계

이 랩에서는 다음 단계를 다룹니다:

1) 표준 분류 플로우 생성하기
2) 대화형 RAG 플로우 생성하기

##### 1) 표준 분류 플로우 생성하기

웹 브라우저를 열고 다음 주소로 이동하세요: https://ai.azure.com

설정 중에 생성한 AI 프로젝트를 선택한 다음, **Build** 메뉴에서 **프롬프트 플로우** 옵션을 선택하고 파란색 **생성** 버튼을 클릭하세요.

![LLMOps Workshop](images/17.12.2023_22.11.22_REC.png)

인터넷의 웹 사이트를 분류하기 위한 표준 플로우를 생성합니다.

플로우 생성 창에서 **탐색 갤러리** 섹션에서 **표준 플로우** 필터를 선택하세요.

그런 다음, 웹 분류 상자에서 **복제** 버튼을 클릭하세요.

![LLMOps Workshop](images/17.12.2023_22.12.07_REC.png)

플로우의 이름은 기본값으로 사용하거나 원하는 이름을 정의하고 **복제**를 클릭하세요.

![LLMOps Workshop](images/04.01.2024_19.22.29_REC.png)

다음 구조를 가진 표준 플로우가 생성됩니다:

![LLMOps Workshop](images/17.12.2023_22.14.04_REC.png)

플로우에는 다섯 개의 노드가 있습니다. 첫 번째 `fetch_text_content_from_url`은 웹 페이지에서 텍스트를 추출하는 파이썬 노드입니다.

그런 다음 추출된 콘텐츠는 LLM 노드 `summarize_text_content`의 입력으로 사용되어 콘텐츠를 요약합니다.

요약은 파이썬 노드 `prepare_examples`에서 생성된 분류 예제와 결합되어 분류가 수행되는 LLM 노드 `classify_with_llm`의 입력으로 사용됩니다.

마지막으로, 플로우의 출력을 파이썬 사전 형식으로 포맷팅하는 Python 노드 `convert_to_dict`가 있습니다.

플로우가 생성되었으므로, 프롬프트 플로우에서 실행할 런타임이 필요합니다.

플로우를 실행하기 위해 런타임 드롭다운에서 **시작**을 선택하여 런타임을 시작하세요:

![LLMOps Workshop](images/04.01.2024_19.35.49_REC.png)

런타임을 시작한 후, 각 LLM 단계에 대한 LLM과의 연결을 정의해야 합니다. 우리의 경우, 이는 `summarize_text_content`와 `classify_with_llm`입니다.

Azure AI 프로젝트 설정 시 생성된 Azure OpenAI 리소스에 연결하는 `Default_AzureOpenAI` 연결을 사용합니다.

`gpt-4`를 `deployment_name`으로 선택하세요. 이 배포는 설정 중에 생성되었습니다.

![LLMOps Workshop](images/17.12.2023_23.49.29_REC.png)

`classify_with_llm` 단계에도 동일한 연결을 연결하세요:

![LLMOps Workshop](images/17.12.2023_23.58.57_REC.png)

> 참고: `response_format` 필드를 비워둘 수도 있고 `{"type":"text"}`를 선택할 수도 있습니다.

런타임이 선택되고 연결이 구성되면 페이지 상단의 **실행** 버튼을 클릭하여 플로우를 시작할 수 있습니다.

플로우 실행에 필요한 입력은 입력 섹션에 지정되어 있습니다.

실행을 완료한 후, 모든 단계가 포함된 완료된 플로우를 볼 수 있습니다.

마지막 노드를 클릭하여 처리 결과를 확인할 수 있습니다.

##### 2) 대화형 RAG 플로우 생성하기

이제 RAG 패턴을 사용하여 대화형 플로우를 생성합니다. **Build** 탭의 **Tools** 섹션에서 **프롬프트 플로우** 항목에서 새로운 플로우를 생성하세요.

**데이터를 사용한 다중 라운드 Q&A** 템플릿을 선택한 다음 **생성** 버튼을 클릭하세요.

![LLMOps Workshop](images/18.12.2023_00.13.52_REC.png)

**복제** 버튼을 클릭하세요. 다음 구조를 가진 플로우가 생성됩니다.

![LLMOps Workshop](images/26.02.2024_10.00.05_REC.png)

**런타임** 드롭다운에서 **시작**을 선택하여 자동 런타임을 시작하세요. 런타임은 플로우 작업에 유용합니다.

![LLMOps Workshop](images/13.03.2024_10.31.21_REC.png)

플로우를 저장하기 위해 **저장** 버튼을 클릭하세요.

![LLMOps Workshop](images/13.03.2024_01.22.07_REC.png)

###### 2.1) 플로우 개요

첫 번째 노드인 `modify_query_with_history`는 사용자의 질문과 이전 상호작용을 사용하여 검색 쿼리를 생성합니다. 다음으로, `lookup` 노드에서 벡터 인덱스를 사용하여 벡터 저장소 내에서 검색을 수행합니다. 검색 과정 이후, `generate_prompt_context` 노드는 결과를 문자열로 통합합니다. 이 문자열은 `Prompt_variants` 노드의 입력으로 사용되어 다양한 프롬프트를 구성합니다. 마지막으로, 이러한 프롬프트는 `chat_with_context` 노드에서 사용되어 사용자의 답변을 생성합니다.

###### 2.2) 검색 인덱스

플로우를 실행하기 전에, 검색 단계의 검색 인덱스를 설정하는 것이 중요한 단계입니다. 이 검색 인덱스는 Azure AI 검색 서비스에서 제공됩니다.

AI 검색 서비스는 이 랩의 **설정** 섹션에서 생성되었습니다. 아직 검색 서비스를 생성하지 않았다면, 아래에 설명된 대로 생성해야 합니다. 검색 서비스를 생성한 후, 인덱스를 생성할 수 있습니다.

우리의 경우, **벡터 인덱스**를 생성할 것입니다. 이를 위해 AI 스튜디오의 프로젝트로 돌아가 **인덱스** 옵션을 선택한 다음 **새 인덱스** 버튼을 클릭하세요.

![LLMOps Workshop](images/07.02.2024_10.41.56_REC.png)

`소스 데이터` 단계에서 `파일/폴더 업로드` 옵션을 선택하고 이 랩의 데이터 폴더에 있는 PDF `files/surface-pro-4-user-guide-EN.pdf`를 업로드하세요.

![LLMOps Workshop](images/07.02.2024_10.42.40_REC.png)

`인덱스 저장소`에서 이전에 생성한 검색 서비스를 선택하세요.

> 다른 사람이 대신 AI 검색 서비스를 생성했다면, **Azure AI 검색 서비스 선택** 옵션에서 해당 서비스를 선택할 수도 있습니다.

![LLMOps Workshop](images/07.02.2024_10.56.42_REC.png)

`검색 설정`에서 다음 이미지에 표시된 대로 **이 인덱스에 벡터 검색 추가**를 선택하세요.

![LLMOps Workshop](images/07.02.2024_10.57.15_REC.png)

`인덱스 설정`에서 아래 이미지에 표시된 대로 기본 옵션을 유지하세요.

![LLMOps Workshop](images/07.02.2024_16.39.01_REC.png)

> 참고: 가상 머신 구성을 선택하려면 **권장 옵션에서 선택**을 클릭하세요. 선택하지 않으면 기본 구성이 서버리스 처리를 사용합니다.

그럼, `검토 및 완료` 단계에서 **생성** 버튼을 클릭하세요.

인덱싱 작업이 생성되고 실행을 위해 제출되었으므로, 완료되기까지 잠시 기다려야 합니다.

실행 대기열에 들어간 후 시작되기까지 약 10분 정도 소요될 수 있습니다.

다음 이미지에 표시된 것처럼 인덱스 상태가 `완료`인지 확인한 후 다음 단계로 진행하세요.

![LLMOps Workshop](images/26.02.2024_10.29.13_REC.png)

완료되었습니다! **구성 요소** 섹션의 **인덱스** 항목에서 인덱스를 확인할 수 있습니다.

![LLMOps Workshop](images/13.03.2024_10.47.56_REC.png)

이제 **프롬프트 플로우**에서 생성한 RAG 플로우로 돌아가 `lookup` 노드를 구성하세요.

`lookup` 노드를 선택한 후 `mlindex_content`를 클릭하세요.

![LLMOps Workshop](images/26.02.2024_10.52.27_REC.png)

**생성** 창이 나타납니다. 이 창에서 `index_type` 필드에서 `Registered Index` 옵션을 선택하세요. 그런 다음 방금 생성한 인덱스의 버전 1을 선택하세요. 이러한 선택을 완료한 후 **저장**을 클릭하세요.

![LLMOps Workshop](images/13.03.2024_10.47.05_REC.png)

이제 `lookup` 노드로 돌아가서 `query_type` 필드에서 `Hybrid (vector + keyword)` 옵션을 선택하세요.

![LLMOps Workshop](images/26.02.2024_10.36.22_REC.png)

###### 2.3) 연결 정보 업데이트

이제 LLM 모델과 연결되는 노드의 연결을 업데이트해야 합니다.

먼저, `modify_query_with_history` 노드의 gpt-4 배포와의 연결을 업데이트하세요.

![LLMOps Workshop](images/07.02.2024_19.14.16_REC.png)

그리고 `chat_with_context` 노드의 gpt-4 배포와의 연결을 업데이트하세요.

![LLMOps Workshop](images/07.02.2024_19.15.13_REC.png)

###### 2.4) RAG 플로우 테스트

이제 채팅 플로우를 시작할 준비가 되었습니다. 페이지 오른쪽 상단에 있는 파란색 **채팅** 버튼을 클릭하여 플로우와 상호작용을 시작하세요.

![LLMOps Workshop](images/14.03.2024_15.00.08_REC.png)