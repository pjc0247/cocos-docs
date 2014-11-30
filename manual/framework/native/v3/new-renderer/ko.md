# 새 버전의 렌더러를 소개합니다.

## 개요
이번 글에서는 Cocos2d-x v3.x의 새 렌더링 파이프라인을 개발자의 관점에서 살펴보도록 하겠습니다. 이 글은 코어 엔진 팀의 [로드맵](https://docs.google.com/document/d/17zjC55vbP_PYTftTZEuvqXuMb9PbYNxRFu0EGTULPK8/edit)을 대신하기 위해 쓴 것이 아닙니다.

**주의:** 이 글에서는 새로운 렌더링 파이프라인의 대부분의 디테일한 부분에 대해서는 다루지 않습니다. 만약 프로젝트에 기여하기 원한다면 로드맵 문서를 참고해주세요.

먼저, 새 렌더링 파이프라인의 목표에 대해서 알아보겠습니다.

## 목표
Cocos2d-x v3.x의 새로운 렌더링 파이프라인은 최신의 멀티 코어를 지원하는 CPU들로부터 최대한의 속도를 끌어내는 것을 목표로 합니다.

그러면서도 기존 2.x버전 유저들에게 부담을 주지 않기 위해서 새 버전의 API 스타일은 구버전과 호환되도록 작성되었습니다.

## 목표
새 렌더러에서 목표하는 새 기능과 성능 향상들은 요약하자면 아래와 같습니다.

- 씬 그래프와 렌더러의 분리
- 절두체 컬링 (Viewing frustum Geometry culling)
- 렌더링을 분리된 쓰레드에서 수행
- 자동적 batching
- (Node 기반의) 커스터마이즈 가능한 렌더링
- 2D를 위한 최적화, 하지만 3D에도 적합하도록

## 로드맵
Cocos2d-x v3.0beta부터 렌더러는 씬 그래프와 분리되었고, 자동적 batching과 자동 컬링을 지원합니다.

새 렌더러의 완전한 로드맵은 [여기](https://docs.google.com/document/d/17zjC55vbP_PYTftTZEuvqXuMb9PbYNxRFu0EGTULPK8/edit#heading=h.dii2kgdfqgcp)서 보실 수 있습니다.

## 새 렌더링 아키텍처의 개요
위에서 말했듯이, 렌더링 API는 **RenderQueue**를 통해 분리된 렌더링 쓰레드로 전달됩니다. 이런 OpenGL 그래픽 명령들은 그래픽 카드에 직접적으로 전송되게 됩니다.

아래에 구조에 대한 설명을 담은 그림이 있습니다.

![architecture](./res/architexture.png)

씬 그래프가 동작하는 동안 front-end 쓰레드에서는 다양한 **Command**들을 발생시키게 됩니다. 각각의 커멘드들은 분리된 back-end 쓰레드에서 실제로 처리되기 전까지 CommandQueue에 전송되서 대기하게 됩니다.

커멘드의 형식에 대한 설명은 이 문서의 범위를 넘어서기 때문에 로드맵 문서를 참고해 주세요.

만약에 커스텀 drawing 코드를 동반한 Sprite를 생성하는 것 처럼 Cocos2d-x v3.x엔진을 확장하고 싶다면, 기존의 방식대로 **draw()**메소드에 코드를 넣는 것을 통해서는 더 이상 할 수 없습니다.

You should be familiar with the corresponding **Command** and construct the Command on demand in the **draw()** function.

자세한 정보를 얻고 싶다면 아래의 링크를 참고하세요.<br> [DrawNode](https://github.com/cocos2d/cocos2d-x/blob/develop/cocos/2d/CCDrawNode.cpp) built-in with Cocos2d-x v3.0beta.

## 요약
아직 Cocos2d-x v3.0의 새로운 렌더러는 개발 단계이기 때문에 완성되기까지 조금 더 시간과 인내가 필요합니다. 중요한 의견과 코멘트 혹은 pull-request를 통한 기여를 해주시면 감사하겠습니다.<br>
[github 링크](https://github.com/cocos2d/cocos2d-x)
