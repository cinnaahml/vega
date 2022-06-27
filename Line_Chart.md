# Line Chart

```
    {
      "$schema": "https://vega.github.io/schema/vega/v5.json",
      "description": "A basic line chart example.",
      "width": 500,    //可視化的範圍
      "height": 200,
      "padding": 5,

      "signals": [
        {
          "name": "interpolate",
          "value": "linear",
          "bind": {
            "input": "select", //輸入模式
            "options": [   // 選擇圖表格式 下拉是選單
              "basis",
              "cardinal",
              "catmull-rom",
              "linear",
              "monotone",
              "natural",
              "step",
              "step-after",
              "step-before"
            ]
          }
        }
      ],
     // 資料量分兩組,以Key("c")做區分
      "data": [
        {
          "name": "table",
          "values": [
            {"x": 0, "y": 28, "c":0}, {"x": 0, "y": 20, "c":1},
            {"x": 1, "y": 43, "c":0}, {"x": 1, "y": 35, "c":1},
            {"x": 2, "y": 81, "c":0}, {"x": 2, "y": 10, "c":1},
            {"x": 3, "y": 19, "c":0}, {"x": 3, "y": 15, "c":1},
            {"x": 4, "y": 52, "c":0}, {"x": 4, "y": 48, "c":1},
            {"x": 5, "y": 24, "c":0}, {"x": 5, "y": 28, "c":1},
            {"x": 6, "y": 87, "c":0}, {"x": 6, "y": 66, "c":1},
            {"x": 7, "y": 17, "c":0}, {"x": 7, "y": 27, "c":1},
            {"x": 8, "y": 68, "c":0}, {"x": 8, "y": 16, "c":1},
            {"x": 9, "y": 49, "c":0}, {"x": 9, "y": 25, "c":1}
          ]
        }
      ],

      "scales": [ // 設定三個scales 在這做(尺度)
        {
          "name": "x",  //設定x軸尺度名稱
          "type": "point", // 類型: "point"為有序的散佈圖與折線圖
          "range": "width", // 範圍:"width" 來自top的定義
          "domain": {"data": "table", "field": "x"} //定義資料來源:名為"table"的來源，取"x"軸
        },
        {
          "name": "y", //設定y軸尺度名稱
          "type": "linear", //尺度類型為linear scales
          "range": "height", // 範圍:"height" 來自top的定義
          "nice": true, //定量尺度名為"nice": true
          "zero": true, //在此指數值是否等於零
          "domain": {"data": "table", "field": "y"} //定義資料來源:名為"table"的來源，取"y"軸
        },
        {  // 這裡設定為如何辨識兩組資料以顏色區分變數位"c"
          "name": "color", //設定尺度名稱為"color"
          "type": "ordinal", //尺度類型為"ordinal"，主要功用將兩組資料用顏色區分
          "range": "category", //尺度範圍:這裡是用配色來分類
          "domain": {"data": "table", "field": "c"}//定義資料來源:名為"table"的來源，取key="c"
        }
      ],
      "axes": [   // 設定x y 軸顯示的位置
        {"orient": "bottom", "scale": "x"}, //x軸的圖示在底下
        {"orient": "left", "scale": "y"} //y軸的圖示在左邊
      ],

      "marks": [ //圖形標記
        {
          "type": "group", //類型設為組標記
          "from": {  //要標記的數據的來源
            "facet": { //用於區分組標記的指令
              "name": "series",
              "data": "table",
              "groupby": "c" //以key:"c"為基準
            }
          },
          "marks": [ //這裡指文本的標記
            {
              "type": "line",
              "from": {"data": "series"},
              "encode": {
                "enter": {
                  "x": {"scale": "x", "field": "x"},
                  "y": {"scale": "y", "field": "y"},
                  "stroke": {"scale": "color", "field": "c"},
                  "strokeWidth": {"value": 2}
                },
                "update": { // 當數據或顯示屬性更新時，都會調用更新集
                  "interpolate": {"signal": "interpolate"}, //調用"signals"的設定
                  "strokeOpacity": {"value": 1}
                },
                "hover": { //鼠標懸停時調用
                  "strokeOpacity": {"value": 0.5} //當鼠標只到時，顏色會變淺
                }
              }
            }
          ]
        }
      ]
    }
```
