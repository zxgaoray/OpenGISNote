# CRS note
## CRSAuthorityFactory
### 根据epsg获取crs
<div  style="background-color:black;padding:4px">
<code>
    Hints hints = new Hints(Hints.FORCE_LONGITUDE_FIRST_AXIS_ORDER, longitudeAxisFirst);
    CRSAuthorityFactory factory = ReferencingFactoryFinder.getCRSAuthorityFactory("EPSG", hints);
    CoordinateReferenceSystem crs = factory.createCoordinateReferenceSystem(code);
</code>
</div>
<p style="color:gray">
    (1) throws FactoryException</br>
    org.opengis.referencing.FactoryException</br>
    (2) 依赖gt-epsg-hsql
</p>