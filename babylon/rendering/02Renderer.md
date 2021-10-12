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

## DepthRenderer & DepthRendererSceneComponent
```mermaid
classDiagram

class DepthRenderer {
    -Scene _scene
    -RenderTargetTexture _deothMap
    -Effect _effect
    -boolean _storeNonLinearDepth
    -Color4 _clearColor
    +boolean isPacked
    -string _cachedDefine
    -Nullable<Camera> _camera
    +boolean enable
    +boolean useOnlyInActiveCamera

    +constructor(...)
    +isReady(SubMesh subMesh, boolean useInstances) boolean
    +getDepthMap() RenderTargetTexture
    +dispose()
}

ISceneComponent <|-- DepthRendererSceneComponent

class DepthRendererSceneComponent {
    +string name
    +Scene scene

    +constructor(Scene scene)
    +register()
    +dispose()
    -_gatherRenderTargets(...)
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
根据指定的render中的tag，循环renderList里面的tag与renderingGroupId相同的boundingBox，进行draw操作  
draw包含front和back

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

--> F{boundingBox._tag!==renderingGroupId}
--No---> 发送BeforeBoxRendering消息
--> 计算worldMatrix
--> engine.bindBuffers

--> H{showBackLines}
--Yes---> T1(engine处理Depth)
--> this._colorShader设置back颜色并绑定worldMatrix
--> engine.drawElementTypeLine:ListDrawMode
--> T2;
H --No---> T2(engine处理Depth)

--> this._colorShader设置front颜色并绑定worldMatrix
--> engine.drawElementType:LineListDrawMode
--> 发送AfterBoxRendering消息

--> I{endLoop}
--No---> D;
F--Yes---> D; 
I--Yes---> this._colorShader.unbind
--> T3(engine处理Depth)
--> over
```

#### \+ renderOcclusionBoundingBox(mesh: AbstractMesh): void
1、准备resources  
2、判定this._colorShader和boundingInfo是否ready  
3、判定this._fillIndexBuffer是否存在，不存在则使用engine创建  
4、计算worldMatrix
5、engine.bindBuffers: vertexBuffer、fillIndexBuffer、colorShader
6、设置depthFunction, colorShader设置worldMatrix
7、engine.drawElementType
8、复原colorShader、DepthFunction

#### \+ dispose
1、observable.clear
2、renderList.dispose
3、colorShader.dispose
4、vertexBuffer.dispose
5、engine.releaseBuffer

## DepthRenderer