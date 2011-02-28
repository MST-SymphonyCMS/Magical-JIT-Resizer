Magical Image Resize Utility
============================

By The Workers London Ltd. 
 
# Overview
Magical image resizer and <img/> tag generator, with fallback to [DummyImage generator][dummy].

[dummy]: https://github.com/robphilp/Symphony-2-Dummyimage

## Required params:
- `upload`: takes an xPath of Symphony's `<upload>` node
    - if `upload` is omitted, it will generate a dummy image with the specified dimensions (`w` and `h` dimensions required in that case)

## Optional params:
- `'w'`: the requested width
- `'h'`: the requested height (both can be set)
- (standard `<img/>` attributes. `class`, `id`, `title`, `name`, `longdesc`, etc.)
- `value-only`: if set to either `'w'` or `'h'` will return only the result number not the whole `<img/>` tag.
- `mode`: `'normal'`, `'fit'`, `'max'`
    - normal: none, `'w'`, `'h'`, `'w'` and `'h'`
    - fit: `'w'` and `'h'`
    - max: `'w'`, `'h'`, `'w'` and `'h'`
    In normal mode the scaler takes whatever `h` and/or `w` is passed and scales the specified dimensions to whatever value was passed.  
    If either `w` or `h` are passed, then the missing value is derived proportionally. If both values are passed then both are applied, effectively constraining the image to a specified size.  
    In fit mode both h and w are required. The two values define the area within which the scaler has to proportionally fit the image.            
- `JITmode`: JIT modes (see docs on Symphony-CMS.com) 
- `JITcenter`: JIT center `1` to `9`
- `JITexternal`: `0` or `url` (without the `http://` part)

## Examples

### Output original size
_No explicit size = original size_

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
    </xsl:call-template>

### Force a specific size
_Specifying both dimensions, using no explicit mode, will output exactly what you asked for._

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="w" select="300"/>  
      <xsl:with-param name="h" select="200"/>  
    </xsl:call-template>

### Scale proportionally based on width or height constraint
_To trigger this simply omit one dimension_

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="w" select="300"/>  
    </xsl:call-template>

### Scale proportionally within a set bounding box (square or not)
_This is useful if you want to proportinally constrain your images to set dimensions._

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="mode" select="'fit'"/>  
      <xsl:with-param name="w" select="400"/>  
      <xsl:with-param name="h" select="400"/>  
    </xsl:call-template>

### Set a grid and trim exceeding values from automatically calculated dimensions
_Makes sure that any automatically calculated value will be a (nearest neighbor) multiple of the grid you set._

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="mode" select="'trim'"/>  
      <xsl:with-param name="gridsize" select="50"/><!-- This will set a 50px grid -->
      <xsl:with-param name="w" select="400"/>  
      <!-- i.e.: 'h' will be trimmed to the nearest neighbor grid -->
    </xsl:call-template>

### Set a grid and use multipliers
_Straighforward. W and H values are used as multiplier in this case._

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="gridsize" select="50"/><!-- This will set a 50px grid -->
      <xsl:with-param name="w" select="3"/> <!-- When in 'pure' grid mode, these values act as multipliers -->
      <xsl:with-param name="h" select="2"/>  
    </xsl:call-template>

### Use and external image
_w and h needed as we don't have XML metadata on the fetched image_

    <xsl:call-template name="img">   
      <xsl:with-param name="upload" select="'images.apple.com/home/images/mbp_hero20110224.png'"/>  
      <xsl:with-param name="w" select="400"/>  
      <xsl:with-param name="h" select="200"/>  
    </xsl:call-template>

### Generate a dummy image (needs Dummy Image extension)
_Note that we are not passing any upload param in this case._

    <xsl:call-template name="img">
      <xsl:with-param name="w" select="200"/>   
      <xsl:with-param name="h" select="150"/>  
    </xsl:call-template>

### Use it as a MATCHED template

    <xsl:apply-templates match="upload">  
      <xsl:with-param name="w" select="300"/>  
    </xsl:apply-templates>

### Add attributes (works in all modes)

    <xsl:call-template name="img">  
      <xsl:with-param name="upload" select="$upload"/>  
      <xsl:with-param name="w" select="500"/>  
      <xsl:with-param name="class" select="'someclass anotherclass'"/>  
      <xsl:with-param name="id" select="'myID'"/>  
      <xsl:with-param name="title" select="'Herr Title'"/>  
    </xsl:call-template>

## Notes
When this template is used as a match rather than called by name, the DummyImage mode is effectively inaccessible.  
Besides, it wouldn't make sense since you if have a match, then you have an upload.

By The Workers London Ltd. [theworkers.net][w] - [io@theworkers.net][io]

[w]: http://theworkers.net/
[io]: mailto:io@theworkers.net