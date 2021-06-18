# **渲染管线**
# Overview
```mermaid
graph LR

    效果([PostProcessRenderEffect])
    --> 后处理([PostProcessRenderPipeline])
    --> 默认([DefaultRenderingPipeline])

```
&emsp;  

# Table of Content
&emsp;  

## &star; DefaultRenderingPipeline
### 术语
* anti-aliasing 抗锯齿效果
* depth of field 景深效果  
&emsp;  

## &star; PostProcessRenderPipeline
###
&emsp;  

## &star; PostProcessRenderEffect
### properties
* _postProcesses
> this._postProcesses = {}  
* _getPostProcess
> this._getPostProcesses = this.getPostProcesses
### methods
* +getPostProcesses(camera?: Camera): Nullable<Array<PostProcess>>
> 根据相机获取PostProcesses

* +_attachCameras(cameras: any): void
>  
```mermaid 
graph TD 
    A([loop cameras]) 
    --> B([this._getPostProcesses]) 
    --> C([this.postProcess])
```

* +_detachCameras(cameras: any): void