# Get / Post 속도
- get이 post보다 빠르다
- 왜? get 요청은 캐싱(한 번 접근 후, 또 요청할 시 빠르게 접근하기 위해 데이터를 저장시켜 놓는다)
- 웹 캐싱.. 이거 나중에 또 나올 것 같은데.

# Hub & Switch

# Web socket ?


# Long polling & Streaming


# 3-way Handshake

# HTML/2
- 성능향상에 초점을 맞춘 protocol
- Multiplexed Streams
    - 한 커넥션으로 동시에 여러개의 메세지를 주고 받을 수 있음
- Stream Prioritization
    - image 파일보다 css 파일의 수신이 늦어지면 렌더링이 늦어짐
    - 리소스간 의존관계(우선순위)를 설정할 수 있음
- 어떻게? binary encoding -> stream message에 매핑
- push promise