---
title: Flex
date: 2019-10-18 17:47
category: CSS
---

# flexbox란?
뷰포트나 요소의 크기가 불명확하거나 동적으로 변할 때에도 효율적으로 요소를 배치,정렬,분산할 수 있는 방법을 제공하는 CSS3의 새로운 레이아웃 방식.

* '복잡한 계산 없이 요소의 크기와 순서를 유연하게 배치할 수 있다'

## flexbox의 구성
flexbox는 복수의 자식 요소인 flex item과 그 상위 부모 요소인 flex container로 구성된다.

![flexbox구성] (https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_01.png)

```css

.flex_container {
    display : flex;
}

```

flex container의 flex-direction 속성으로 주축 방향을 결정한다.
디폴트는 row가 적용되고 column은 주축방향을 위에서 아래 방향으로 흐르게 한다.

* flex container 속성 : flex-direction, flex-wrap, justify-content, align-items, align-content
* flex item 속성 : flex, flex-grow, flex-shrink,flex-basis,order

flex-direction 

![flex-direction](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_05.png)


