# Stacked Area Chart-更新數值為不疊加
目標:
Stacked Area Chart-更改為:原始圖為數值疊加圖，更新為數值不疊加。 

原始圖表Stacked Area Chart連結:

https://vega.github.io/editor/#/examples/vega/stacked-area-chart

----------

{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "A basic stacked area chart example.",
  "width": 500,
  "height": 200,
  "padding": 5,
  "data": [
    {
      "name": "table",
      "values": [
        {"x": 0, "y": 28, "c":0}, {"x": 0, "y": 55, "c":1},
        {"x": 1, "y": 43, "c":0}, {"x": 1, "y": 91, "c":1},
        {"x": 2, "y": 81, "c":0}, {"x": 2, "y": 53, "c":1},
        {"x": 3, "y": 19, "c":0}, {"x": 3, "y": 87, "c":1},
        {"x": 4, "y": 52, "c":0}, {"x": 4, "y": 48, "c":1},
        {"x": 5, "y": 24, "c":0}, {"x": 5, "y": 49, "c":1},
        {"x": 6, "y": 87, "c":0}, {"x": 6, "y": 66, "c":1},
        {"x": 7, "y": 17, "c":0}, {"x": 7, "y": 27, "c":1},
        {"x": 8, "y": 68, "c":0}, {"x": 8, "y": 16, "c":1},
        {"x": 9, "y": 49, "c":0}, {"x": 9, "y": 15, "c":1}
      ]  
    }
  ],
  "scales": [
    {
      "name": "x",
      "type": "point",
      "range": "width",
      "domain": {"data": "table", "field": "x"}
    },
    {
      "name": "y",
      "type": "linear",
      "range": "height",
      "nice": true, "zero": true,
      "domain": {"data": "table", "field": "y"}
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
      "type": "group",
      "from": {
        "facet": {
          "name": "series",
          "data": "table",
          "groupby": "c"
        }
      },
      "marks": [
        {
          "type": "area",
          "from": {"data": "series"},
          "encode": {
            "enter": {
              "interpolate": {"value": "monotone"},
              "x": {"scale": "x", "field": "x"},
              "y": {"scale": "y", "field": "y"},
              
              "stroke": {"scale": "color", "field": "c"}
            },
            "update": {
              "fillOpacity": {"value": 1}
            },
            "hover": {
              "fillOpacity": {"value": 0.5}
            }
          }
        }
      ]
    }
  ]
}

----------

更新後呈現如下:
刪除疊加設定transform，以及疊加設定的y0/y1/y2。

| “transform": [<br>        {<br>          "type": "stack",<br>          "groupby": ["x"],<br>          "sort": {"field": "c"},<br>          "field": "y"<br>        }<br>      ]                                                                                                | *transform設定整行刪掉<br>*刪掉<br>*修改地方                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "marks": [<br>        {<br>          ....<br>              "x": {"scale": "x", "field": "x"},            <br>              "y": {"scale": "y", "field": "y0"},<br>              "y2": {"scale": "y", "field": "y1"},<br>              "fill": {"scale": "color", "field": "c"} | "marks": [<br>        {<br>          ....<br>              "x": {"scale": "x", "field": "x"},            <br>              "y": {"scale": "y", "field": "y"},<br>           "stroke": {"scale": "color", "field": "c"} |
| "scales": [<br>    {<br>      "name": "y",<br>      "type": "linear",<br>      "range": "height",<br>      "nice": true, "zero": true,<br>      "domain": {"data": "table", "field": "y1"}<br>    }    <br>改成:<br>"domain": {"data": "table", "field": "y"}                    | ![](https://paper-attachments.dropbox.com/s_5DA751EF22C0877ACA230875A53863AFAE1906822ECA7B35567A3B872ECC1FDF_1657615285961_image.png)                                                                                  |


