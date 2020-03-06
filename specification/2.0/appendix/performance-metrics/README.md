
|                      |                           |                                           | 
|----------------------|---------------------------|-------------------------------------------|
| Name |Property   |Description  |
|[vertexcount]       |Scene       |Total number of vertices used by a model in a scene|  
|[nodecount]         |Scene       |Max nodecount in scene (add upp all nodes in a scene)|  
|[primitivecount]    |Scene       |Total number of referenced primitives (per scene).  This figure is the un-batched number of primitives, engines may optimize if primitives and meshes share textures.  |  
|[textures]          |Scene       |Flags specifying presence of materials and it's texture usage, this is the aggregated max usage. BASECOLOR, METALLICROUGHNESS, NORMAL, OCCLUSION, EMISSIVE, (SPECULARGLOSS)  

# Scene #
Metrics that describe properties of a scene.  
glTF models may contain multiple scenes, only one scene will be visible at a time.  
To accomodate this the following metrics are calculated on a per-scene basis.  
It is possible to reference different meshes (nodes) using different scenes, this makes it possible to have the same texture assets but reference different primitives that use alternative materials (textures).  
Ie one model could have 3 different scenes, where the same accessors (position, uv) are used but different primitives that reference materials with varying number of texture channels.  

[nodecount]  
This represent the number of traversed nodes in a scene.  
This is calculated by traversing nodes in each scene (depth or breadth first does not matter) - for each node increase the nodecount.  

[verticecount]  
This represents the number of drawn vertices for each scene in the model.  
This is calculated by traversing the nodes in each scene.  
For each mesh use the POSITION attribute in each primitive, get the Accessor and add up the count field.  

[primitivecount]  
This represents the number of primitives for each scene  
Each primitive references a material with 0 or more texture sources, has attributes and accessors.  


[textures]  
This value represents the texture sources that are used by a scene.  
This is the max value from the most 'complex' primitive that will be referenced by a Node in the scene.  
The goal of this metric is to provide a worst case texture usage where texturecount and complexity is known.  
Ie it is known if a texture is BASECOLOR or if part of PBR such as METALICROUGHNESS and now many texture sources that are used.  


# JSON #  
This is how the output would be formatted using JSON  

"scene" : [ { "verticecount" : 4300, "nodecount" : 20, "primitivecount" : 50 ,
              "textures" : ["BASECOLOR", "METALLICROUGHNESS"]} ]  