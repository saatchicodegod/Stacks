# Migrating from Stacks v1 to v2

With the release of Stacks v2, we've made some breaking changes to the library. The purpose of this guide is to help you prepare your code be compatible with the changes in Stacks v2.

## Table of contents

- [Upgrading dependencies](#upgrading-dependencies)
- [Using Stacks v2 colors](#using-stacks-v2-colors)
  - [Color stop mappings](#color-stop-mappings)
    - [Orange, blue, green, red, yellow, theme-primary, theme-secondary](#orange-blue-green-red-yellow-theme-primary-theme-secondary)
    - [Black](#black)
    - [Gold, sliver, bronze](#gold-sliver-bronze)
    - [Regular expression patterns](#regular-expression-patterns)
  - [Deprecation of powder color set](#deprecation-of-powder-color-set)
  - [Referencing theme colors without stop suffix](#referencing-theme-colors-without-the-stop-suffix)
- [Deprecation of Less color variables](#deprecation-of-less-color-variables)
  - [Replacing Less color variables](#replacing-less-color-variables)
  - [Referencing Less color variables](#referencing-less-color-variables)
- [Using legacy colors](#using-legacy-colors)
  - [Using the `-legacy` suffix](#using-the--legacy-suffix)
  - [Examples of legacy color references](#examples-of-legacy-color-references)
  - [Helpful regular expressions](#helpful-regular-expressions)
    - [Add `-legacy` suffix to colors](#add--legacy-suffix-to-colors)
    - [Add `-legacy` to aliased values](#add--legacy-to-aliased-values)
- [Custom theme generation](#custom-theme-generation)
  - [Replacing `.generate-calculated-themed-variables()`](#replacing-generate-calculated-themed-variables)
- [Deprecation of `.s-btn__primary`](#deprecation-of-s-btn__primary)

## Upgrading dependencies

While updating to Stacks v2, you must also update other Stacks dependencies you may have included in your project. Below is a table of Stacks dependencies and the corresponding minimum versions compatible with Stacks v2.

Dependency | Minimum version
:-:|:-:
[stacks](https://github.com/StackExchange/Stacks) | [2.0.0](https://www.npmjs.com/package/@stackoverflow/stacks/v/2.0.0)
[stacks-editor](https://github.com/StackExchange/Stacks-Editor) | [0.9.0](https://www.npmjs.com/package/@stackoverflow/stacks-editor/v/0.9.0)
[stacks-icons](https://github.com/StackExchange/Stacks-Icons) | [6.0.0](https://www.npmjs.com/package/@stackoverflow/stacks-icons/v/6.0.0)

## Using Stacks v2 colors

In Stacks v2, all components and atomic classes have been updated to use new color sets optimized for accessibility. In the process, the previous colors have been deprecated and the number of color stops has been reduced. This section will help you migrate to use the new colors.

### Color stop mappings

Stacks v2 reduces and unifies the number of color stops for each set of colors. Below, you'll find a series of tables that map the old color stops to the new color stops. Update your color references by mapping them to the new stop values.

> **Note**
> The `powder` color set has been deprecated and is not included in the updated colors. See the [deprecation of powder color set](#deprecation-of-powder-color-set) section for more information.

#### Orange, blue, green, red, yellow, theme-primary, theme-secondary

Stacks v1 | Stacks v2
:-:|:-:
025*† | 100
050, 075* | 200
100, 150*, 200 | 300
300, 350*, 400, 500 | 400
600, 700 | 500
800, 900 | 600

- *only applies to theme colors
- †only applies to green and red

#### Black

Stacks v1 | Stacks v2
:-:|:-:
N/A | 050
025 | 100
050 | 150
075 | 200
100 | 225
150 | 250
200 | 300
300, 350 | 350
400, 500 | 400
600, 700 | 500
750, 800, 900 | 600

#### Gold, sliver, bronze

Stacks v1 | Stacks v2
:-:|:-:
[color]-lighter | 100
N/A | 200
[color] | 300
[color]-darker | 400

#### Regular expression patterns

Below is just one example of a regular expression that can help you find and replace color references. Swap the color names and stop suffixes to update different remappings.

###### Find
```regexp
(theme-primary|theme-secondary|orange|blue|green|red|yellow)-(100|150|200)
```

###### Replace
```regexp
$1-300
```

### Deprecation of `powder` color set

Stacks v2 no longer includes the `powder` color set. We recommend you replace all references to `powder` with `blue`. If you need to continuing referencing `powder`, you can use the `-legacy` suffix until the library permanently removes legacy colors. See the [using legacy colors](#using-legacy-colors) section for more information.

### Referencing theme colors without the stop suffix

We've removed the `-color` suffix from the default theme variable custom properties. Previously, you could reference the 400 stop of theme colors with `theme-(primary|secondary)-color`. Now, you should reference the 400 stop of theme colors with `theme-(primary|secondary)`. Below is a table showing the old and new custom property names, though this pattern also applies to color, background, and border utility classes.

Variable | Stacks v1 | Stacks v2
:-:|:-:|:-:
**Primary** | `--theme-primary-color` | `--theme-primary`
**Secondary** | `--theme-secondary-color` | `--theme-secondary`

## Deprecation of Less color variables

Stacks v2 deprecates the usage of Less variables outside of Stacks. Although there's no way to make these variables truly private, we now discourage referencing them directly unless otherwise stated in our official documentation and advise that changes to psuedo-private variables may break your code if you reference them.

### Replacing Less color variables

Stacks v2 includes new CSS custom properties to replace a handful of commonly referenced Less variables. Update your Less variable references to the new CSS custom properties where possible.

Stacks v1 | Stacks v2
:-:|:-:
`@black` | `var(--_black-static)`
`@white` | `var(--_white-static)`

### Referencing Less color variables

In rare circumstances, you may need to reference a color value directly in order to operate on it and generate a new color or extract some value. In these cases, you *can* reference colors within a given color set. We advise against this and don't guarantee that these variables will remain stable. Nevertheless, here's how you would reference a Less color variable in Stacks v2.

Stacks v1 | Stacks v2
:-:|:-:
`@black-350` | `.set-black()[350]`

## Using legacy colors

Legacy colors are deprecated and will be removed January 2024. We **strongly** recommend you migrate to the new colors as soon as possible. With that said, you may have a need to reference legacy colors in the short term. This section gives an overview of how to reference legacy colors.

### Using the `-legacy` suffix

Stacks v2 replaces v1 colors, but you can still reference the v1 colors by adding the `-legacy` label to your color references. The `-legacy` suffix will generally come *after* the color name and *before* a color stop suffix (if it exists). This applies to atomic color classes (such as `.bc-*`, `.bg-*`, and `.fc-*`), CSS custom properties, and Less variables. See the [legacy color documentation](https://stackoverflow.design/product/v1/base/colors/) for more information.

### Examples of legacy color references

Stacks v1 | Stacks v2
:-:|:-:
`.bc-red-800` | `.bc-red-legacy-800`
`--theme-secondary-400` | `--theme-secondary-legacy-400`
`--theme-base-primary-color-h` | `--theme-base-primary-color-legacy-h`
`.fc-light` | `.fc-light-legacy`
`.fc-white__forced` | `.fc-white-legacy__forced`
`@black` | `@black-legacy`

### Helpful regular expressions

Below you'll find a set of regular expressions to help you find and replace existing color references with corresponding legacy reference. 

> **Note**
> Even with these regular expressions, you'll may still need to find and replace some color references manually. For example, this regex will not alter references to `black` that don't contain a numeric stop suffix.

#### Add `-legacy` suffix to colors

##### Find
```regexp
(theme-primary|theme-secondary|black|orange|blue|green|red|yellow|powder|fog)-(025|050|075|100|150|200|300|350|400|500|600|700|750|800|900)
```

##### Replace
```regexp
// Replace
$1-legacy-$2
```

#### Add `-legacy` to aliased values

##### Find
```regexp
(bc|fc|bg)-(lightest|lighter|light|medium|darker|dark)
```

##### Replace
```regexp
$1-$2-legacy
```

## Custom theme generation

Stacks v2 has deprecated the Less function `.generate-calculated-themed-variables()` in favor of the newly added `.create-custom-theme-hsl-variables()` function. This new function works differently behind the scenes, but it accepts the same arguments and generates the same CSS custom properties as its deprecated equivelent. See the [theming documentation](https://stackoverflow.design/product/foundation/theming/).

### Replacing `.generate-calculated-themed-variables()`

To update to the new custom theme variable generation function, simply search for `.generate-calculated-themed-variables` in your codebase and replace it with `.create-custom-theme-hsl-variables`.

## Deprecation of `.s-btn__primary`

Stacks v2 deprecates the `.s-btn__primary` variant. To retain a similar style, we recommend replacing all instances of `.s-btn__primary` with `.s-btn__filled`. See the [button documentation](https://stackoverflow.design/product/components/buttons/) for more information.
