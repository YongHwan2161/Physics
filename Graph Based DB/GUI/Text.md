# Align
- windows에서 text를 출력할 때는 rect를 지정하고 그 rect를 기준으로 text를 출력하게 된다. 이 때 text의 정렬이 중요한다. text의 시작 위치가 특정 x, y coordinate가 되도록 하는 방법을 알아야 그 점을 기준으로 왼쪽부터 오른쪽으로 text를 출력할 수 있다. 
- 이 때는 SetTextAlign 함수를 이용한다. ref: https://codingfarm.tistory.com/370
```c
    SetTextAlign(hBufferDC, TA_LEFT);
    TextOutW(hBufferDC, x, y, wzoomText, wcslen(wzoomText));
    TextOutW(hBufferDC, x, y + lineHeight, wpanText, wcslen(wpanText));
    TextOutW(hBufferDC, x, y + lineHeight * 2, wmouseText, wcslen(wmouseText));
    TextOutW(hBufferDC, x, y + lineHeight * 3, wworldText, wcslen(wworldText));
```