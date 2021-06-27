# Hugging vs Resistance

---

## content hugging

- 늘어나지 않으려는 힘
- huging priority가 높으면 높을 수록 해당 컨텐츠의 늘어나지 않는 힘이 강해집니다.
  - 늘어나면 보기 싫은 컨텐츠가 있다. 그런 경우 hugging priority를 높게 설정하면 된다.
- **최대 크기에 대한 제약**



위 내용을 다른 뜻으로 해석하자면:

#### 주어진 크기보다 작아질 수 있다[intrinsic size보다는 크게]. 

이러한 이유 때문에 

- 특정한 객체의 content hugging의 priority가 주변 제약의 priority 보다 높으면 주변 객체의 영향을 받아 주어진 크기보다 커질 수 있습니다.

- 반대로 content hugging 의 priority가 주변 제약의 priority보다 낮으면 주어진 크기보다 커집니다.

## Compression resistance

- 외부에서 주는 힘을 버티려는 힘
- resistance priority가 높으면 높을 수록 해당 컨텐츠는 외부 힘에 의해 줄어들지 않는 힘이 강해진다.
  - 왼쪽에 있는 컨텐츠가 아무리 커져도 오른쪽에 있는 컨텐츠의 resistance priority가 높을 경우 왼쪽 컨텐츠영역을 침범할 수 없다.
- **최소 크기에 대한 제약**



다른 의미로는

#### 주어진 크기보다 커질 수 있다.

이러한 이유 때문에

- 특정 객체의 compression resistance의 priority가 다른 제약 priority 보다 높으면 주어진 크기보다 커집니다.
- 반대로 priority가 낮으면 주어진 크기보다 커질 수 없습니다!



내용을 종합 해 보자면



hugging은 최대 크기에 대한 제약을 가리키고 resistance는 최소 크기에 대한 제약을 가리킵니다. hugging의 priority가 다른 제약 priority보다 높으면 주어진 크기보다 작아질 수 있고 resistance의 priority가 다른 제약 priority보다 높으면 주어진 크기보다 커질 수 있습니다



[참고]:

[Content hugging vs Compression resistance 차이점 알기!](https://ontheswift.tistory.com/21)