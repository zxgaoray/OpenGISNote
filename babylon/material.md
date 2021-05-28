# material
## 简介
使用color或者材质覆盖mesh，体现对光照的反射现象，目前支持4种反射
* diffuse
    > 漫反射
* specular
    > 镜面反射
* emissive
    > 自发光
* ambient
    > 环境光

材质类型
* color 颜色
* texture 贴图
* wireframe 线框
* transparent color 透明色 alpha通道
* back-face culling 背面剔除

纹理映射
* bump map 凹凸贴图
* normal map 法线贴图
* inverting bumps and dents 反向凹凸、压痕
* parallax mapping 视差贴图
* parallax occlusion mapping 视差贴图，与parallax mapping是递进关系
* uv mapping uv贴图

材质映射到各个mesh的面
