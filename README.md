# njaPatcher
njaPatcher is a reverse engineering of NJA (Ninja Ascii) and NJ (Ninja Chunk) file formats used in many SEGA Dreamcast games. Currently njaPatcher can generate NJA files from OBJ as OBJ to NJA, or OBJ to NJ. njaPatcher is not intended to infringe on the rights of other software companies, and is only made with intention of moving an already available process out of being restricted to Windows 98/Windows NT/Windows XP machine.

## njaPatcher capabilities
njaPatcher can auto detect some types of OBJ, it can determine whether the output should have a texture list and normals list, producing different versions of NJA.

The process of making a working NJA/NJ model is a 100% success if njaPatcher is implemented correctly, but the process of creating a Wavefront OBJ file has a strict set of requirements. These requirements are listed within the OBJ requirements section of this document.

# How to use njaPatcher
The current repo is still in early development and is refactored in a minimal way, the testing browser/console for njaPatcher is Google Chrome, so it's recommended while njaPatcher is still in alpha version. All functionality for this version of njaPatcher is controlled through the "main.js" script.

In the latest update for njaPatcher, there is a JavaScript object to replace the previous functionality of editing variables in the "njaPatcher.js" script. A new script is included named "njPatcher.js", which has dependency on "njaPatcher.js", njaPatcher cannot currently return data in a mutable state, since the focus is creating an accurate conversion process first.

## Included files

The included HTML/CSS with an added "main.js" script is setup to export an NJ with texture coordinates, shade smooth normals and triangulated faces, since this is required for NJ output. The default NJ output is a resource OBJ file listed below.

The repo currently contains 3 test OBJ files:

1) A directional cube which shows coordinate orientation (XYZ) in respect to Blender's default viewport. This model has both textures and normals. Also included is the texture for this object in PNG and DDS format, to be used in a 3D editor.

2) A cube with normals, this model does not have textures and can be used to visualize the difference between a textured NJA (NJTL) and a non-textured NJA (NJCM). This type of file is not supported for NJ output at the moment.

3) A weapon style object to visualize the 3D object scaling (low-mid vertex count). This OBJ is the default file loaded in njaPatcher.

## njaPatcher object
An object of njaPatcher can be created. The objFilePath parameter should be the folder intended to load OBJ files from. An OBJ folder is setup for this version of njaPatcher.

`let nja = new njaPatcher(objFilePath)`

String: **objFilePath** is required.

## njaPatcher functions
### loadOBJ(obj)
This function will output NJA if used by itself, the `obj` parameter is the name of the OBJ file to be converted, including the extension `".obj"`. `loadOBJ(obj)` is the only function required to produce NJA output.

`nja.loadOBJ(obj)`

String: **obj** is required.

### flashNJ()
This function will output an NJ file during the `loadOBJ(objFilePath)` function.

`nja.flashNJ()`

Boolean.

### objScale(scalar)
This will change the scale of OBJ faces by adding a scalar to each vert, the resize will not display in model viewers like Noesis.

`nja.objScale(3.0)`

Floating point: **scalar** is required.

### forceNormals(x, y, z)
This will force normal entries into NJA or NJ output, the main usage of this is for analysis of either file, since it will create a pattern within their data structures. It can still be used to stylize objects with a flat shading model.

`nja.forceNormals(1.0, 1.0, 1.0)`

Floating point: **x** is required  
Floating point: **y** is required  
Floating point: **z** is required  

### forceTextureCoords(x, y)
This will force texture coordinate entries into NJA or NJ output, the main usage of this is for analysis of either NJA or NJ, since it will create a pattern within their data structures. Texture coordinates cap out at `nja.forceTextureCoords(256, 256)` for NJA/NJ files.

`nja.forceTextureCoords(256, 256)`

Integer: **x** is required  
Integer: **y** is required  

### hideHex()
This function will remove hex from showing in NJA files, doesn't work with NJ since NJ is outputted as hex.

`nja.hideHex()`

Boolean.

### unloadTextures(), unloadNormals()
Two more functions for analysis of NJA and NJ, they unload all normals and texture coordinates, both will break NJ files, but the error is displayed in HTML for now.

```
nja.unloadTextures()
nja.unloadNormals()
```

Boolean.

### optimizeStrips()
This function is seriously not recommended for general usage, it will optimize triangle strips size, but it is extremely slow in it's current state and will make njaPatcher hang. The effectiveness of this function can be seen when processing a small OBJ (100 verts~), but is technically unnecessary.

`nja.optimizeStrips()`

Boolean.

### details(name, custom_comment)
This function is not necessary to use, but will add some extra text into an NJA output. The next version will allow adding a permanent comment into NJ (NJTL) files.

`nja.details(name, custom_comment)`

String: **name** is required  
String: **custom_comment** is required  

## OBJ requirements

Before running an OBJ through njaPatcher, various adjustments need to be made to the OBJ to get working results. All of those adjustments can be made in the 3D editor used (tested with Blender), the adjustments are listed below.

* Export as triangulated mesh (do this on export, don't permanently add tris to your mesh it will be pain to re-adjust)
* Make sure shade smooth is enabled in the viewport before exporting to OBJ
* Export OBJ as single object (no support for groups)
* Don't write materials to OBJ on export
* Use a 256x256 texture, can use any square image, but the UV coordinates should be inside 256x256

It's not recommended to exceed 1000 verts with the OBJ, recommended max for now is around 750 verts, but anything around 1000-1500 verts can cause maximum stack call for the time being.

njaPatcher doesn't require a texture input, and doesn't process them either, it's only concerned with UV mapping. For games that support AFS, it is possibly required to convert the image texture to DDS before using it.  

# Future of njaPatcher

At the current state of development it should now be possible to implement features like warping OBJ models before conversion, so some small features like this may be implemented to transform the OBJ and modify texture coordinates before conversion.

Ideally njaPatcher will support batch conversion of OBJ to NJA and OBJ to NJ in the future, and better detection for unusable OBJ files.

Another aim for njaPatcher is to have it producing more types of NJ files, e.g., purely NJCM files or modified versions of NJTL and NJCM.

It's not recommended to add to the current code base, as there is no entity relationships in place yet, but feel free to use any code from the project whilst bearing in mind it will be updated later.

# Credits

Special thanks to DOOMGUY, Rize and the PSOBB dev and modding communities.
