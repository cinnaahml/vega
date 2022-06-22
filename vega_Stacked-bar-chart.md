# 😃vega_Stacked-bar-chart
1.stacked-bar-chart
2.資料格式:
{      # #以**$schema**定義架構來源
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "A basic stacked bar chart example.",
  "width": 500,     #設定圖表x軸寬度#
  "height": 200,    #設定圖表y軸高度#
  "padding": 5,       #設定視圖邊框之間的邊距，透過 padding 的來達到水平及垂直置中的效果#
  "data": [             #設定資料來源 "數據集"[{"name":"圖表名稱","values":[{},{}]}]#
    {
      "name": "table",  **#圖表名稱(可自定義)#**
      "values": [        **#圖表值 "values”(可使用**`values/url/source`**):[]   #"x軸欄位名稱":"x軸物件項目標籤","y軸欄位名稱":"y軸物件項目標籤”,”配置**區塊設定顏色**”: 0原始顏色/1橘色**
        {"x": 0, "y": 28, "c": 0}, {"x": 0, "y": 55, "c": 1},    
        {"x": 1, "y": 43, "c": 0}, {"x": 1, "y": 91, "c": 1},
        {"x": 2, "y": 81, "c": 0}, {"x": 2, "y": 53, "c": 1},
        {"x": 3, "y": 19, "c": 0}, {"x": 3, "y": 87, "c": 1},
        {"x": 4, "y": 52, "c": 0}, {"x": 4, "y": 48, "c": 1},
        {"x": 5, "y": 24, "c": 0}, {"x": 5, "y": 49, "c": 1},
        {"x": 6, "y": 87, "c": 0}, {"x": 6, "y": 66, "c": 1},
        {"x": 7, "y": 17, "c": 0}, {"x": 7, "y": 27, "c": 1},
        {"x": 8, "y": 68, "c": 0}, {"x": 8, "y": 16, "c": 1},
        {"x": 9, "y": 49, "c": 0}, {"x": 9, "y": 15, "c": 1}
      ],
      “transform": [     #數據轉換
        {
          "type": "stack",   #定義數據類型:堆疊
          "groupby": ["x"],  #組合:[**x軸欄位物件名稱]**
          "sort": {"field": "c"}, #排序:{數據對應鍵:c顏色}
          "field": "y"    #數據對應鍵名:y數據值
        }
      ]
    }
  ],
  "scales": [   #定義座標表示**比例尺，設定垂直及水平軸，**轉換數據為圖表(縮放函數將數據值映射到視覺值)
    {
      "name": "x",    #設定x軸比例尺物件名稱:x
      "type": “band",   #類型
      "range": "width",  #範圍:圖表寬度(width/height)
      "domain": {"data": "table", "field": "x"}  #"資料源":{"資料來源":"圖表名稱","**field**":”**x軸欄位物件名稱**"}
    },
    {
      "name": "y",     #設定x軸比例尺物件名稱:y
      "type": "linear",   #類型線性
      "range": "height",  #範圍:圖表寬度(width/height)
      "nice": true, “zero": true,      #標籤邊距設置"nice": true, “zero": true禁用此功能
      "domain": {"data": "table", "field": "y1"}  #”定量範圍":{"資料來源":"圖表名稱","**field**":"y**軸項目標籤的****值**"}
    },
    {
      "name": "color",   #設定物件名稱:color
      "type": “ordinal",   #類型序數比例尺 (可以使用非數字的值)
      “range": “category", #範圍:category
      "domain": {"data": "table", "field": "c"}  #"定量範圍":{"資料來源":"圖表名稱","**field**":"c顏色"}
    }
  ],
  "axes": [    #定義座標軸標籤的上下左右位置
    {"orient": "bottom", "scale": "x", "zindex": 1},   #**orient:底部,scale:**x軸比例,堆叠層級顯示
    {"orient": "left", "scale": "y", "zindex": 1} #**orient:左邊,scale:y**軸比例,堆叠層級顯示
  ],
  "marks": [   #定義標記(位置、大小、形狀和顏色)
    {
      "type": "rect",  #類型選項：矩形
      "from": {"data": "table"},   #從:{資料源:圖表}
      "encode": {
        "enter": {    #enter是資料的方式
          "x": {“scale": "x", “field": "x"},  #x軸配置項目中(水平位置),使用**scale應用於x**以獲得x
          "width": {"scale": "x", "band": 1, “offset": -1},    #寬度位置:使用**scale應用於x**,`"band": 1`檢索刻度帶的完整大小(預設1),offset**矩形**座標的偏移位置(預設-1)
          "y": {"scale": "y", "field": "y0"},     #y軸配置項目中(設置垂直軸，定義矩形頂部:),使用**scale應用於y**以獲得y0
          "y2": {"scale": "y", "field": "y1"},   #y2終點軸配置項目中(設置y2垂直軸高度，定義矩形底部),使用**scale應用於y**以獲得y1
          "fill": {"scale": "color", "field": "c"} #圖形內部的填滿顏色配置項目,使用**scale應用於color**以獲得c
        },
        "update": {  #更改時更新設置，更新顏色(返回到其原始顏色)
          "fillOpacity": {"value": 1}  #圖形內部的不透明度值：[0-1]
        },
        "hover": { #鼠標懸停時矩形更改填充顏色
          "fillOpacity": {"value": 0.5}  #圖形內部的不透明度值：[0-1]
        }
      }
    }
  ]
}

