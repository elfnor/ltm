---
author: elfnor
date: '2014-09-14 22:00'
image: 'Screenshot-drop\_down\_01.png'
layout: post
tags:  copy2
title: 'Drop down and button select menus for Blender Operator add-ons'
---



Note: Examples were tested in Blender 2.71 running on Linux Mint.

While making the fancy new interface for my [copy2 add-on]({{ site.baseurl }}{% link _posts/2014-09-02-copy2_docs.md %}) I needed to work out how to display choices to the user. There are many examples around for drop down menus etc. So many examples that I got rather confused to start with. Here is my best attempt at simplifying this to the essentials.

If you\'re new to writing an operator add-on this is the best place to start is with this add-on tutorial at [blender.org](http://www.blender.org/documentation/blender_python_api_2_71_release/info_tutorial_addon.html)

Any add-on tool you write to add new objects or manipulate objects will typically be a blender `Operator` class. This will give you a tool that can be accessed from the existing menus or via a keyboard shortcut. This class gives heaps of functionality such as an easily constructed options panel for the user to interact with and automatically see their changes update.

## Simple drop down

The following add-on has a simple drop down menu.

```python

import bpy    
              
class DropDownExample(bpy.types.Operator) : 
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"} 
    
    fixed_items = bpy.props.EnumProperty(items= (('0', 'A', 'The zeroth item'),    
                                                 ('1', 'B', 'The first item'),    
                                                 ('2', 'C', 'The second item'),    
                                                 ('3', 'D', 'The third item')),
                                                 name = "fixed list")      
    def execute(self, context) :  
        print("fixed item", self.fixed_items)  
        return {"FINISHED"} 
    
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_MT_object.append(add_to_menu)  
         
def unregister() :  
    bpy.utils.unregister_module(__name__)   
    bpy.types.VIEW3D_MT_object.remove(add_to_menu)  
    
if __name__ == "__main__" :  
    register()
```

All we had to do was add the `bpy.props.EnumProperty` as a class attribute. When this code is run blender adds an item `Drop Downs` (the name is defined by `bl_label`) to the `Object` menu in the `VIEW3D`. When clicked this new menu item runs the add-on, and blender creates a sub-panel in the tool panel automatically constructing a drop down with the defined items on it. Each time the user changes the selection on the drop down the `execute` method is called. In this case it simply prints the value of the selection to the terminal (if blender was started from a terminal).

The items list contains a list of tuples like so: `(value, label, description)`. The `value` is what blender return when the attribute is referenced as in the `print(self.fixed_items)` will print the `value` of the selected item. The `label` is what blender displays on the drop down. The `description` is what the user sees when they hover the mouse over the item.

The `name` is the label displayed above the drop down. Set this to an empty string to have no label.

## Selection buttons

You can have more control over how the `EnumProperty` is displayed to the user by adding a `draw` method to the class.

The `layout.prop(self, "radio", expand=True)` changes the drop down to what are sometimes called \"radio buttons\".

```python
import bpy    
              
class DropDownExample(bpy.types.Operator) : 
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"} 
    
    radio = bpy.props.EnumProperty(items= (('SW', 'Shortwave', 'The zeroth item'),      
                                           ('AM', 'AM', 'The first item'),      
                                           ('FM', 'FM', 'The second item'),     
                                           ('NET', 'Internet', 'The third item')),  
                                   name = "radio buttons") 

    def draw(self, context) :  
        layout = self.layout  
        layout.prop(self, "radio", expand=True)    
                                                                 
    def execute(self, context) :  
        print("fixed item", self.radio)  
        return {"FINISHED"} 
    
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_MT_object.append(add_to_menu)  
         
def unregister() :  
    bpy.utils.unregister_module(__name__)   
    bpy.types.VIEW3D_MT_object.remove(add_to_menu)  
  
   
if __name__ == "__main__" :  
    register()
```

![drop down 02]({{ site.baseurl }}/images/Screenshot-drop_down_02.png)

