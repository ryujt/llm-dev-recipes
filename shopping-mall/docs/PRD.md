다듬은 문서는 아래와 같습니다:

---

# Product Requirements Document

## 기술 스택

* **Backend**: Node.js, Express
  * 실제 API 대신, 고정된 데이터를 반환하는 Fake API 형태로 구현합니다.

* **Frontend**: React.js
  * 개발 시 `Project Rules for React.js.md`에 정의된 규칙을 반드시 준수합니다.
  * 필수 패키지로는 Tailwind CSS, axios, zustand를 사용합니다.

## 사용자 시나리오

* 상세한 흐름은 `Navigation Scenario.md` 문서를 참고하세요.

## 구현 가이드

* 코드 작성 시 **가독성을 최우선**으로 고려해 주세요.
* 단일 책임 원칙을 적용하여, 하나의 기능마다 별도의 파일로 나누어 구현합니다.
* 모든 CSS 관련 코드는 별도 파일로 분리하여 유지보수가 용이하도록 합니다.
* 실제 서비스를 사용하는 것처럼 느껴질 수 있도록 다음 기준을 지켜 주세요:
  * https://loremflickr.com/320/240/dog 같은 플레이스홀더 이미지를 사용해 시각적인 완성도를 높여 주세요.
  * 실서비스에 가까운 레이아웃 구성과 컴포넌트 배치를 적용해 주세요.
  * 실제 서비스에 들어갈 법한 자연스러운 콘텐츠를 구성해 주세요.