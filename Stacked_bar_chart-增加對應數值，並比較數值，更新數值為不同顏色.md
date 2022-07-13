# Stacked bar chart-增加對應數值，並比較數值，更新數值為不同顏色
目標:
Stacked bar chart-更改為:增加圖表對應的數值，並比較當數值>50此時，更新數值為不同顏色。 

原始圖表Stacked bar chart連結:

https://vega.github.io/editor/#/examples/vega/stacked-bar-chart

----------

{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "A basic stacked bar chart example.",
  "width": 500,
  "height": 200,
  "padding": 5,
  "data": [
    {
      "name": "table",
      "values": [
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
      "transform": [
        {
          "type": "stack",
          "groupby": ["x"],
          "sort": {"field": "c"},
          "field": "y"
        }
      ]
    }
  ],
  "scales": [
    {
      "name": "x",
      "type": "band",
      "range": "width",
      "domain": {"data": "table", "field": "x"}
    },
    {
      "name": "y",
      "type": "linear",
      "range": "height",
      "nice": true, "zero": true,
      "domain": {"data": "table", "field": "y1"}
    },
    {
      "name": "color",
      "type": "ordinal",
      "range": "category",
      "domain": {"data": "table", "field": "c"}
    }
  ],
  "axes": [
    {"orient": "bottom", "scale": "x", "zindex": 1},
    {"orient": "left", "scale": "y", "zindex": 1}
  ],
  "marks": [
    {
      "type": "rect",
      "from": {"data": "table"},
      "encode": {
        "enter": {
          "x": {"scale": "x", "field": "x"},
          "width": {"scale": "x", "band": 1, "offset": -1},
          "y": {"scale": "y", "field": "y0"},
          "y2": {"scale": "y", "field": "y1"},
          "fill": {"scale": "color", "field": "c"}
        },
        "update": {
          "fillOpacity": {"value": 1}
        },
        "hover": {
          "fillOpacity": {"value": 0.5}
        }
      }
    },{
      "type": "text",
      "from": {"data": "table"},
      "encode": {
        "enter":{          
          "baseline": {"value": "bottom"},
          "fontSize":{"value":20},
          "fill":{"value":"#000"}},
      "update":{    
          "x":{"scale": "x","field":"x","offset":10},
          "y":{"scale": "y","field":"y1","offset":0},
          "text":{"data": "table", "field":"y"},
           "fill":[
            {"test":"datum['y'] > 50 ","value":"#000"},{"value":"red"}]
          }
        }
      }
      
  ]
}

----------
| 添加設定-text(對應數值)&<br>fill(使用test比較來更新數值>50時顯示不同顏色)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 更新顯示如圖                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| ,{<br>      "type": "text",<br>      "from": {"data": "table"},<br>      "encode": {<br>        "enter":{          <br>          "baseline": {"value": "bottom"},<br>          "fontSize":{"value":20},<br>          "fill":{"value":"#000"}},<br>      "update":{    <br>          "x":{"scale": "x","field":"x","offset":10},<br>          "y":{"scale": "y","field":"y1","offset":0},<br>          "text":{"data": "table", "field":"y"},<br>           "fill":[<br>            {“test":"datum['y'] > 50 ","value":"#000"},{"value":"red"}]<br>          }<br>        }<br>      } | ![](https://paper-attachments.dropbox.com/s_8187FD07624D5FBA2338A86DB2BDA4188A41D05C8681B53005A85E64360CB7A8_1657676608509_image.png) |


