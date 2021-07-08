# **Mesh** 
# Overview


# Table of Content
## Mesh

## VertexData
### &#9758; *class*
```mermaid
classDiagram

IGetSetVerticesData <|-- VertexData
IGetSetVerticesData <|-- Geometry

class IGetSetVerticesData {
    <<interface>>
    +isVerticesDataPresent(string kind) boolean
    +getVerticesData(string kind, boolean copyWhenShared?, boolean forceCopy?) Nullable~FloatArray~
    +setVerticesData(string kind, FloatArray data, boolean updatable)
    +getIndices(boolean copyWhenShared?, boolean forceCopy?) Nullable~IndicesArray~
    +setIndices(IndicesArray indices, Nullable<number> totalVertices, boolean updatable?)

}

class VertexData {
    +int FRONTSIDE$
    +int BACKSIDE$
    +int DOUBLESIDE$
    +int DEFAULTSIDE$
    +Nullable<FloatArray> positions
    +Nullable<FloatArray> normals
    +Nullable<FloatArray> tangents
    +Nullable<FloatArray> uvs
    +Nullable<FloatArray> colors
    +Nullable<FloatArray> indices
    +Nullable<FloatArray> metricesIndices
    +Nullable<FloatArray> metricesWeights
    +set(FloatArray data, string kind)
    +applyToMesh(Mesh mesh, boolean updatable?): VertexData
    +applyToGeometry(Geometry geometry, boolean updatable?) VertexData
    +updateMesh(Mesh mesh): VertexData
    +updateGeometry(Geometry geometry): VertexData
    -_applyTo(IGetSetVerticesData meshOrGeometry, boolean updatable) VertexData
    -_update(IGetSetVerticesData meshOrGeometry, boolean updateExtends?, boolean makeItUnique?) VertexData
    +transform(Matrix matrix) VertexData
    +merge(VertexData other, boolean use32BitsIndices) VertexData
    -_mergeElement(Nullable<FloatArray> source, Nullable<FloatArray> other) Nullable<FloatArray>
    -_validate()
    +ExtractFromMesh(...)$ VertexData
    +ExtractFromGeometry(...)$ VertexData
    -_ExtractFrom(IGetSetVerticesData meshOrGeometry, boolean copyWhenShared?, boolean forceCopy?) VertexData
    -ComputeNormals(...)$
    -_ComputeSides(...)$
}

class Geometry {
    +string id
    +number uniqueId
    +int delayLoadState
    +Nullable<string> delayLoadingFile
    +Handle onGeometryUpdated
    
}

```

### &#9758; *kind*
&starf; positions  
包含顶点位置信息的数组
[..., x, y, z, ...]

&starf; normals  
包含顶点法线信息的数组
[..., x, y, z, ...]

&starf; tangents  
包含顶点切向信息的数组
[..., x, y, z, ...]

&starf; uvs  
使用uv将纹理图片映射到顶点上的数组
UV ~ UV6

&starf; colors  
顶点颜色的数组

&starf; matricesIndecies  
由骨骼生成的矩阵的索引列表  
每个顶点有4个索引 (如果设置了extra则是8个)

&starf; matricesWeights  
最终估算过程中的每个索引matrix的权重值列表

&starf; matricesIndicesExtra  
扩展矩阵索引

&starf; matricesWeightsExtra  
扩展矩阵权重

&starf; indices  
构成三角形的三个顶点的索引
[..., i, j, k, ...]

### &#9758; *methods*
* \+ set(data: FloatArray, king: string): void  
    > 为VertexData按类型设置数据
