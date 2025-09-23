
## ウィジェット内で更新アドレスを設定する場合、GETリクエストメソッドは以下の形式のコンテンツを返す必要があります
```Json
{
  "large" : { // 大型ウィジェット
    "result" : [ 
      {
        "group" : "合計",
        "lines" : [
          13495
        ],
        "result" : [ // 固定されていないデータ長
          {
            "name" : "例1",
            "sort" : 0,
            "unity" : 1000,
            "value" : 10999
          },
          // ...  残りは上記と同様
        ],
        "sort" : 0,
        "type" : "pie"
      },
      {
        "group" : "示例1",
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