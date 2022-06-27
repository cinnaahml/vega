# vega_Grouped Bar Chart

1.grouped bar chart
2.資料格式:
{   #以**$schema**定義架構來源
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "A basic grouped bar chart example.",
  "width": 300,  #設定圖表x軸寬度
  "height": 240,   #設定圖表y軸高度
  "padding": 5,  #設定視圖邊框之間的邊距，透過 padding 的來達到水平及垂直置中的效果#
  "data": [
    {
      "name": "table",  #設定資料來源 "數據集"[{"name":"圖表名稱","values":[{},{}]}]#
      "values": [         **#圖表值 "values"(可使用**values/url/source**):[{"x軸欄位名稱":"x軸物件項目標籤",**position定位位置**,"y軸欄位名稱":"x軸類別項目標籤的****值****"}]#**
        {"category":"A", “position":0, "value":0.1},
        {"category":"A", "position":1, "value":0.6},
        {"category":"A", "position":2, "value":0.9},
        {"category":"A", "position":3, "value":0.4},
        {"category":"B", "position":0, "value":0.7},
        {"category":"B", "position":1, "value":0.2},
        {"category":"B", "position":2, "value":1.1},
        {"category":"B", "position":3, "value":0.8},
        {"category":"C", "position":0, "value":0.6},
        {"category":"C", "position":1, "value":0.1},
        {"category":"C", "position":2, "value":0.2},
        {"category":"C", "position":3, "value":0.7}
      ]
    }
  ],
  "scales": [   #定義座標表示**比例尺，設定垂直及水平軸，**轉換數據為圖表(縮放函數將數據值映射到視覺值)
    {                                  #yscale使用band 將 domain 
      "name": “yscale",     #設定y軸比例尺物件名稱，yscale
      "type": "band",         #定義類型:band
      “domain": {"data": "table", "field": "category"},
      "range": "height",  #範圍:圖表高度(width/height)
      "padding": 0.2   #y座標軸標籤之間的間隔(長條圖間距)，**padding**<0
    },
    {
      "name": "xscale",      #設定x軸比例尺物件名稱，xscale
      "type": “linear",        #定義類型:線性
      "domain": {"data": "table", "field": “value"},   #"定量範圍":{"資料來源":"圖表名稱","**field**":"value"}
      "range": "width", #範圍:圖表寬度(width/height)
      "round": true,   #以確保捕捉到像素邊界
      "zero": true,  #不要禁用此功能
      "nice": true  
    },
    {
      "name": "color",   #設定物件名稱:color
      "type": "ordinal",    #類型序數比例尺 (可以使用非數字的值)
      "domain": {"data": "table", "field": "position"},    #"定量範圍":{"資料來源":"圖表名稱","**field**":"定位位置"}   
      "range": {“scheme": "category20"}   ##範圍:schemeg作為比例範圍的配色方案：category20顏色編號
    }
  ],
  "axes": [       #**orient**定義座標軸標籤的上下左右位置，設定**orient:**top、right、bottom 或 left
    {"orient": "left", "scale": "yscale", “tickSize": 0, “labelPadding": 4, “zindex": 1},#**orient:左邊,scale:y**軸比例,tickSize座標軸上刻度線條的尺寸(數字負值往座標內縮，正值往座標外),labelPadding座標標籤和座標軸之間的間距,zindex堆叠層級顯示
    {"orient": "bottom", "scale": "xscale"}    ,#**orient:底部,scale:x**軸比例
  ],
  "marks": [  #定義標記(位置、大小、形狀和顏色)使用矩形、線條和符號等圖形標記對數據進行可視化編碼。
    {
      "type": "group", #類型選項：分組子圖形
      "from": {    ＃指定所使用的數據集
        “facet": {   ＃細分數據，用於跨多個組標記對數據進行分區
          "data": "table",   #資料來源":"圖表"
          "name": "facet", 
          "groupby": "category"   ＃對數據進行分區：category
        }
      },
      "encode": {  ＃編碼規則
        "enter": {  ＃輸入
          "y": {"scale": "yscale", "field": "category"}  ＃y軸配置項目：scale轉換為數據,使用**scale**轉換yscale值以此領域應用:category
        }
      },
      "signals": [  ＃更新動態變量,定義標記屬性或數據轉換參數
        {"name": "height", "update": "bandwidth('yscale')"}   ＃以height,更新指定帶寬轉換為yscale寬度
      ],
      "scales": [  ＃數字轉換為視覺屬性
        {
          "name": "pos",
          "type": "band",  
          "range": “height",   #轉換比例範圍：height
          "domain": {"data": "facet", "field": "position"}  ＃數據引用：使用資料來源轉換facet,**以field**應用在定位}
        }
      ],
      "marks": [＃使用圖形標記對數據進行可視化編碼。
        {
          "name": "bars",
          "from": {"data": "facet"},   ＃指定所使用的數據集
          "type": "rect",  
          "encode": {
            "enter": {
              "y": {"scale": "pos", "field": "position"},
              "height": {"scale": "pos", "band": 1},
              "x": {"scale": "xscale", "field": "value"},
              "x2": {"scale": "xscale", "value": 0},
              "fill": {"scale": "color", "field": "position"}
            }
          }
        },
        {
          "type": "text",  ＃類型：文字
          "from": {"data": "bars"},  ＃指定所使用的數據集為資料源
          "encode": {
            "enter": {
              "x": {"field": "x2", "offset": -5},    x軸配置中,區段轉次要x軸，文字於x座標沿圖表的偏移位置 (正值往右偏移/負值往左偏)  -5個像數
              "y": {"field": "y", "offset": {"field": "height", "mult": 0.5}},  ＃y軸配置：y軸的區段,文字於圖表偏移位置以區段高的值乘以0.5  
              "fill": [  ＃內部的填滿顏色配置
                {"test": “contrast('white', datum.fill) > contrast('black', datum.fill)", "value": "white"},  
                {"value": "black"}
              ],
              "align": {"value": "right"},  ＃水平文本對齊：右   （left (default), center, right）
              "baseline": {"value": "middle"},  ＃文字基線：中間
              "text": {"field": "datum.value"} ＃文本：
            }
          }
        }
      ]
    }
  ]
}


