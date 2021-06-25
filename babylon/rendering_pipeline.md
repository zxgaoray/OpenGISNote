# **渲染管线**
# Overview
## class extend relationship
```mermaid
graph TD


    后处理([PostProcessRenderPipeline])
    --> 默认([DefaultRenderingPipeline])

```
&emsp;  

# Table of Content
&emsp;  

## &star; mind
```plantuml
@startmindmap
* DefaultRenderringPipeline
** PostProcessRenderPipeline
*** renderEffects
@endminmap
```

## &star; DefaultRenderingPipeline
默认渲染管线
### 术语
* anti-aliasing 抗锯齿效果
* depth of field 景深效果  
&emsp;  

## &star; PostProcessRenderPipeline
后处理渲染管线
### properties
* -_rederEffects
    > { [key: string]: [PostProcessRenderEffect](&star; PostProcessRenderEffect) }
    &emsp;  

* -_renderEffectsForIsolatedPass
    > Array of [PostProcessRenderEffect](&star; PostProcessRenderEffect)

### methods

* -constructor
    > 构造函数  
    > this._cameras = []

* +addEffect(renderEffect: PostProcessRenderEffect): void
    > 添加effect
  
* +_enableEffect(renderEffectName:string, camera:any): void  
    > 启用effect

* +_attachCameras(cameras: any, unique: boolean): void
    > 附加到camera 
    > 1. renderEffectName in this._renderEffects 
    > 2. this._renderEffects[renderEffectName]._attachCameras(cameras)   // postProcessRenderEffect._attachCameras(cameras: any): void [see](&star; PostProcessRenderEffect)

* +_detachCameras(cameras: any): void
    > 

* +_update(): void

* +_reset(): void
    > 重置

* -_enableMSAAOnFirstPostProcess(sampleCount: number): boolean

## &star; PostProcessRenderEffect
后处理效果
### properties
* _postProcesses
  
    > this._postProcesses = {}  
* _getPostProcess
  
    > this._getPostProcesses = this.getPostProcesses
### methods
* +getPostProcesses(camera?: Camera): Nullable<Array<PostProcess>>
  
    > 根据相机获取PostProcesses
    
* +_attachCameras(cameras: any): void
  
    > postProcess附加到相机
```mermaid
graph TD 
    start([start])
    --> stepLoopCameras[loop cameras];
    stepLoopCameras--> C1{ this._postProcesses是否包含cameraKey };
    C1 --否--> C1Sub1[this._getPostProcesses]
    --> C1Sub1-1[this._postProcesses.cameraKey = postProcesses]-->C2;
    C1 --是--> C2{ this._indicesForCamera是否包含cameraName };
    C2 --否--> C2Sub1[ this._indicesForCamera.cameraName = Array.new ]
    --> stepLoopProcesses;
    C2 --是--> stepLoopProcesses[loop postProcesses]
    --> stepSetIndex1[ index=camera.attachPostProcesses:postProcess ]
    --> stepSetIndex2[ this._indicesForCamera.cameraName.push:index ]
    --> over([over]);
```

* +_detachCameras(cameras: any): void
  
    > postProcess与相机分离
```mermaid
graph TD
    start([start])
    --> loopCameras[loop cameras]
    --> getPostProcesses[get postProcesses by camera name]
    --> loopPostProcesses[loop postProcesses]
    --> cameraDetachProcess[camera.detach:postProcess]
    --> over([over])
```

* +_enable(cameras: any): void