# Quadtree note
## 【 QuadtreeTile 】
### 描述
<p>
方法</br>
1) 创建L0级瓦片 createLevelZeroTiles </br>
2) 更新附加数据 updateCustomData </br>
3) 查找瓦片 </br>
findTileToWest、findTileToEast、findTileToSouth、findTileToNorth </br>
4) 释放资源 freeResources </br>
</p>


## 【 TilingScheme 】
### 描述
WebMercatorTilingScheme & GeographicTilingScheme </br>
1) WebMercatorTilingScheme </br>
地理弧度转墨卡托坐标</br>
方法 : </br>
a) getNumberOfXTilesAtLevel </br>
b) getNumberOfYTilesAtLevel </br>
c) rectangleToNativeRectangle </br>
d) tileXYToNativeRectangle </br>
e) tileXYToRectangle </br>
f) positionToTileXY </br>

2) WebMercatorTilingScheme </br>
地理弧度转地理坐标</br>
方法 : </br>
a) getNumberOfXTilesAtLevel </br>
b) getNumberOfYTilesAtLevel </br>
c) rectangleToNativeRectangle </br>
d) tileXYToNativeRectangle </br>
e) tileXYToRectangle </br>
f) positionToTileXY </br>
