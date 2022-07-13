# A basic bar chart-直接呈現數值於圖表上
目標:
A basic bar chart-更改為:直接呈現對應數值於圖表。 

原始圖表A basic bar chart連結:

https://vega.github.io/editor/#/examples/vega/bar-chart


{
 "$schema": "https://vega.github.io/schema/vega/v5.json",
 "description": "A basic bar chart example, with value labels shown upon mouse hover.",
 "width": 400,
 "height": 200,
 "padding": 5,
 
 "data": [
   {
     "name": "table",
     "values": [
       {"category": "A", "amount": 28},
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
 
 "signals": [
   {
     "name": "tooltip",
     "value": {},
     "on": [
       {"events": "rect:mouseover", "update": "datum"},
       {"events": "rect:mouseout",  "update": "{}"}
     ]
   }
 ],
 
 "scales": [
   {
     "name": "xscale",
     "type": "band",
     "domain": {"data": "table", "field": "category"},
     "range": "width",
     "padding": 0.05,
     "round": true
   },
   {
     "name": "yscale",
     "domain": {"data": "table", "field": "amount"},
     "nice": true,
     "range": "height"
   }
 ],
 
 "axes": [
   { "orient": "bottom", "scale": "xscale" },
   { "orient": "left", "scale": "yscale" }
 ],
 
 "marks": [
   {
     "name": "aaa",
     "type": "rect",
     "from": {"data":"table"},
     "encode": {
       "enter": {
         "x": {"scale": "xscale", "field": "category"},
         "width": {"scale": "xscale", "band": 1},
         "y": {"scale": "yscale", "field": "amount"},
         "y2": {"scale": "yscale", "value": 0}
       },
       "update": {
         "fill": {"value": "steelblue"}
       }
      
     }
   },
   {
     "type": “text",
     "from": {"data": "aaa"},
     "encode": {
       "enter": {
         "y": {"field":"y", "offset": -5},
         "x":{"field":"x", "offset": 5},
         "text": {"field":"datum.amount"},
         "fill": {"value": "red"},
         "fontSize":{"value":30 }
         
       }
      
       }
   }
 ]
}

----------

更新後呈現如下:
增設text設定以增加數值於圖表上。

| {<br>     "type": “text",<br>     "from": {"data": "aaa"},<br>     "encode": {<br>       "enter": {<br>         "y": {"field":"y", "offset": -5},<br>         "x":{"field":"x", "offset": 5},<br>         "text": {"field":"datum.amount"},<br>         "fill": {"value": "red"},<br>         "fontSize":{"value":30 }<br>       }<br>       }<br>   } | ![](https://paper-attachments.dropbox.com/s_122AD6B2F28FB8B9A468F978847A8D08A5D042771A6D986F0B7191A90B821674_1657677243398_image.png) |


