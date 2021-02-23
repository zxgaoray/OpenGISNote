# Utilities function note
## DataUtilities工具函数
### DataUtilities.reType(SimpleFeatureType featureType, SimpleFeature feature)
<p>要素转类型</p>
<p style="color:gray">
feature的要素类型与featureType出现同名属性name时，feature的name字段binding的数据类型与featureType的name字段binding的数据类型不一致时，featureType的name字段仍然会被赋值成feature的name字段值，造成reType后的新feature的name字段的属性值与binding的class不一致状况。
</p>