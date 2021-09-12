# Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

---



## Left Constraint

Left, Right는 절대적 방향을 뜻하며 화면의 왼쪽 또는 오른 쪽을 뜻합니다.

따라서 left constraint는 왼쪽 제약이라고 해석할 수 있겟습니다.



## Leading Constraint

Leading, Trailing은 장치별 지역 설정(locale)에 의해서 영향을 받습니다.

읽기 방향이 오른쪽에서 왼쪽인 지역에서는 leading이 오른쪽이되고 trailing이 왼쪽이 됩니다.

반대 한국과 같이 읽기 방향이 왼쪽에서 오른쪽인 지역에서는 leading이 왼쪽, trailing이 오른쪽이 됩니다.

따라서 Leading constraint는 device의 locale에 다라서 달라지기에 특정한 지역에서는 오른쪽 제약을 뜻할 수 있고 도 다른 지역에서는 왼쪽 제약을 뜻할 수 있습니다.



[참고]:

[Difference between leftAnchor and leadingAnchor?](https://stackoverflow.com/questions/32981532/difference-between-leftanchor-and-leadinganchor)