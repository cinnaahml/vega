# 😃vega_Bar Charts

1.[Bar Chart](https://vega.github.io/vega/examples/bar-chart/)
2.資料格式:

    {      #以$schema定義架構來源
      "$schema": "https://vega.github.io/schema/vega/v5.json",
      "description": "A basic bar chart example, with value labels shown upon mouse hover.",
      "width": 400,     #設定圖表x軸寬度#
      "height": 200,    #設定圖表y軸高度#
      "padding": 5,     #設定視圖邊框之間的邊距，透過 padding 的來達到水平及垂直置中的效果。
    
      "data": [         #設定資料來源 "數據集"[{"name":"圖表名稱","values":[{},{}]}]#
        {
          "name": "table",  #圖表名稱(可自定義)#
          "values": [       #圖表值 "values"(可使用values/url/source):[{"x軸欄位名稱":"x軸物件項目標籤","y軸欄位名稱":"x軸類別項目標籤的值"}]#
            {"category": "A", "amount": 28}, #定義x軸欄位物件名稱:類別項目標籤,x軸類別項目標籤的值#
            {"category": "B", "amount": 55},
            {"category": "C", "amount": 43},
            {"category": "D", "amount": 91},
            {"category": "E", "amount": 81},
            {"category": "F", "amount": 53},
            {"category": "G", "amount": 19},
            {"category": "H", "amount": 87}
          ]
        }
      ],
    
      "signals": [     #定義事件信號#動態變量，觸發交互更新
        {
          "name": "tooltip",     #跳出提示說明的特效工具名稱
          "value": {},
          "on": [           #on事件啟動
            {"events": "rect:mouseover", "update": "datum"},  #onmouseover 事件發生在鼠標移到一個矩形標記上時，鼠標懸停僅在調用該函數後update起作用:資料綁定基準
            {"events": "rect:mouseout",  "update": "{}"} #onmouseout 事件在鼠標指針移出元素時發生，鼠標懸停移出矩形時，是一個空對象
          ]
        }
      ],
    
      "scales": [       #定義座標表示比例尺，設定垂直及水平軸，轉換數據為圖表(縮放函數將數據值映射到視覺值)
        {
          "name": "xscale",        #設定x軸比例尺物件名稱，xscale
          "type": "band",     #定義類型:band帶
          "domain": {"data": "table", "field": "category"},   #"定量範圍":{"資料來源":"圖表名稱","field":"x軸欄位物件名稱"}
          "range": "width",   #範圍:圖表寬度(width/height)
          "padding": 0.05,    #x座標軸標籤之間的間隔(長條圖間距)，padding<0
          "round": true       #
        },
        {
          "name": "yscale",           #設定y軸比例尺物件名稱，yscale    
          "domain": {"data": "table", "field": "amount"},  #"資料源":{"資料來源":"圖表名稱","field":"x軸項目標籤的值"}#
          "nice": true,
          "range": "height"    #範圍:圖表高度(width/height)
        }
      ],
    
      "axes": [     #定義座標軸標籤的上下左右位置，設定orient:top、right、bottom 或 left 
        { "orient": "bottom", "scale": "xscale" },   #orient:底部,scale:x軸比例
        { "orient": "left", "scale": "yscale" } ##orient:左邊,scale:y軸比例
      ],
    
      "marks": [        #定義標記(通過將標記映射到顏色、大小或形狀。)
        {
          "type": "rect",     #類型選項：條形圖
          "from": {"data":"table"},    #從:{資料源:圖表}
          "encode": {
            "enter": {     #enter是資料的方式
              "x": {"scale": "xscale", "field": "category"}, #設置水平軸，使用scale應用於xscale以獲得category
              "width": {"scale": "xscale", "band": 1},  #設置水平軸的寬度，使用scale應用於xscal，band矩形座標的偏移位置:1(正常為1)
              "y": {"scale": "yscale", "field": "amount"},  #設置垂直軸，定義矩形頂部:使用scale應用於yscale以獲得amount
              "y2": {"scale": "yscale", "value": 0}   #設置y2垂直軸高度，定義矩形底部:{"scale": "yscale",矩形座標的y軸終點值value:0(正常為0)}
            },
            "update": {    #更新矩形顏色，返回到其原始顏色
              "fill": {"value": "steelblue"}  #圖形內部的填滿顏色:steelblue
            },
            "hover": {    #鼠標懸停時矩形更改填充顏色
              "fill": {"value": "red"}      #懸停填充顏色:red
            }
          }
        },
        {
          "type": "text",    #圖形文字
          "encode": {
            "enter": {   #enter是資料的方式
              "align": {"value": "center"},   #對齊:中心
              "baseline": {"value": "bottom"},  #基線:底
              "fill": {"value": "#333"}   #填充:項目值文字顏色#333
            },
            "update": {  #更改時更新設置(返回到其原始顏色)
              "x": {"scale": "xscale", "signal": "tooltip.category", "band": 0.5},  #配置category使用提示說明的特效工具，"band": 0.5將使用頻帶的一半，調整x軸位置的偏移位置
              "y": {"scale": "yscale", "signal": "tooltip.amount", "offset": -2}, #配置amount使用提示說明的特效工具，offset:-2 調整y軸位置的偏移位置-2為向下兩個像素
              "text": {"signal": "tooltip.amount"},  #文字類別配置以amount使用提示說明的特效工具
              "fillOpacity": [     #圖形內部的不透明度值：[0-1]
                {"test": "datum === tooltip", "value": 0},  
                {"value": 1}
              ]    
            }
          }
        }
      ]
    }

3.資料來源:

