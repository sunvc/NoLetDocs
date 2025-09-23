
## 위젯 내 업데이트 주소 설정, 요청 방법 GET으로 다음 형식의 내용을 반환해야 함
```Json
{
  "large" : { // 대형 위젯
    "result" : [ 
      {
        "group" : "총계",
        "lines" : [
          13495
        ],
        "result" : [ // 고정되지 않은 데이터 길이
          {
            "name" : "예시1",
            "sort" : 0,
            "unity" : 1000,
            "value" : 10999
          },
          // ... 나머지는 위와 동일
        ],
        "sort" : 0,
        "type" : "pie"
      },
      {
        "group" : "예시1",
        "lines" : [
          1834
        ],
        "result" : [
          {
            "name" : "a",
            "sort" : 0,
            "unity" : 1000,
            "value" : 565
          },
           // ...  剩下若干同上
        ],
        "sort" : 1
      },
       // ...  剩下若干同上
    ],
    "subTitle" : "总计收到64条",
    "title" : "NoLet"
  },
  "lock" : { // 锁屏组件
    "subTitle" : "总计收到64条",
    "title" : "NoLet"
  },
  "medium" : { // 中号组件
    "result" : [ // 数组长度6
      {
        "name" : "总计",
        "value" : 18
      },
     // 数组长度6，剩下2个数据同上
    ],
    "subTitle" : "总计收到64条",
    "title" : "NoLet"
  },
  "small" : {// 小号组件
    "result" : [  // 数组长度3
      {
        "name" : "总计",
        "value" : 18
      },
     // 数组长度3，剩下2个数据同上
    ],
    "subTitle" : "总计收到64条",
    "title" : "NoLet"
  }
}


```