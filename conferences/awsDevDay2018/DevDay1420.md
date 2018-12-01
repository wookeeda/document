AWS lambda 조성열

## serverless advantage
* server 관리 필요 없음
* 규모 자동 조정
* 밀리초 단위 사용
* 높은 가용성 및 자동 복구

## event source
- data 변화
- 직접 호출
- 리소스 상태 변화
- cloudwatch
- cron

## CI CD
    1. repo commit
    2. build test
    3. stage, production 모든 환경을 동일하게 해야함

## 같이 쓰는 services
    lambda
    api gateway
    sns
    sqs
    s3
    kinesis
    dynamodb
    rds

## cloudformation
    infrastructure as a code
    json/yaml format template

    SAM
    AWS SAM cli : 로컬에서 테스트
    

## CI/CD
- codepipeline
- codecommit
- codebuild
- x-ray or 3rd party tools : 분석 및 디버깅
- codedeploy or sam

## demo
1. S3
2. make text file : data.txt
3. upload to S3
---
4. another S3 : codebuild가 사용 output file 저장
5. codecommit
6. make lambda function : index.js
7. upload to codecommit
8. make samTemplate : samTemplate.yaml
9. upload to codecommit
10. make codebuild spec : buildspec.yaml
11. upload to codecommit
---
12. make role for codebuild
13. make role for cloudformation
---
14. make codePipeline
15. source : codecommit
16. build : make codebuild
17. create role for codePipeline


## tips
- cold start vs warm start
- cloudwatch time-based event 주기적인 warm-up
- code package size 최소화
- lambda handler / core logic 분리
- execution context 재활용
- environment variable 활용
- 자바 .jar 파일은 /lib 디렉토리
- 사용하지 않는 lambda function 정리
- api 스로틀링 대비 : exponential back-off algorithm
- VPC는 반드시 필요한 경우만
- cloudwatch custom metrics를 통해 lambda function 필요한 지표 기록
- x-ray로 lambda function trace

## issues
1. 실패 혹은 실행이 안될 때 : cloudwatch log 에러 확인, timeout exceeded memory exceeed
2. 시간 오래 걸림 : cold start, memory 확장, third party에서 시간이 소요되기도 함.
3. 스로틀링 : 동기식호출->429, 비동기식호출->6시간 재시도, 풀이벤트소스(kinesis,dynamo DB)->7일 재시도
4. schedule 제 때 동작 X : 대한민국 표준시가 아니라 UTC 시간으로 설정하였는지 확인, second level of precision 지원하지 않음->정확한것을 원할때는 EC2
5. VPC에 람다 생성 불가 : 권한, subnet ID, security group 확인


