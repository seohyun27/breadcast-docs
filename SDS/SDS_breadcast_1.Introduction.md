## [ Revision history ]

| Revision date | Version \# | Description                                                                                                                     | Author |
|:--------------|:-----------|:--------------------------------------------------------------------------------------------------------------------------------|:-------|
| 2025/11/07    | 1.00       | 초기 버전 작성                                                                                                                        | 팀 전원   |
| 2025/11/28    | 1.10       | 구현 진도에 맞춰 문서를 수정하였습니다. 가장 큰 수정 사항은 아래 두 가지입니다.<br>1. 엔티티 최적화: bread, classify 삭제 후 Menu로 통합<br>2. API 응답 규격 통일 (ApiResponse 도입) 및 에러 핸들링 적용 | 김서현    |



## = Authors for each section =
| Section                    | Authors |
|:---------------------------|:----|
| Introduction               | 이지원 |
| Use case analysis          | 팀 전원 |
| Class diagram              | 김서현 |
| Sequence diagram            | 김서연, 박세은 |
| State machine diagram      | 김서연 |
| User interface prototype   | 김현지, 노은재, 이지원 |
| Implementation requirements | 김서현 |
| Glossary                   | 김현지, 노은재 |
| References                 | 이지원 |

---

<br>

# 1. Introduction
<img width="216" height="95" alt="Group 321" src="https://github.com/user-attachments/assets/0debf390-81fc-49f7-bda2-97490a9d2d01" />


본 문서는 우리 조가 개발하는 **BreadCast 웹**의 설계 명세서 (System Design Specification, **SDS**)이다.  
BreadCast의 **기능적 요구사항**을 기반으로 하여 시스템을 **구조적·동적 관점**에서 설계하기 위해 작성되었다.<br> 
BreadCast는 **사용자 간 실시간 소통과 콘텐츠 공유**를 중심으로 한 플랫폼으로, 본 문서는 그 구현을 위한 전반적인 설계 방향을 제시한다.

**Use Case Analysis**에서는 사용자 관점에서 소프트웨어가 제공하는 기능을 서술하였다.<br> 
**Use Case Diagram**을 통해 시스템이 어떤 방식으로 동작하는지를 시각적으로 표현하였으며, <br>
**Use Case Description**을 통해 사용자의 행위와 시스템의 반응이 어떻게 연결되는지를 구체적으로 기술하였다.<br>

**Class Diagram**은 시스템의 **정적 구조**를 나타내며 핵심 도메인 모델을 중심으로 기술한다. <br>
또한 **Sequence Diagram**과 **State Machine Diagram**은 시스템의 **동적 동작**을 표현한다.<br>
**User Interface 설계**는 사용자의 시각적 경험과 상호작용 방식을 정의하였으며, <br>
**Figma**를 활용하여 각 화면의 구성, 페이지 전환, 컴포넌트 간 역할을 구체화하였다.<br>

본 문서의 작성 과정에서 가장 중점을 둔 부분은 **다이어그램 간의 일관성(consistency)** 이다. <br>
Use Case, Class, Sequence Diagram 간의 **객체명·메시지·흐름**이 상호 일치하도록 지속적인 검토와 수정을 반복하였다. <br>
변경사항이 발생할 때마다 관련된 문서를 함께 갱신하여 전체 설계의 통일성을 유지하였으며,<br> 여러 차례의 수정 과정을 거쳐 오류와 불일치를 최소화하였다.<br>

본 설계 명세서는 BreadCast 시스템의 **핵심 구조를 모두 포함**하며, 이후 **구현 단계에서 개발자가 참고할 수 있는 기본 청사진** 역할을 수행한다.<br>
