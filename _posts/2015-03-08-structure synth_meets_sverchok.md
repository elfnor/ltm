---
author: elfnor
date: '2015-03-08 22:00'
image: 'seuss\_nouveu\_11.png'
layout: post
tags: ' blender sverchok structure-synth'
title: 'Structure Synth meets Sverchok - Generative Art inside Blender'
---

Update: This lsystem/Structure Synth code has now (March 2016) been incorporated into the Sverchok *Generative Art* node. See the [updated examples](%7Bfilename%7Dgenerative_art_example_updates.md) and the [node docs](%7Bfilename%7Dgenerative_art_docs.md)

After using the [\"Matrix Iterate\"](%7Bfilename%7Dsimple_sverchok_05.md) node to generate some of the simpler structures from [Structure Synth](http://structuresynth.sourceforge.net/). I started to think about how to implement more of Structure Synth directly in Blender. It turned out to be remarkably simple using a scripted node and some existing python code.

Structure Synth uses a domain specific language called eisenscript to define a design grammar to construct 3D structures (LSystems). Here\'s the eisenscript for a structure with 6 spirals.

    set maxdepth 400

    r0

    rule r0 {
      3 * { rz 120  } R1
      3 * { rz 120 } R2
    }

    rule R1 {
      { x 1.3 rx 1.57 rz 6 ry 3 s 0.99} R1
      { s 4 } sphere
    }

    rule R2 {
      { x -1.3 rz 6 ry 3 s 0.99} R2
      { s 4 }  sphere
    }

![matrix iterate sample image]({{ site.baseurl }}/images/matrix_iterate_13.png)

An eisenscript consists of a list of rules. Each rule contains a set of instructions. That instruction can either be to place an object or to call another rule. That call can be to to the rule doing the calling in a recursive fashion. Each instruction also has an associated transform to scale rotate or move the current coordinate system. The original eisenscript also allowed transforms on the colour and transparency of the object.

The above structure can be easily implemented in Sverchok using the \"Matrix Iterate\" node as shown in the [previous post](%7Bfilename%7Dsimple_sverchok_05.md).

The interesting bit is that each rule can have multiple definitions that are called randomly according to the weight assigned to each rule definition. This allows structures with random branches and other complexities. This random choice of rule is bit harder to implement directly in Sverchok nodes but I\'m thinking about it\...

In the meantime searching the web, I couldn\'t find anyone who had included a parser for eisenscript files in Blender (there is one in [MeshLab](http://meshlab.sourceforge.net/)), but I did find that [Little Grasshopper](http://github.prideout.net/) (aka Philip Rideout) had written some [scripts](https://github.com/prideout/lsystem) in python go and c++ to work with Structure Synth type LSystems and RenderMan.

He uses an xml form of eisenscript to define the design grammar. The above script looks like this in Philip\'s xml:

``` {.xml}
<rules max_depth="400">
    <rule name="entry">
        <call count="3" transforms="rz 120" rule="R1"/>
        <call count="3" transforms="rz 120" rule="R2"/>
    </rule>
    <rule name="R1">
        <call transforms="tx 1.3 rx 1.57 rz 6 ry 3 sa 0.99" rule="R1"/>
        <instance transforms="sa 4" shape="sphere"/>
    </rule>
    <rule name="R2">
        <call transforms="tx -1.3 rz 6 ry 3 sa 0.99" rule="R2"/>
        <instance transforms="sa 4" shape="sphere"/>
    </rule>
</rules>
```

From Little Grasshopper\'s [post](http://prideout.net/blog/?p=44)

> Every description must contain at least one rule named entry; this is the starting point for the evaluator. Note the mini-language used for specifying transformations. It's a simple string consisting of translations, rotations, and scaling operations. For example, `tx n` means "translate n units along the x-axis", `ry theta` means "rotate theta degrees about the y-axis", and `sa f` means "scale all axes by a factor of f".

In Philip\'s github [repo](https://github.com/prideout/lsystem) he provides a python module (`LSystem.py`) that parses the above type of xml strings and outputs a list of matrices and the name of the object to place at the location, scale and rotation defined by the matrix. This code is beautifully separated from any code used to implement or render the geometry and is perfect to incorporate in [Sverchok](http://nikitron.cc.ua/sverchok_en.html).

The module `LSystem.py` has two dependencies, `elementtree` for parsing the xml and `euclid` for the matrix multiplication. I replaced `elementtree` with `xml.etree.cElementTree` avaliable in the standard python included with Blender, and replaced `euclid` with the Blender `mathutils` module.

I also added a break to the code on reaching maximum number of objects as I kept crashing/locking Blender/Sverchok when I got more then a few 10s of thousands of objects.

From there all we need is a simple \"Scripted Node\" in Sverchok with the following code:

``` {.python}
from sverchok.data_structure import Matrix_listing

import LSystem_blender as LSystem
import GA_xml

import imp

max_mats = 5000

def sv_main(rseed=1):

    in_sockets = [['s', 'rseed', rseed]]
       
    imp.reload(GA_xml)
       
    tree = GA_xml.Library["Default"]
    lsys = LSystem.LSystem(tree, max_mats)
    shapes = lsys.evaluate(seed = rseed)
       
    mats = [shape[1] for shape in shapes if shape] 
    mat_out =  Matrix_listing(mats)
     
    out_sockets = [['m', 'matrices', mat_out]]
    
    return in_sockets, out_sockets
```

Note: The node code above is given to show how simple a Sverchok scripted node can be. I have since updated the code on my repository to do some other stuff.

See [here](http://sverchok.readthedocs.org/en/latest/nodes/generator/scripted_intro.html) if you need help working with the \"Scripted Node\" in Sverchok.

That was too simple and it just works! It\'s fast too. Before doing this I played around trying to implement the object generation directly in Blender python and had trouble getting fast enough code, let alone getting lost in `bpy.ops` and `bpy.context` calls.

The latest version of `LSystem_blender.py` and my Sverchok nodes is available on my github [repo](https://github.com/elfnor/lsystem) (forked from prideout/lsystem).

To use the GA node in Blender first install the [Sverchok](http://nikitron.cc.ua/sverchok_en.html) addon. Download the lsystem code from [github](https://github.com/elfnor/lsystem). Then load the three python files in the Blender directory (`LSystem_blender.py, GA_xml.py, GA_node.py`) as separate text blocks into a blend file. Add a \"Scripted Node\" to a Sverchok node tree. On the node select the `GA_node.py` code from the lower drop down. Then click the plugin icon to the right of this field. The node should turn blue with some inputs and outputs. Wire the matrices output to a \"Viewer Draw\" node and you should see some geometry as below.

![GA node diagram]({{ site.baseurl }}/images/Lsystem_pipe_05.blend.png)

The node has an input \"rseed\" which is used to set the random number generator. For a rule set that includes multiple definitions of rules, changing this will change the structure.

The Library of xml rule sets is stored in the `GA_xml.py` module. This allows you to easily change the rule set. In the last line of code `Library["Default"] = Library["Tree"]` change `Tree` to any of the other examples. You can also change the rules or add new ones. The geometry should change in the 3D View after clicking the \"Update Node Tree\" button in the Sverchok panel.

My computer chokes if I try to feed too many matrices into Sverchok. The `max matrices` sets a limit in the node to the number of matrices calculated and displayed. Change this depending on you and your computer\'s ability to cope with lockups and crashes. Note that the nodes get recalculated when you open a blend file. Saving a blend file with an LSystem that has a long calculating time will make it slow to open.

Using the matrices output allows a separate object to be placed at each location. The vertices input and the mesh (vertices, edges, faces) output \"skins\" the mesh into a much smaller number of objects. The mask node can be used if the rule set has more than one type of object. I\'ll describe these further in the next post but here\'s a preview of the mesh mode (with a subdivison modifier applied).

![GA tube nodes and render/screenshot]({{ site.baseurl }}/images/Fern.blend.png)

------------------------------------------------------------------------
