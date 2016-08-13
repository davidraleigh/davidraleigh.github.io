---
layout: post
title: Geometry Micro-Service
---

![Logos!!!!](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/geometry_micro/Geometry-Micro-Service.png)

Recently I put myself to the challenge of automating a port of ESRI's [geometry-api-java library](https://github.com/Esri/geometry-api-java) from Java to C#. The [geometry micro-service](http://geometry.fogmodel.io/) demonstration is an application built using the resulting geometry-api-cs library.


The automated port of the library is done using the Sharpen-Eclipse Abstract Syntax Tree library and a whole mess of python scripting. It isn't a pretty thing. Anytime there is a change in the ESRI java code, a build process is triggered and the results are commited to the C# repo. I hosted a TeacmCity Continuous Integration server for the automation and I gotta say, I really love it.

It took a lot of work to get the Geometry library to compile and pass tests. There are a few operators that don't yet work. Among the non-working are the JSON import and export methods. And that means the [geometry.fogmodel.io](http://geometry.fogmodel.io) demo relies on WKT geometries for importing and exporting geometries. Import and export using ESRIShape binary is working, but that's not really too fun for interactive geometry editing in Leaflet.

![diagram](http://davidraleigh.io/content/images/2016/01/Geometry-Operator-Diagram-2.svg)

The client side of the demo uses React, Ampersand, Leaflet and websockets to allow for submitting requests to a Node server. That Node server then places requests on a RabbitMQ message queue server. A geometry micro-service is connected to the RabbitMQ message queue and processes geometry requests using the geometry-api-cs library. Those processed requests then find their way back to the client via RabbitMQ and websockets. It's pretty groovy stuff.
