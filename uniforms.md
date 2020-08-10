# Uniforms

ShadersMod & OptiFine define many uniforms that allow shaders to access the current state of the world, the player, and the renderer.

Note: Some of these uniforms are not supported by ShadersMod. They will take on an initial/default value of `0`, per the OpenGL Specification.

## Player

### Held item ID

Item ID of the item currently held in the main hand. 

For OptiFine, the item ID is defined by the mapping in `items.properties`.

> TODO
> * OptiFine: What about when nothing is held? (probably air)
> * OptiFine: What about when the currently held item isn't defined in `items.properties`?

For ShadersMod, which does not support `item.properties`, the ID is defined by the Anvil (pre-1.13 world format) ID mapping of the currently held item. If the player is currently not holding an item, an ID of `-1` is provided. For example, if the player was holding a stone block, the value would be `1`.

#### Declaration

```glsl
uniform int heldItemId;
```

#### Implementation Support

* ✔️ ShadersMod
* ✔️ OptiFine


### Held block light value

Light value of the block that the player is currently holding in their hand.

> TODO:
> * For OptiFine, the exact behavior is unknown. It's probably similar to ShadersMod.

For ShadersMod, the light value, ranging from 0 (no light) to 15 (brightest), of the block's default block state is provided. If the player is not currently holding an item, or the currently held item is not a block, a value of 0 is provided.

For example, if the player is currently holding a block of Glowstone, a value of 15 will be provided, but if they are holding a block of Stone, a value of 0 will be provided.

#### Declaration

```glsl
uniform int heldBlockLightValue;
```

### Held item ID (off hand)

This is the same as the main [Held item ID](#held-item-id), but instead referring to the off hand (most commonly the left hand).

#### Declaration

```glsl
uniform int heldItemId2;
```

#### Implementation Support

* ❌ ShadersMod
* ✔️ OptiFine

### Held block light value (off hand)

This is the same as the main [Held block light value](#held-block-light-value), but instead referring to the off hand (most commonly the left hand).

#### Declaration

```glsl
uniform int heldBlockLightValue2;
```

#### Implementation Support

* ❌ ShadersMod
* ✔️ OptiFine


## Sky and fog

### OpenGL fog mode

Holds the current OpenGL fog mode, or 0 if there is no fog. This is equivalent to the value of `glGetInteger(GL_FOG_MODE)`.

The following values are valid:

| Name      | Decimal | Hex    |
| --------- | ------- | -------|
| GL_LINEAR | 9279    | 0x2601 |
| GL_EXP    | 2048    | 0x0800 |
| GL_EXP2   | 2049    | 0x0801 |

For more information on fog modes, see the [official OpenGL documentation](https://www.khronos.org/registry/OpenGL-Refpages/gl2.1/xhtml/glFog.xml#description).


#### Declaration

```glsl
uniform int fogMode;
```

#### Implementation Support

* ✔️ ShadersMod
* ✔️ OptiFine


### Fog density

The current fog density. Ranges from 0.0 to 1.0.

> TODO:
> * Find out more about this value

#### Declaration

```glsl
uniform float fogDensity;
```

#### Implementation Support

* ❌ ShadersMod
* ✔️ OptiFine


### Fog color

The current color of the fog.

Each color channel has a float value of 0.0 to 1.0, corresponding to RGB values from 0 to 255. Note that the values are encoded in the non-linear sRGB color space: to receive linear RGB values, apply the function `pow(x, 2.4)` as described [on Wikipedia](https://en.wikipedia.org/wiki/SRGB#The_reverse_transformation).

> TODO:
> * Make sure that the values are actually sRGB!

#### Declaration

```glsl
uniform vec3 fogColor;
```

#### Implementation Support

* ✔️ ShadersMod
* ✔️ OptiFine