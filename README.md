# copc-format-wakayama-point-cloud-data

## Public Website
- Ctrlを押しながらドラッグで回転できる  
- 色はデフォルトでElevationになっているのでパネルからRGBに変更すると実際の色で表示できる  
https://viewer.copc.io/?copc=https://xs489works.xsrv.jp/copc-data/pref-wakayama/06QC511_org_translated.copc.laz

## COPC（Cloud Optimized Point Cloud）の生成方法
- [pdal 2.5.2](https://pdal.io/en/latest/)

```
# OSGeo4W Shellを起動
# パイプラインを用いてテキストからLASへの変換
pdal pipeline pipeline_txt2las.json

# メタデータの確認
pdal info --metadata 06QC511_org.las

# 座標参照系の付与
pdal translate -i 06QC511_org.las -o 06QC511_org_translated.las --writers.las.a_srs="EPSG:6674"

# メタデータの確認
pdal info --metadata 06QC511_org_translated.las

# COPCへの変換
pdal translate -i 06QC511_org_translated.las -o 06QC511_org_translated.copc.laz --writers.copc.forward=all
```
pipeline_txt2las.json
```json
{
  "pipeline":[
    {
      "type":"readers.text",
      "filename":"06QC511_org.txt",
      "separator":",",
      "header":"ID,X,Y,Z,B",
      "spatialreference":"EPSG:6674"
    },
    {
      "type":"filters.reprojection",
      "out_srs":"EPSG:6674"
    },
    {
      "type":"writers.las",
      "filename":"06QC511_org.las"
    }
  ]
}

```
## COPC ViewerでCOPCの表示
- https://viewer.copc.io
- 以下のように直接URLを指定してアクセス  
https://viewer.copc.io/?copc=https://www.example.com/xxx.copc.laz

## Data Source
https://wakayamaken.geocloud.jp/mp/22
