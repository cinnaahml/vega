# Stacked Area Chart-鼠標放上時，顯示不同顏色
目標:
Stacked Area Chart-更改為:鼠標旋停於圖表時，區塊顯示不同色系。 

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
      "type": "point",
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
              "y": {"scale": "y", "field": "y0"},
              "y2": {"scale": "y", "field": "y1"},
              "fill": {"scale": "color", "field": "c"}
            },
            "update": {
              "fill": {"scale": "color","field": "c"}
            },
            "hover": {
             "fill": [
            {"test":"datum['c'] > 0 ","value":"pink"},{"value":"red"}]
            }
          }
        }
      ]
    }
  ]
}

----------

更新後呈現如下:
以定義的’c’色塊界定更新顏色

| "update": {<br>              "fill": {"scale": "color","field": "c"}<br>            },<br>            "hover": {<br>             "fill": [<br>            {"test":"datum['c'] > 0 ","value":"pink"},{"value":"red"}]<br>            } | ![](https://paper-attachments.dropbox.com/s_5DA751EF22C0877ACA230875A53863AFAE1906822ECA7B35567A3B872ECC1FDF_1657611426484_image.png) | ![](https://paper-attachments.dropbox.com/s_5DA751EF22C0877ACA230875A53863AFAE1906822ECA7B35567A3B872ECC1FDF_1657611457148_image.png) |



