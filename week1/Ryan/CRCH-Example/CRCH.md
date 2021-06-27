![](https://images.velog.io/images/ryan-son/post/ec320479-de67-4f3e-b9c0-457b3bd7e0c7/image.png)
source: [Apple Developer](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1)


위의 그림은 `Content compression`과 `Content hugging`이라는 개념을 소개하고 있습니다.
컨텐츠가 줄어들지 않으려고 버티는 힘을 `Compression Resistance (CR)`, 늘어나지 않으려 버티는 힘을 `Content Hugging (CH)`라고 합니다. 예시를 위해 아래와 같은 상황을 가져왔습니다.

## 예시 1
![](https://images.velog.io/images/ryan-son/post/070218d2-a205-43de-bf27-ac8aa9212b53/image.png)
`Stack View`에 네 개의 버튼이 들어가 있습니다. `Stack View`의 `Distribution`이 `Fill`으로 설정되어 있어 `Stack View`의 크기를 맞추기 위해 버튼 하나의 크기가 다른 버튼보다 길어져 있는 상황입니다. 첫 번째 버튼이 아니라 네 번째 버튼의 크기를 늘리고 싶다면, 첫 번째부터 세 번째 버튼까지의 `CH Priority`를 네 번째 버튼보다 크게 만들면 됩니다. 
![](https://images.velog.io/images/ryan-son/post/4f0a1905-23df-4c84-bf69-31cbcde57639/image.png)
첫 번째부터 세번째 버튼의 `CH Priority`: 500
네 번째 버튼의 `CH Priority`: 250

반대로 복수의 요소 사이에서 특정 요소의 크기가 작아지지 않아야 할 때 해당 요소의 `Compression Resistance (CR)`를 다른 요소보다 크게 만들어 대응할 수 있습니다.

## 예시 2
다양한 상황에서의 CRCH 예시를 가져왔습니다. 동일한 Stack View 내에서 동일한 CRCH Priorities를 가지고 있을 때는 어떤 컨텐츠의 크기가 늘어나고 줄어들지에 대한 가늠을 하기 어렵지만, CRCH Priorities를 설정함으로써 Stack View 내의 요소 간의 크기 충돌이 발생할 때 어떤 요소의 크기의 줄어듦을 방지 (CR)할 것인지, 어떤 컨텐츠의 크기의 늘어남을 방지 (CH)할 것인지를 설정할 수 있습니다.
아래 예시에서는 가로(horizontal) 방향의 CRCH만 비교하였으며 세로(vertical) 방향에서 일어나는 현상도 동일하게 적용됩니다.
![](https://images.velog.io/images/ryan-son/post/e29ccc91-b27e-4b21-8206-1243e82ab53d/image.png)