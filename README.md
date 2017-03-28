# Sass / Css Styleguide

## Table of Contents
* [General](#general)
  - [Formatting](#formatting)
  - [Comments](#comments)
  - [BEM naming](#bem-naming)
      + [Block](#block)
      + [Element](#element)
      + [Modifier](#Modifier)
      + [Example of using the naming convention](#example-of-using-the-naming-convention)
  - [ID Selectors](#id-selectors)
  - [JavaScript hooks](#javascript-hooks)
* [Resets](#resets)
* [Sass](#sass)
  - [Folder structure](#folder-structure)
    + [Index file](#index-file)
    + [Blocks](#blocks)
    + [Helpers](#helpers)
    + [Vendor libs](#vendor-libs)
  - [Block structure](#block-structure)
* [Helpful Links](#helpful-links)


## General

**Sass vs Scss vs Css**
On this project we use indented Sass syntax that provides a more concise way of writing CSS. It uses indentation rather than brackets to indicate nesting of selectors, and newlines rather than semicolons to separate properties.

So we use this:
```sass
.block-name

  &:after
    // ...

  &_hover
    // ...
  
  &__element-name
    // ...
```

...instead of this:
```scss
.block-name {

  &:after {
    /* ... */
  }

  &_hover{
    /* ... */
  }
  
  &__element-name{
    /* ... */
  }
}
```

Some vendor libraries were imported in the Scss (Sassy CSS) format. Either syntax can import files written in the other.


### Formatting

Sass and CSS:
* Use soft tabs (2 spaces) for indentation
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* In properties, put a space after, but not before, the `:` character.
* Put blank lines between rule declarations

CSS:
* Put a space before the opening brace `{` in rule declarations
* Put closing braces `}` of rule declarations on a new line


### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks
* In markup write block start and block end comments for easy navigation

```html
<!-- Block-name start -->
<div class="block-name">
    <div class="block-name__element-name"></div>
</div>
<!-- Block-name end -->
```


### BEM naming

We use BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines.

* Names of BEM entities are written using **numbers** and **lowercase** **Latin characters**.
* Individual words within names are separated by a **hyphen** (`-`).
* Information about the names of blocks, elements, and modifiers is stored using **CSS classes**.


#### Block

`block-name` - defines a namespace for elements and modifiers.

**Example**

`menu`

`lang-switcher`

*HTML*

```html
<div class="menu">...</div>
```

*CSS*

```css
.menu { color: red; }
```


### Element

An element name is delimited by a double underscore (`__`) from a block name.
The full name of an element is created using this scheme:

`block-name__elem-name`

If a block has several identical elements, such as in the case of menu items, all of them will have the same name `menu__item`.

**Important!** [Using elements within elements is not recommended by the BEM methodology](../../faq/faq.en.md#why-does-bem-not-recommend-using-elements-within-elements-block__elem1__elem2).

**Example**

`menu__item`

`lang-switcher__lang-icon`

*HTML*

```html
<div class="menu"> ... <span class="menu__item"></span> </div>
```

*CSS*

```css
.menu__item { color: red; }
```


#### Modifier

On the current project most of modifiers are **Key-value type modifier**.

**Block modifier**
The value of a modifier is separated from its name by a single underscore (`_`). The full name is created using the scheme:
    `block-name_mod-name_mod-val`.

**Example**

`menu_theme_morning-forest`

*HTML*

```html
<div class="menu menu_state_hidden">...</div>
<div class="menu menu_theme_morning-forest">...</div>
```

> *Incorrect notation*
>
> ```html
> <div class="menu_state_hidden">...</div>
> ```
>
> Here the notation is missing the block that is affected by the modifier.

*CSS*

```css
.menu_state_hidden { display: none }
.menu_theme_morning-forest { color: green; }
```

**Element modifier**.
The value of a modifier is separated from its name by a single underscore (`_`). The full name is created using the scheme:
    `block-name__elem-name_mod-name_mod-val`.

**Example**

`menu__item_type_radio`

*HTML*

```html
<div class="menu"> ... <span class="menu__item menu__item_state_visible menu__item_type_radio"></span> </div>
```

*CSS*

``` css
.menu__item_type_radio { color: blue; }
```


#### Example of using the naming convention

The implementation of an authorization form in HTML and CSS:

*HTML*

```html
<form class="form form_role_login form_theme_forest">
    <input class="form__input">
    <input class="form__submit form__submit_state_disabled">
</form>
```

*CSS*

```css
.form {}
.form_theme_forest {}
.form_login {}
.form__input {}
.form__submit {}
.form__submit_state_disabled {}
```

*Sass*
```sass
.form
  &_theme_forest

  &_role_login

  &__input

  &__submit

    &_state_disabled
```


### ID Selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.


### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="button button__role_primary js-request-to-book">Request to Book</button>
```


## Resets

Some project pages use Foundation framework, so for now we can't use general `reset` or `normalize` stylesheet. 
For this purpose we have a few rough `sass` mixins that should be added to each block and some block's elements:

```sass
// Total reset of the inheritable properties
+normalize-block()

// Box model fix
+box-model-normalizer()

// Light version of the normalize-block for text elements
+text-normalizer()
```

On the refactoring stage these mixins will be removed in favor of `reset` stylesheet


## Sass

### Folder structure

```
sass/                             * sass preprocessor styles
   ├── blocks/                    * blocks library
   |   └── block-name.sass
   ├── helpers/                   * mixins, variables, base, fonts
   ├── vendor/                    * third-party code
```


#### Index file

#### Blocks

#### Helpers

#### Vendor libs

### Block structure

```sass
@import '../helpers/settings'
@import '../helpers/mixins'

////////////////////////////////////////
// $Block name
////////////////////////////////////////

//////////////////////////////////////// Block variables

$block-var-name: ""

//////////////////////////////////////// Block mixins and functions

=block-mixin-name()
  // ...


//////////////////////////////////////// Block styles
$block: ".block-name"

#{$block}
  // ...

  +mixin-name()

  // Media queries
  @media screen and (max-width: 960px)
    // ...

  &::before
    // ...

  &:hover
    // ...

    &:after
      // ...

  // Block-name STATE: active state
  &_state_active
    // ...

  // Block-name MOD: modifier name
  &_key_val
    // ...

    @media screen and (max-width: 960px)
      // ...

  // Block-name MOD: modifier name STATE: active state
  &_key_val#{&}_state_active
    // ...

    @media screen and (max-width: 960px)
      // ...


  // Block element name
  &__element
    // ...

    +mixin-name()

    // Block element name CONTEXT: block-name STATE: active state
    #{$block}_state_active &
      // ...

    // Block element name CONTEXT: block-name MOD: modifier name
    #{$block}_key_val &
      // ...

    @media screen and (max-width: 960px)
      // ...

      #{$block}_state_active &
        // ...

      #{$block}_key_val &
        // ...

    &::before
      // ...

    &:empty
      // ...

      &:after
        // ...

    // Block element name STATE: active state
    &_state_active
      // ...

    &_key_val
      // ...

      // Block element name MOD: modifier name CONTEXT: block-name STATE: active state
      #{$block}_state_active &
        // ...

      // Block element name MOD: modifier name CONTEXT: block-name MOD: modifier name
      #{$block}_key_val &
        // ...

      @media screen and (max-width: 960px)
        // ...

        #{$block}_state_active &
          // ...

        #{$block}_key_val &
          // ...


  // Other block element name
  &__other-element
    // ...
```


## Helpful Links
* [BEM naming convention](https://en.bem.info/methodology/naming-convention/)
* [BEM FAQ](https://en.bem.info/methodology/faq/)
