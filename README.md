# Leaflet.js Editable Polylines plugin

**Work in progress**

## Why

Most editable polylines have performance problems on bigger polylines because of too many markers shown on a map.
This Leaflet polyline plugin will show editable markers only for a selected part of the map.

## Examples

 * [Simple example](http://tkrajina.github.io/leaflet-editable-polyline/example1.html)
 * [Example with 20,000 points](http://tkrajina.github.io/leaflet-editable-polyline/example2.html) (you need to zoom close enough to be able to edit it).

## How to...

Initialization:

    L.tileLayer(osmUrl, {minZoom: 0, maxZoom: 15, attribution: osmAttrib}).addTo(map);

    var coordinates = [ [45.2750072361, 13.7187695503], [45.2757622049, 13.7198746204], [45.2763963762, 13.7197780609] /*, ...*/ ];

    var polyline = L.Polyline.PolylineEditor(coordinates, {maxMarkers: 100}).addTo(map);

The initialization method is:

    L.Polyline.PolylineEditor(coordinates, options, contexts)

...or...

    L.Polyline.PolylineEditor(coordinates, options)

Options is a normal Leaflet polyline options object with some additions:

 * **maxMarkers** is a max number of editable markers to be shown. That means that if the current map bounds are such that more than maxMarkers points of the polyline is shown -- the polyline **will not be editable**. This can be used for very large polylines where too many editable markers would make editing too CPU-intennsive.
 * **pointIcon** icon to be shown for point markers.
 * **newPointIcon** icon to be shown for middle point markers (markers used when creating new points).

The editing can be done by:

 * **drag the point marker** to move it around
 * **right-click on point marker** to __remove__ the point
 * **drag the middle point marker** to create new points between two existing
 * **right-click on middle point marker** to __split__ the point
 * **click on the first or last point** to add a new first/last point

When the editing is done, you can retrieve all points with:

    var points = polyline.getPoints()
    for(var i = 0; i < points.length; i++) {
        var point = points[i];
        console.log('latLng=' + point.getLatLng());
        if(point.context) {
            // The original position of this point (new point may be added or 
            // removed before this one when editing):
            console.log('originalPointNo=' + point.context.originalPointNo);
            console.log('originalPolylineNo=' + point.context.originalPolylineNo);
        }
    }

The **contexts** object-like variable may be an array (same length as coordinates!).
Values from the contexts will be stored with every point and can be retrieved later with point.context.

You can add as many editable polylines as you need. 
The resulting polylines can be retrieved with:

    var polylines = map.getEditablePolylines()

## License

Leaflet.Editable.Polyline is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)

