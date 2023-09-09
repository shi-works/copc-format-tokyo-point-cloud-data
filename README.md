# copc-format-tokyo-point-cloud-data

## Public Website
- Ctrlを押しながらドラッグで回転できる  
- 色はデフォルトでElevationになっているのでパネルからRGBに変更すると実際の色で表示できる
### 八王子駅周辺
https://viewer.copc.io/?copc=https://xs489works.xsrv.jp/copc-data/tokyo-digitaltwin/hachioji-station-translated.copc.laz

### 青ヶ島（DEM）
https://viewer.copc.io/?copc=https://xs489works.xsrv.jp/copc-data/tokyo-digitaltwin/aogashima-dem-translated.copc.laz

### 青ヶ島（DEM）
https://viewer.copc.io/?copc=https://xs489works.xsrv.jp/copc-data/tokyo-digitaltwin/takahata-fudoson-tumulus-dem-translated.copc.laz

## COPC（Cloud Optimized Point Cloud）の生成方法
- [pdal 2.5.2](https://pdal.io/en/latest/)

```
# OSGeo4W Shellを起動
# メタ情報の確認
pdal info --metadata 09LC2867.las

# パイプラインでlasをマージ
pdal pipeline merge-pipeline.json

# 座標参照系の付与
pdal translate -i hachioji-station-org.las -o hachioji-station-translated.las --writers.las.a_srs="EPSG:6677"

# メタ情報の確認
pdal info --metadata hachioji-station-translated.las

# COPCへの変換
pdal translate -i hachioji-station-translated.las -o hachioji-station-translated.copc.laz --writers.copc.forward=all
```
merge-pipeline.json
```json
{
  "pipeline": [
    {
      "type": "readers.las",
      "filename": "las/*.las"
    },
    {
      "type": "writers.las",
      "filename": "hachioji-station.las"
    }
  ]
}
```
## COPC ViewerでCOPCの表示
- https://viewer.copc.io
- 以下のように直接URLを指定してアクセス  
https://viewer.copc.io/?copc=https://www.example.com/xxx.copc.laz

## Data Source
### 東京都デジタルツイン　多摩地域点群データ
https://catalog.data.metro.tokyo.lg.jp/dataset/t000029d0000000020
