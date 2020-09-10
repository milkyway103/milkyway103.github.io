---

layout: post
title: <누구든지 하는 리액트: 초심자를 위한 react 핵심 강좌> 3. 리액트의 Virtual Dom
categories : javascript
tags : javascript react library

comments : true
---

> 인프런 강의 <누구든지 하는 리액트: 초심자를 위한 react 핵심 강좌> by velopert

We built React to solve one problem: building large applications with data that changes over time.
우리는 지속해서 데이터가 변화하는 대규모 애플리케이션을 구축하기 위해 리액트를 만들었습니다. (지금은 안 적혀 있음)

- 리액트가 유일한 해답은 아님
- MVC, MVVM, MVW 등을 사용하던 기존 웹 프레임워크/라이브러리
공통점: Model<- 양방향 바인딩을 통해 Model과 View의 값 싱크를 맞춰줌

- **변화(Mutation)** -> 특정 이벤트가 발생했을 때, 모델에 변화를 일으키고, 모델이 어떤 변화를 일으킴에 따라 어떤 돔을 가져와서 어떠한 방식으로 뷰를 업데이트해 줄 지 로직을 정해줘야 함 (우리가 직접 정해준다기보단, 프레임워크/라이브러리에서 정해 줌)
- Mutation을 하지 말자. 그 대신에, 데이터가 바뀌면 그냥 뷰를 날려버리고 새로 만들어버리면 어떨까? (Blow It all away!)

- 브라우저는 돔 기반 작동 -> 페이지가 그때그때 새로운 뷰를 만들려고 하면 성능적 문제

- **Virtual DOM!**
변화가 일어나면 JS로 이루어진 가상의 돔에 한 번 렌더링하고 기존의 돔과 비교한 후 정말 변화가 필요한 곳에만 변화해줌
-> 어떻게 업데이트할지 고려 X
-> 일단 바뀐 데이터로 그려놓고 비교
-> 바뀐 부분만 찾아서 비교

### 참고
브라우저 렌더링: [자바스크립트 브라우저 렌더링](https://12bme.tistory.com/140)
ReactJS의 Virtual DOM과 Repaint, Reflow : [ReactJS의 Virtual DOM과 Repaint, Reflow | DrakeJin](http://blog.drakejin.me/React-VirtualDOM-And-Repaint-Reflow/)
