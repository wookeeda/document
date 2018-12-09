# 고객 접점을 담당하는 개발팀의 기민함에 관하여
- 신정호 / 우아한 형제들


## 고객 접점에서 발생하는 생각하지 못한 일 들
1. 크롤링
2. 해킹
3. abusing - ci/di
4. 시각장애자 고려
5. 살아서 움직이는 트래픽

- 고객을 의심하면 안된다.
- 문제 해결을 위해 구현에 의존하지 않아도 된다.
    - 디자인으로 해결하거나
    - 무조건적으로 개발로만 해결하려하지 말자
- 사용자의 접근을 막으면 안된다.


- api gateway로 막아 보자
    - msa 방식으로 변경이 필요한가
- 점진적 update
    - 앱 10%, 30%, 50%, 100%
    - 서버 : 카나리 배포
- 부분 장애가 전체 장애로 인식되지 않게 해야 한다.
    - (사용자의 접근을 막으면 안된다)와 연결해서 생각해보라.
- 테스트, 품질 책임자의 초기 투입

## 고객 접점에서 발생하는 문제들을 해결하는 3가지
1. 서비스 개선
    - 4가지 측면으로 분류 : 정책적 / 시스템적 / 디자인적 / 마케팅
    - 개발 흐름을 알아야 함 : 가설 > 시나리오 > 정책 > 요구사항 > 구현
    - 지속적 개선 / 빅뱅 개선 : 좋은 방법을 선택
        - 지속적 개선
            - 디자인/시스템 개선에 효과적
            - 정책, 마케팅 개선이 처리할 사람보다 적을 때는 지속적 개선
        - 빅뱅 개선 : 정책/마케팅 개선에 효과적

2. 우선순위 결정
    - 장애대응 > 보안 > 시장대응
    - 팀 리더 우선순위 : 파악 -> 판단 -> 책임
        - 파악은 쪼기 위해서가 아니라
        - 올바른 판단을 위해서
        - 책임감(responsibility)
        - 팀리더의 파악 미흡 -> 판단 실패 -> 책임 전가
    -우선순위가 변경되면?
        - 담당자에게 이해를 시켜야 함.
        - 영향 범위 파악 필요

3. 팀 리듬
    - 단계를 파악하는 것이 중요
        - 생성 : 존재의 이유
        - 확장 : 퍼포먼스와 쌓여있는 일
        - 분할 : 효율
            - 파트로 분리
        - 해체 : 분할 효율 증가
            - 보통 분할 후 해체하지 않는다 -> 파트의 독립을 지향
    - how
        - 마이크로 매니지먼트
            - 단계에 따라 필요 : forming, storming 필요 / 분할, performing 불필요
            - 모두의 commit을 확인하나, 사람에 따라 확인해야 할 깊이는 다르다.
                - 파트로 나뉘면 역할 위임
        - 역할 조직과 위계 조직 : 상황에 따라
    

## 관념적인 것에 매몰되는 위험함
    - 프로젝트 진행방식
        - 목적 없는 회의, 독자 없는 문서, 의미 없는 분석자료, 일정 없는 이슈, 쓸 데 없는 코드
    - 형상관리
        - 브랜치, 데일리 커밋, 페어프로그래밍
    - 구현원칙
        - 바본단계 경험은 부끄러운 것이 아니다.
        - 아키텍처, 디자인 패턴 등 악마는 디테일에 있다.