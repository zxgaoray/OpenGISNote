# **Renderer**


# Overview
## BoundingBoxRenderer
```mermaid
classDiagram

ISceneComponent <|-- BoundingBoxRenderer

class ISceneComponent {
    <<interface>>
    +string name
    +Scene scene
    +register()
    +rebuild()
    +dispose()
}

class BoundingBoxRenderer {
    +Color3 frontColor
    +Color3 backColor
    +Boolean showBacklines
    +Observable<BoundingBox> onBeforeBoxRenderingObservable
    +Observable<BoundingBox> onAfterBoxRenderingObservable
    +Observable<BoundingBox> onResourcesBoxRenderingObservable
    +SmartArray<BoundingBox> renderList
    -ShaderMaterial _colorShader
    -Map<String, Nullable<VertexBuffer>> _vertexBuffer
    -DataBuffer _indexBuffer
    -Nullable<DataBuffer> _fillIndexBuffer
    -Nullable<IndicesArray> _fillIndexData

    +constructor(Scene scene)
    +register()
    -_evaluateSubMesh(AbstractMesh mesh, SubMesh subMesh)
    -_preActiveMesh(AbstractMesh mesh)
    -_prepareResources()
    -_createIndexBuffer()
    +rebuild()
    +reset()
    +render(number renderingGroupId)
    +renderOcclusionBoundingBox(AbstractMesh mesh)
    +dispose()

}
```

# Table of Contents
## BoundingBoxRenderer
### methods
#### \+ constructor(scene: Scene)
> scene._addComponent(this)

#### \+register()
注册  
before active  
> this.scene._beforeEvaluateActiveMeshStage.registerStep()  
  
pre active
> this.scene._preActiveMeshStage.registerStep()  
  
sub mesh  
> this.scene._evaluateSubMeshStage.registerStep()  
  
after rendering group  
> this.scene._afterRenderingGroupDrawStage()  

#### \- _evaluateSubMesh(mesh: AbstractMesh, subMesh: SubMesh)
评估sub mesh  
```mermaid
graph TD

begin([start])
--> A{mesh.showSubMeshesBoundingBox}
--Yes---> B(mesh.getBoundingInfo)
--> C(boundingInfo.boundingBox._tag=mesh.renderingGroupId)
--> D(this.renerList.push--boundingInfo.boundingBox)
-->over([end])

```

#### \- _preActiveMesh(mesh: AbstractMesh)
```mermaid
graph TD
begin([start])
--> A{mesh.showBoundingBox or scene.forceShowBoundingBoxes}
--Yes---> B(mesh.getBoundingBoxInfo)
--> C(boundingBoxInfo.boundingBox._tag=mesh.renderingGroupId)
--> D(this.renderingList.push--boundingInfo.boundingBox)
--> over([end])

```

#### \- _prepareResources()
预备_colorShader  
1.已有color shader直接返回void  
2.没有则 new ShaderMaterial  
3.设置_vertexBuffers  
4.createIndexBuffer  
5.设置_fillIndexData  
6.onResourcesReadyObserverable.notifyObservers

#### \- _createIndexBuffer()
使用this.scene.getEngine()创建index buffer

#### \+ rebuild()
1.rebuild vertexbuffer [PositionKind]  
2._createIndexBuffer()  

#### \+ reset()
置空renderList  
renderList.reset()

#### \+ render(renderingGroupId: number)
```mermaid
graph TD
begin([start])
--> A{renderList是否为空}
--Yes--->over([end]);
A--No--->B(this._prepareResources)
--> C{this._colorShader.isReady}
--No--->over;
C--Yes--->this._colorShader._preBind
--> D{loop}
--Yes--->E(根据boundingBoxIndex从renderList获取boundingBox)
--> F
--> over
```
