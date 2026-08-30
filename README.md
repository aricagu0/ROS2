Review
# 🤖 Nav2 및 Marker 기반 협업배송로봇 시스템
본 프로젝트는 ROS2(Humble)와 TurtleBot3를 기반으로 **Nav2 좌표 주행과 Marker 추종 제어**를 결합하여 구현한 2대의 협업배송로봇시스템 프로젝트입니다.
하나의 로봇에 모든 기능을 집중하는 대신, **Leader 로봇**(좌표 기반 이동 및 물체 취급)과 **Follow 로봇**(선행 로봇 추종 및 적재/리프팅 작업)으로 역할을 분담하여 효율적인 배송 임무를 수행하도록 설계되었습니다.
## 📑 목차 (문서 리스트)
아래의 각 항목을 클릭하시면 해당 상세 문서로 이동합니다.
1. [01. 프로젝트 계획서 (Project Plan)] (Project_Plan.md)
   - 프로젝트 목표, 개발 기간, 인원 및 역할 분담 등 초기 착수 단계의 계획을 담고 있습니다.
2. [02. 시스템 설계 및 개발 (System Design & Development)](./02.%20System_Design_Development.md)
   - 시스템 아키텍처, 하드웨어/소프트웨어 설계, ROS2 노드 구성 및 세부 구현 내용이 포함되어 있습니다.
3. [03. 최종 시나리오 (Scenario Final)](./03.%20Scenario_Final.md)
   - Leader 로봇과 Follow 로봇이 협력하여 수행하는 최종 주행 및 배송 시나리오를 설명합니다.
4. [04. 개발 결과 (Development Result)](./04.%20Development_Result.md)
   - 프로젝트 최종 성과, 트러블슈팅, 향후 개선 방향 등 프로젝트 마무리 결과 보고서입니다.
