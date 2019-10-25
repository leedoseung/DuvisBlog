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

![flexbox구성](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_01.png)

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


flex : 1 속성으로 자식 요소의 크기 확장

flex : 1 => flex : flex-grow 1  flex-shrink 1  flex-basis 0;

flex-grow 속성
flex item의 확장에 관련된 속성.
속성값이 1 이상이면 flex container를 채우도록 flex item의 크기가 커짐
속성값이 0 이면 원래 크기가 유지.

flex-shink
flex item의 축소에 관련된 속성
속성값이 1 이상이면 flex container의 크기가 flex item의 크기보다 작아질 때 flwx item의 크기가 flwx container의 크기에 맞추어 줄어들음.
속성값이 0 이면 원래 크기가 유지.

flex-basis
width 속성에서 사용하는 모든 단위(px, %, em, rem 등)를 속성값에 사용할 수 있다. flex-basis 속성의 값을 30px이나 30%와 같이 설정하면 flex item의 크기가 고정된다.
![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_09.png)

auto와 함께 자주 사용하는 속성 값은 0이다.
속성 값을 0으로 설정하면 flex item은 절대적 flex item이 되어 flex container 기준으로 크기가 결정됨

auto로 설정하면 flex item은 상대적 flex item이 되어 콘텐츠의 크기를 기준으로 크기가 결정된다.

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_10.png)


flex: none 속성으로 자식 요소의 크기 고정

flex 속성의 기본값은 flex: 0 1 auto
flex item의 크기를 고정하려면 반드시 flex: none속성을 적용해야 한다.

flex: none => flex-grow: 0 flex-shrink: 0 flex-basis:auto

flex속성의 값에 따라 flex-item 크기는 다음과 같이 변한다.
* initial(기본값) : flex container의 크기가 작이지면 flex item의 크기도 작아짐. 하지만 flex container의 크기가 커져도 flex item의 크기는 커지지 않는다
* none : flex item 크기 고정
* auto : flex container 크기에 따라
* 양의정수 : flex container를 일정한 비율로 나눠 가지면서 flex container의 크기에 따라 flex item의 크기가 커지거나 작아진다.

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_13.png)

flex-item의 margin

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_14.png)
![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_15.png)


브라우저 화면 아래 푸터

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_16.png)

```css
.flex-container {
  display: flex;
  flex-direction: column;
}

.flex_item {
  margin-top: auto;
}

```

정렬이 다른 메뉴

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_18.png)

```css

.flex-container {
    display: flex;
    justify-content: space-between;
}

```

justify-content 속성은 주축을 기준으로 flex item을 수평으로 정렬

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_19.png)


align-items 속성은 주축을 기준으로 flex item을 수직으로 정렬한다.

![](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_21.png)


중앙 정렬 아이콘

두가지 방법이 있다.

1. flex container에 align-items: center 속성과 justify-content:center 속성을 적용해 아이콘에 해당하는 flex item을 화면 정중앙에 정렬
```css

.flex_container {
  display: flex ;
  align-flex : center;
  justify_content: center;
}

```

2. 아이콘에 해당하는 flex item에 margin: auto 속성을 적용해 아이콘이 화면 정중앙에 위치하게 한다.

```css

.flex_container {
  display: flex;
}

.flex_item {
  margin: auto;
}

```

유동 너비 박스

유동 너비 박스는 flex container인 부모 요소 크기에 따라 flex item인 자식 요소의 크기가 콘텐츠의 크기보다
줄어두는 레이아웃이다.

```css

.flex_container {
  display: flex;
}

.flex_item { 
  max-width: 300px;
}

```

flex item의 크기에 관련된 속성인 flex 속성의 기본값은 0 1 auto다. 그래서 flex container의 크기가 커질 때는 flex item의 크기는 변하지 않지만, flex container의 크기가 작아지면 flex item의 크기가 작아진다.

말줄임과 아이콘


```css

.flex_container {
  display: inline-flex;
  max-width: 100%;
}

.text{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space:nowrap;
}

```

display: inline-flex 속성은 dispaly:inline-block이랑 같은 의미
