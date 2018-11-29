epanet.js and swmm.js
=====================

JavaScript version of EPANET and SWMM. No installation will be required. Data will not be sent to the server.

This is an updated version of the one found at [sdteffen/epanet.js](https://github.com/sdteffen/epanet.js) ( Great job done there! )

To view the original one have a look at :

Demo: [epanet.de/js](http://epanet.de/js/)

Detailed information: [epanet.de/developer/epanetjs.html](http://epanet.de/developer/epanetjs.html)

Requirements
============

[Emscripten](http://emscripten.org)'s emcc c-to-js compiler.

Compilation for epanet.js
=========================

A shell like Bash need to be used to build epanet.js. 
1. Download epanet source files from https://www.epa.gov/sites/production/files/2018-10/en2source.zip
2. Unzip the files into the desired folder. I will use for our example /home/test/epanet. Go into /home/test/epanet .
3. Modify the file epanet.c as follows:
.....

/*** New compile directives ***/  //(2.00.11 - LR)

#define CLE     /* Compile as a command line executable */

//#define SOL     /* Compile as a shared object library */

//#define DLL       /* Compile as a Windows DLL */ 


.....

4. Put the content of /js folder into /home/test/epanet.
5. Download shell.html or create a new one that has a similar structure.
6. Run the following command 

emcc -O1 epanet.c hash.c hydraul.c inpfile.c input1.c input2.c input3.c mempool.c output.c quality.c report.c rules.c smatrix.c -o js.html --pre-js js/pre.js --post-js js/post.js --shell-file shell.html --js-library js/epanet.js -s EXPORTED_FUNCTIONS="['_main', '_hour']"

5. Use it and enjoy!

SAMPLE FOR EPANET.js
====================

![Example of how it's used in an application](https://github.com/bogdanvaduva/epanet.js-and-swmm.js/blob/master/epanet.gif)

TODO
====
to modify the generated js.js to deal with null nodes.

to change sample.html 

to add swmm.js

Libraries
=========

epanet.js uses several libraries:

* [Emscripten](http://emscripten.org)
* [D3.js](http://d3js.org)
* [jQuery](http://jquery.com)
* [Bootstrap](http://getbootstrap.com)
* [FileSaver.js](https://github.com/eligrey/FileSaver.js/)

DATA MODEL
==========

I am using QWAT and QGEP data model ( Great job! )

In order to be able to export data from QWAT to EPANET we need to know if the water is flowing from one point (node) to another. I am using pgRouting and a few function wrote by myself.

We know that pgRouting extends the PostGIS / PostgreSQL geospatial database to provide
geospatial routing functionality. On the other hand we know that QWAT models the water network.
In this model we have pipes and we have nodes. In order to find the water flow in QWAT we need
to have edges (pipes), start node and end node. The good thing about model is that it already defines
node A and node B for a pipe.

In my environment I am spliting the pipes only when two or more pipes meet. The 'meeting pipes' will have not to have the
function "branchement privé". In the following picture there is an example of how my
network is laid out:

![Example of how it's used in an application](https://github.com/bogdanvaduva/epanet.js-and-swmm.js/blob/master/fig1.png)