Within the draw method quite complex layouts can be defined if necessary. See these [examples](http://wiki.blender.org/index.php/Dev:2.5/Py/Scripts/Cookbook/Code_snippets/Interface) on the blender site.

## Icons

The text can be replaced with icons if desired. In this case each item tuple now has 5 entries.  
`(value, label, description, icon name, unique number)`  
The icons avaliable and their names can be found using the `Development: Icons` add-on. This adds a panel to the properties panel (`CTRL-T`) of the Text Editor view. Hovering the mouse over an icon gives it name and clicking and icon copies its name to the clipboard.

```python
import bpy    
              
class DropDownExample(bpy.types.Operator) : 
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"} 
    
                                                                                      
    icon =  bpy.props.EnumProperty(items= (('V', '', 'use vertices', 'VERTEXSEL', 0),    
                                           ('E', '', 'use edges', 'EDGESEL', 1),    
                                           ('F', '', 'use faces', 'FACESEL', 2)) ,  
                                   name = "Copy To:",  
                                   description = "place to copy to")  

    def draw(self, context) :  
        layout = self.layout  
        layout.prop(self, "icon", expand=True)    
                                                                 
    def execute(self, context) :  
        print("fixed item", self.icon)  
        return {"FINISHED"} 
    
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_MT_object.append(add_to_menu)  
         
def unregister() :  
    bpy.utils.unregister_module(__name__)   
    bpy.types.VIEW3D_MT_object.remove(add_to_menu)  
  
   
if __name__ == "__main__" :  
    register()
```

![drop down 03]({{ site.baseurl }}/images/Screenshot-drop_down_03.png)

## Dynamic Lists

A callback function can be used to generate the items for the list dynamically. This is useful, for example, for making a list of the objects in the scene for the user to select from. The callback needs to go at the top of the class above the `bpy.props.EnumProperty` that uses it.

```python
import bpy  
                
class DropDownExample(bpy.types.Operator) :  
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"}  
      
    def item_cb(self, context):  
        return [(ob.name, ob.name, ob.type) for ob in bpy.context.scene.objects]  
        
    objname = bpy.props.EnumProperty(items=item_cb,  
                                         name = "Object",  
                                         description = "Choose object here")                               
                                             
    def execute(self, context) :  
        print("object name", self.objname)  
 
        return {"FINISHED"}  
             
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_PT_tools_object.append(add_to_menu)   
         
def unregister() :  
    bpy.utils.unregister_module(__name__)   
    bpy.types.VIEW3D_PT_tools_object.remove(add_to_menu)   
  
   
if __name__ == "__main__" :  
    register() 
```

![drop down 04]({{ site.baseurl }}/images/Screenshot-drop_down_04.png)

The callback should return a list of tuples. The above example will update the list of objects every time the user changes the selection. This may not be what you want if the add-on adds lots of new objects to a scene. For the Copy2 add-on I wanted the list to only contain objects that were in the scene when the user started the add-on.

The `invoke` method of the class is only called when the add-on is started. In the following example this is the only place the object list is updated. I\'ve added some code that duplicates the object selected in the drop down 10 times. The new objects are not added to the drop down list until the add-on is rerun from the menu. Note also that as you change which object is selected in the drop down any previously duplicated objects are removed from the scene. This is part of the default undo behaviour of an Operator.

Of course if you where to make this into a real add-on, it would be best to add the number of objects and the vector direction as user selections. These would be a `bpy.props.IntProperty` and a `bpy.props.FloatVectorProperty`.

```python
import bpy  
from mathutils import Vector
                
class DropDownExample(bpy.types.Operator) :  
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"}  
      
    obj_list = [(obj.name, obj.name, obj.name) for obj in bpy.data.objects]  
          
    def obj_list_cb(self, context):  
        return DropDownExample.obj_list 
        
    objname = bpy.props.EnumProperty(items=obj_list_cb, 
                                         name = "Object",  
                                         description = "Choose object here")   
                                         
    def invoke(self, context, event):  
        DropDownExample.obj_list = [(obj.name, obj.name, obj.name) for obj in bpy.data.objects]  
        return {"FINISHED"}                             
                                             
    def execute(self, context) :  
        print("object name", self.objname)  
        for i in range(10):
            copy_obj = bpy.data.objects[self.objname].copy()
            bpy.context.scene.objects.link(copy_obj)
            copy_obj.location = copy_obj.location + Vector((1.0*i, 0.0, 0.0))
    
        return {"FINISHED"}  
             
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_PT_tools_object.append(add_to_menu)   
         
def unregister() :  
    bpy.utils.unregister_module(__name__)   
    bpy.types.VIEW3D_PT_tools_object.remove(add_to_menu)   
  
   
if __name__ == "__main__" :  
    register() 
```

If you add the duplication code to the 1st example in this section, the list will update with all the new objects. Not only does this clutter up the list, strange things happen to which item blender thinks is selected.

## Selections changing other list items.

A callback function for the items list can be used to make one list update its items in response to a user selection in another list. I needed this in the Copy2 add-on where the user needs to select a primary and secondary axis. The primary and secondary axes can\'t be the same. I could of offered the user a single choice of one of the six pairs but I decided to make the secondary axis list update in response to the primary axis choice.

```python
import bpy    
              
class DropDownExample(bpy.types.Operator) : 
    bl_idname = "mesh.dropdownexample"  
    bl_label = "Drop Downs"  
    bl_options = {"REGISTER", "UNDO"} 
    
    def sec_axes_list_cb(self, context):  
        if self.priaxes == 'X':  
            sec_list = [('Y','Y','Y'), ('Z', 'Z', 'Z')]  
           
        if self.priaxes == 'Y':  
            sec_list = [('X','X','X'), ('Z', 'Z', 'Z')]  
              
        if self.priaxes == 'Z':  
            sec_list = [('X','X','X'), ('Y', 'Y', 'Y')]       
        return sec_list
    
    priaxes = bpy.props.EnumProperty(items=(('X', 'X', 'along X'),  
                                             ('Y', 'Y', 'along Y'),  
                                             ('Z', 'Z', 'along Z')),  
                                             )  
                                                                                                
    secaxes = bpy.props.EnumProperty(items=sec_axes_list_cb, name='Secondary Axis')  
      
    def draw(self, context):  
        layout = self.layout  
           
        layout.label("primary axis:")   
        layout.prop(self, 'priaxes', expand=True)  
        layout.label("secondary axis:")   
        layout.prop(self, 'secaxes', expand=True)  
            
        return                   
    
                   
    def execute(self, context) :  
        print("axes", self.priaxes, self.secaxes)  
        return {"FINISHED"} 
    
def add_to_menu(self, context) :  
    self.layout.operator("mesh.dropdownexample", icon = "PLUGIN")  
  
def register() :  
    bpy.utils.register_module(__name__)       
    bpy.types.VIEW3D_MT_object.append(add_to_menu)  
         
def unregister() : 
```

## Selections changing which properties are displayed

Another way the panel can be made responsive to user selection, is to offer additional properties based on what selections are already made. In the Copy2 plugin the scale options only work in edge mode, so I made it so they are only drawn when edge mode is selected. This is done with some `if` statements in the draw method.

For a example of all these ideas in a real add-on have a look at the source code of the [Copy2 add-on]()
