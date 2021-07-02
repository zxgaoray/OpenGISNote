# **Light**
# Overview
## class relationship

```mermaid
classDiagram
class Light {
    <<abstract>>
    +Color3 diffuse
    +Color3 specular
    +int falloffType
    +number intensity
}
```