# Learn Sass

-   vscode extension: live sass compiler
-   two syntax: SCSS(popular) and Indented

## Compile with Live Sass Compiler

-   See in output
-   Watch mode: auto Compile at saving
-   only modify the scss file, not modify the css file.

## Variables

-   get rid of `:root`
-   Dollar sign: `$variable: #fff`
-   `color: $variable`
-   When Compiling into css, it is exactly values in css, not variables.
-   array variables

### Array Variables

map-get($map, $key)

```scss
$font-weights: (
    "regular ": 400,
    "medium": 500,
    "bold": 700,
);

body {
    background: $bgcolor;
    font-weight: map-get($font-weights, bold);
}
```

## Nest

Notice not to use too much nesting.

```scss
.main {
    width: 80%;
    margin: 0 auto;

    p {
        font-weight: map-get($font-weights, bold);
    }
}
```

### parent shorthand `&`

There is a class `main__paragraph`

#### Just `&`

```scss
.main {
    width: 80%;
    margin: 0 auto;

    &__paragraph {
        font-weight: map-get($font-weights, bold);
    }
}
```

> This compiled to:

```css
.main__paragraph {
    font-weight: 700;
}
```

#### `#{&}`

```scss
.main {
    width: 80%;
    margin: 0 auto;

    #{&}__paragraph {
        font-weight: map-get($font-weights, bold);
    }
}
```

> This compiled to:

```css
.main .main__paragraph {
    font-weight: 700;
}
```

## Partial Files

**With underscored file name**

An underscored filename of an scss file indicates that it is a **partial** file¹². A partial file is a Sass file that contains snippets of CSS that can be imported into other Sass files¹. The underscore tells Sass not to compile the file into a CSS file on its own, but only when it is imported by another file¹². This is a way to modularize your CSS and keep things easier to maintain¹. For example, you can have a partial file named \_variables.scss that contains variables for colors, fonts, etc. and import it into your main stylesheet with @import 'variables';¹. 😊

## `@import`

There is a file `_resets.scss` and `main.scss`.

Import:  
In `main.scss`;  
`@import './resets';`

## `@function`

```scss
@function weight($weight-name) {
    @return map-get($font-weights, $weight-name);
}
#{&}__paragraph {
    font-weight: weight(regular);
    &:hover {
        color: pink;
    }
}
```

## `@mixin` and `@include`

Use this on reusable blocks.

[W3School](https://www.w3schools.com/sass/sass_mixin_include.php)

**Tip:** A tip on hyphens and underscore in Sass: Hyphens and underscores are considered to be the same. This means that @mixin important-text { } and @mixin important_text { } are considered as the same mixin!

Example:

```scss
@mixin theme($light-theme: true) {
    @if $light-theme {
        background: lighten($primary-color, 100%);
        color: darken($text-color, 100%);
    }
}
```

### default parameter

```scss
@mixin theme($light-theme: true) {
    @if $light-theme {
        background: lighten($primary-color, 100%);
        color: darken($text-color, 100%);
    }
}
```

vs.

```scss
@mixin theme($light-theme) {
    @if $light-theme {
        background: lighten($primary-color, 100%);
        color: darken($text-color, 100%);
    }
}
```

The difference between these two Sass mixins is that the first one has a **default parameter** for `$light-theme`, while the second one does not. A default parameter is a value that is assigned to a parameter if no argument is passed to the mixin¹. For example, if you write `@include theme;` without specifying the value of `$light-theme`, the first mixin will assume that `$light-theme` is `true`, while the second mixin will throw an error. Default parameters are useful for providing fallback values or optional arguments to mixins¹.

¹: [Sass: Mixins](^1^)

Source: Conversation with Bing, 2/4/2024
(1) The Perfect Theme Switch Component | Aleksandr Hovhannisyan. https://www.aleksandrhovhannisyan.com/blog/the-perfect-theme-switch/.
(2) The Perfect Theme Switch Component | Aleksandr Hovhannisyan. https://www.aleksandrhovhannisyan.com/blog/the-perfect-theme-switch/.
(3) Angular - how to theme my own component using mixin?. https://stackoverflow.com/questions/59461568/angular-how-to-theme-my-own-component-using-mixin.
(4) Light and Dark Theme using Mixin - Medium. https://medium.com/ampersand-academy/light-and-dark-theme-using-mixin-98eb025032c9.
(5) css - Color themes with SASS mixins - Stack Overflow. https://stackoverflow.com/questions/23446965/color-themes-with-sass-mixins.

## functions vs. mixins

Functions and mixins are similar.

-   Function should be used on computing values and returning values
-   Mixins should used on setting properties

## Built-in functions

-   `lighten($text-color, 100%)`
-   `darken($tex-color, 100%)`

## `@media` and `@content`

```scss
@mixin theme($light-theme: true) {
    @if $light-theme {
        background: lighten($primary-color, 100%);
        color: darken($text-color, 100%);
    }
}

@mixin flexCenter($direction) {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: $direction;
}
@mixin mobile {
    @media (max-width: $mobile) {
        @content;
    }
}
.main {
    @include flexCenter(row);
    width: 80%;
    margin: 0 auto;
    #{&}__paragraph {
        font-weight: weight("bold");
        &:hover {
            color: pink;
        }
    }
    @include mobile {
        flex-direction: column;
    }
}

.light {
    @include theme($light-theme: true);
}
```

`@content` is what will be filled in `@include` clause

## Extend: `@extend`

```scss
#{&}__paragraph1 {
    font-weight: weight("bold");
    &:hover {
        color: pink;
    }
}
#{&}__paragraph2 {
    @extend .main_paragraph1;
    &:hover {
        color: $accent-color;
    }
}
```

## `calc()`

-   This is good: `width: 80% - 40%;`
-   This is not good: `width: 80% - 400px;`

Mixed unit calculate must use `calc()`
