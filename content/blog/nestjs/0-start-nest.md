---
title: 'NestJS 공부를 시작하며...'
date: 2022-06-23 11:04:76
category: nestjs
thumbnail: './images/book1.png'
draft: false
---

![images/book1.png](./images/book1.png)

## NestJS를 사용하기로 했다

이번 프로젝트에 Express를 그냥 쓰면 자유도도 너무 높고 제대로 관리가 안 될 것 같았다. 이번에 `TS`를 무조건 쓰자고해서 Fastify-CLI 같은 것을 써볼까 했지만 DI, IoC랑 커맨드툴로 코드도 생성해주고 다들 편하다는 NestJS를 쓰기로 결정했다.

## 목적

Nest로 시그널링 서버를 구현할 것 같다.
일단 당장은 Websocket을 이용해서 시그널링 서버를 구현해야 한다.
GateWay를 생성해서 할 것 같다.

Pub/Sub구조를 가진 Redis랑 연동하는 것도 해야 한다.
찾아보니깐 Redis Subscribe?? 이런 것이 있더라.

## 할 수 있을까?

Redis도 생소하고 STUN이랑 TURN이랑도 연결하고 해야되는데 갈길이 멀다.
가보자!!! ㅎㅎ
