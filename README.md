# Fontaine

> **WARNING: THIS LIBRARY IS NOT FUNCTIONAL YET!**

This library is meant to provide a solid base for font rendering. It's goals are:

- Provide text layouting
  - Create a sequence of positioned glyphs for a given input string
  - High level text rendering, will handle word wrap, alignment, maximum width, ...
- Provide string layouting
  - Create a sequence of positioned glyphs for a given input string
  - Bare bones function, will not handle lines or word wrap
- Provide glyph information
  - Beziers
  - Alpha texture rendering

## Example

```zig
const fontaine = @import("fontaine");

// the font size is "12 units", renderer has to translate this later on
var font = try fontaine.Font.load("berkeley-mono.ttf", 12.0);
defer font.deinit();

const text =
  \\My name is Marquise and i'm from France!
  \\എന്റെ പേര് മാർക്വിസ്, ഞാൻ ഫ്രാൻസിൽ നിന്നാണ്!
  \\私の名前はマルキーズで、フランス出身です！
  \\我的名字是侯爵夫人，我来自法国！

var iter = fontaine.layoutText(font, text, .{
  .alignment = .justify,
  .max_width = 200, // units
  .max_height = 400, // units
});

while(iter.next()) |glyph_desc| {
  const bitmap = try glyph_cache.getScaled(glyph_desc.glyph_index, font.size);
 
  renderer.drawBitmap(glyph_desc.x, glyph_desc.y, bitmap);
}
```
