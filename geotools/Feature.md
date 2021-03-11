# Feature note

## SimpleFeatureTypeBuilder
### buildFeatureType()
<p>
    创建要素类型
</p>
<p style="color:gray">
必须先setCRS(), 再setDefaultGeometry(), 再add GeometryDescriptor, 否则crs无法给到geometry。
<p>