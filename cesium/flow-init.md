# flow to init Viewer

## 1 App
### <font color="darkorange">constructor</font>
### &#9758; instances
* viewer = new Viewer()

## 1.1 Viewer
### <font color="darkorange">constructor</font>
### &#9758; parameters
* container
  
    > dom element
* options
    > parameters
    * imageryProvider
    * terrianProvider
### &#9758; instances
* cesiumWidget = new CesiumWidget()     --- [1.1.1](#1.1.1 CesiumWidget)

## 1.1.1 CesiumWidget
### <font color="darkorange">constructor</font>
### &#9758; parameters
* container
  
    > dom element
* options
    > parameters
    * imageryProvider
    * terrianProvider
### &#9758; instances
* ellipsoid = Ellipsoid.WGS84
* scene = new Scene()       --- [1.1.1.2](#1.1.1.2 Scene)
* globe = new Globe()       --- [1.1.1.3](#1.1.1.3 Globe)
* skyBox = new SkyBox()

### &#9758; assignment
scene.globe = globe

## 1.1.1.2 Scene
### <font color="darkorange">constructor</font>
### &#9758; parameters
* options
    * canvas
    * mapProjection
    * mapMode2D

### &#9758; instances

## 1.1.1.3 Globe
### <font color="darkorange">constructor</font>
### &#9758; parameters

### &#9758; instances
* _terrainProvider = new EllipsoidTerrainProvider()     --- [1.1.1.3.1](#1.1.1.3.1 EllipsoidTerrainProvider)
* _imageryLayerCollection = new ImageryLayerCollection()        --- [1.1.1.3.2](#1.1.1.3.2 ImageryLayerCollection)
* _surface = new QuadtreePrimitive      --- [1.1.1.3.3](#1.1.1.3.3 QuadtreePrimitive)
  
    > options.tileProvider = new GlobeSurfaceTileProvider()     --- [1.1.1.3.4](#1.1.1.3.4 GlobeSurfaceTileProvider)

## TerrainProvider
### <font color="darkorange">constructor</font>
#### &#9758; properties
* TerrainProvider.heightmapTerrainQuality = 0.25  

### <font color="darkorange">methods</font>
#### + TerrainProvider.getEstimatedLevelZeroGeometricErrorForAHeightmap
* parameters
    > ellipsoid  
    tileImageWidth  
    numberOfTilesAtLevelZero  
* process
    
    > (ellipsoid.maximumRadius * 2 * Math.PI * TerrainProvider.heightmapTerrainQuality) / (tileImageWidth * numberOfTilesAtLevelZero)

## 1.1.1.3.1 EllipsoidTerrainProvider
### <font color="darkorange">constructor</font>
#### &#9758; properties
* _tilingScheme = options.tilingScheme
    
    > new GeographicTilingScheme()

### <font color="darkorange">methods</font>
#### + EllipsoidTerrainProvider.prototype.getLevelMaximumGeometricError
* parameters
    
    > level
* process
    
    > this._levelZeroMaximumGeometricError / (1 << level)

## 1.1.1.3.2 ImageryLayerCollection
### <font color="darkorange">constructor</font>
#### &#9758; parameters

### <font color="darkorange">methods</font>
* +

## 1.1.1.3.3 QuadtreePrimitive
### <font color="darkorange">constructor</font>
#### &#9758; parameters
* options
    * tileProvider
    > options.tileProvider  --- [1.1.1.3](#1.1.1.3 Globe)

#### &#9758; properties
* _tileProvider = options.tileProvider
* tilingScheme = options.tileProvider.tillingScheme

#### &#9758; instances

### <font color="darkorange">methods</font>
### + QuadtreePrimitive.prototype.endFrame(frameState)
* processTileLoadQueue(this, frameState)    --- [processTileLoadQueue](#- QuadtreePrimitive function processTileLoadQueue(primitive, frameState))

### - QuadtreePrimitive function processTileLoadQueue(primitive, frameState)
* var tileProvider = primitive._tileProvider        --- [GlobeSurfaceTileProvider](#1.1.1.3.4 GlobeSurfaceTileProvider)

* processSinglePriorityLoadQueue()      --- [processSinglePriorityLoadQueue](#QuadtreePrimitive function processSinglePriorityLoadQueue())
    
    > primitive,frameState,tileProvider,endTime,tileLoadQueueLow,didSomeLoading

### - QuadtreePrimitive function processSinglePriorityLoadQueue()
> primitive,frameState,tileProvider,endTime,loadQueue,didSomeLoading
* tileProvider.loadTile(frameState, tile)   --- [loadTile](#+ GlobeSurfaceTileProvider.prototype.loadTile(frameState, tile))

### - QuadtreePrimitive function selectTilesForRendering

### - QuadtreePrimitive function visitIfVisible

### - QuadtreePrimitive function visitTile


## 1.1.1.3.4 GlobeSurfaceTileProvider
### <font color="darkorange">constructor</font>
### &#9758; parameters
* options
    * terrainProvider
    > globe._terrainProvider  --- [1.1.1.3](#1.1.1.3 Globe)
    * imageryLayers
    > globe._imageryLayerCollection  --- [1.1.1.3](#1.1.1.3 Globe)
    * surfaceShaderSet

### &#9758; instances

### &#9758; properties
* _imageryLayers = options.imageryLayers;

### <font color="darkorange">methods</font>
### + GlobeSurfaceTileProvider.prototype.initialize
* this._imageryLayers.queueReprojectionCommands(frameState)

### + GlobeSurfaceTileProvider.prototype.loadTile(frameState, tile)
* GlobeSurfaceTile.processStateMachine()
    > tile,
    frameState,
    this.terrainProvider,
    this._imageryLayers,
    this._vertexArraysToDestroy,
    terrainOnly



## GlobeSurfaceTile

### <font color="darkorange">methods</font>
### + GlobeSurfaceTile.processStateMachine

### + GlobeSurfaceTile.initialize
* parameters
    > tile,
        terrainProvider,
        imageryLayerCollection

* process  
(1) tile.data = new GlobeSurfaceTile()      --- [GlobeSurfaceTile](#GlobeSurfaceTile)
(2) prepareNewTile(tile, terrainProvider, imageryLayerCollection)       --- [prepareNewTile](#- GlobeSurfaceTile function prepareNewTile())

### - GlobeSurfaceTile function prepareNewTile()
* layer._createTileImagerySkeletons(tile, terrainProvider)

## ImageryLayer
### <font color="darkorange">methods</font>
### + ImageryLayer.prototype._createTileImagerySkeletons

## QuadtreeTileProvider


## QuadtreeTile
### <font color="darkorange">methods</font>
### + QuadtreeTile.createLevelZeroTiles
* process  
(1) new QuadtreeTile()

## TilingScheme
### <font color="darkorange">subclasses</font>
* WebMercatorTilingScheme

## WebMercatorTilingScheme
### <font color="darkorange">constructor</font>
### &#9758; parameters
* options
    > numberOfLevelZeroTilesX=1  
    numberOfLevelZeroTilesY=1

### <font color="darkorange">methods</font>
### + WebMercatorTilingScheme.prototype.getNumberOfXTilesAtLevel(level)
* process
    
    > this._numberOfLevelZeroTilesX << level

### + WebMercatorTilingScheme.prototype.getNumberOfYTilesAtLevel(level)
* process
    
    > this._numberOfLevelZeroTilesY << level

## GeographicTilingScheme

