CSS-Styleguide
==============

The purpose of this document is to outline a collection of opinionated
best-practices and methodologies for building object-oriented CSS archetectures
for Drupal builds that are highly scalable and easily maintained. 

## Table of Contents

1. [General principles](#General Principles)
  a. [Whitespace](#Whitespac and Comments) 
  b. [Format](#Format)
  c. [Exceptions](#Exceptions)
2. [Archetecture](#Archetecture)
  a. [Object Oriented CSS (OOCSS)](#Object Oriented CSS)
  b. [Naming Conventions and Structure](#Naming Conventions and Structure)
  c. [Selector Construct and Specificity](#Selector Construct and Specificity)
3. [Progressive Enhancement](#Progressive Enhancement)
4. [Practical example](#Practical Example)
5. [Works Cited](#Works Cited)
6. [License](#license)


<a name="General Principles"></a>
# General Principles

<a name="Whitespace and Comments"></a>
## Whitespace and Comments
* Remain consistent with your use of whitespce for greater legability of your
  code. Never mix spaces and tabs to adjust indentation.
* Use soft-tabs with a two space indent. This is in accordence with Drupal's
  coding standard [DrupalCC](http://drupal.org/coding-standards#indenting). To
  make this easy, this is set within your [Vim
  config](https://github.com/kwaledesign/dotfiles) (or equivelant).
* The key to portable code is in carefully documenting each components purpose,
  how it works, and if necessary its intended markup pattern. 
* Place comments on their own line directly above the code they document.
* Limit line length the 80 characters (again via
  [DrupalCC](http://drupal.org/coding-standards#linelength))
* When ever necessary, breakup long code blocks into discrete sections
* Avoid adding end-of-line whitespace. Again, configure your IDE to make this
  easy.

Example:

```css
/* ==========================================================================
   Section comment block
   ==========================================================================
*/

/* Sub-section comment block
   ========================================================================== */

/**
* Short description using Doxygen-style comment format
*
* The first sentence of the long description starts here and continues on this
* line for a while finally concluding here at the end of this paragraph.
*
* The long description is ideal for more detailed explanations and
* documentation. It can include example HTML, URLs, or any other information
* that is deemed necessary or useful.
*
* @tag This is a tag named 'tag'
*
* @todo This is a todo statement that describes an atomic task to be completed
*  at a later date. It wraps after 80 characters and following lines are
*  indented by 2 spaces.
*/

/* Basic comment */
`````

* Object-Extension Pointer - when extending a module within a separate
  stylesheet, leave a comment pointing to the original base object in order to
  establish a concrete link between the object and its extension.[13] 

Example:
```
/**
 * Extension of the .foo object in _bar.scss
 */
 .foo__bar {...}
```

<a name="Formating"></a>
## Formating
* Use one seletor per line in multi-selector rulesets.
* Use one space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each delaration.
* Use one space after the colon of a declaration.
* Use lower-case, shorthand hex color codes (ex: #000), or rgba().
* Use double quotes for quoted attribute vaules in selectors; ex:
  'input[type="checkbox"]'
* _Where allowed_, avoid specifying units for zero-values ex: 'margin: 0;'.
* Follow every comma with a space for comma-seperated property or function
  values.
* Include a semi-colon at the end of the last declaration in a declaration
  block.
* Place the closing brace of a rulset in the same column as the first charater
  of the ruleset.
* Seperate each ruleset with a blank line.
* Strictly adhear to consistent use of naming conventions to facilitate easy use of
  `ack` and/or `grep`.
* Declaration Order - CSS rulesets should be grouped by function - positioning
  rules, box-model rules, skin rules. 
* Units
  * Either `em's`, or `px's` may be used for `font-size`. Both allow the user
    to change the default font size within their browser, which is what we
    want. Also effective is to use a Sass mixin to define `font-size` in `rems`
    with a fall-back to `px's` for browsers lacking support.
  * Use unit-less 'line-height' because it does not inherit a percentage value
    of its parent element - it's based on a mulitiplier of the 'font-size'.
  * For all other measures try to use 'em''s, to best create a responsive
    system that scales to it's screen size.
  * Avoid absolute measurements.  For example, by using `.dropdown-nav li:hover
    { top: 37px; }` you are coding a single point of failure into your
    interface. Instead, build with flexibility in mind by using `.dropdown-nav
    li:hover { top: 100%; }`[13].
  * Use [Modular
    Scales](http://www.alistapart.com/articles/more-meaningful-typography/) to
    help define your proportional rhythms within your designs. The [Sassy
    Modular Scale](https://github.com/scottkellum/modular-scale) Ruby Gem makes
    working with Modular Scales easy. 
>"By using culturally relevant historically pleasing ratios to create modular
>scales and basing the measurements in our compositions on values from those
>scaleswe can achieve a visual harmony not found in layouts that use
>arbitraryconventionalor easily divisible numbers." - [Tim
>Brown](http://www.alistapart.com/articles/more-meaningful-typography/)

<a name="Exceptions"></a>
### Exceptions
Large blocks of single declarations can use a slightly different single-line
format. In this casea space should be included after the opening brace and
before the closing brace.

Example:
```css
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```
Long comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs.

Example:
```css
.selector {
  background-image:
    linear-gradient(#fff#ccc),
    linear-gradient(#f3c#4ec);
  box-shadow:
    1px 1px 1px #000,
    2px 2px 1px 1px #ccc inset;
}
```

### Preprocessors
* Use Sass. 
* Use Compass CSS3 vendor prefixes, vertical rhythm, and sprites.
* Only use nesting when you intend for nested CSS selectors.
* Never mimick HTML structure of a module with SCSS nesting.
* Leave your generated CSS file uncompressed until production and monitor it
  closely to ensure you aren't introducing bloated code or unintinsional
  nesting issues.
* Limit nesting to two or three levels deep. Reassess any nesting more than two
  levels deep. This prevents overly-specific CSS selectors.
* Avoid large numbers of nested rules. Break them up when readability starts to
  be affected.
* Always place `@extend` statements on the first lines of a declaration block.
* Group `@include` statements at the top of a declaration blockafter any
  `@extend` statements.
* Prefix custom functions to avoid collisions with Sass or Compass.
* Use a variable for all color values to allow for reuse and consistentcy.
* Consider using a variable for other common vaules.

Example:
```scss
.selector-1 {
  @extend .other-rule;
  @include clearfix();
  @include box-sizing(border-box);
  width: x-grid-unit(1);
  // other declarations
}
```

<a name="Architecture"></a>
# Architecture

<a name="Object Oriented CSS"></a>
## Object Oriented CSS (OOCSS)
>"[A] CSS “object” is a repeating visual patternwhich can be abstracted into an
>independent snippet of HTML CSS and possibly JavaScript. Once created an object
>can then be reused throughout a site." - [Nichole
>Sullivan](https://github.com/stubbornella/oocss/wiki)

### Two Main Principles of OOCSS
1. Seperation of Structure from Skin - distinquish between structure styles
   (box-model) and skin styles (color, font, gradients) and abstract these
   styles inot class-based modules to allow re-use[6].
2. Seperation of Container and Conent - avoid all explicit parent-child
   relationship within style declarations so that a module's style is not
   dependant upon its container which allows the module to be reused. 

### Additional Object Oriented Principles
* **Single Responsibility Principle** - an object should have only a single
  responsibility, and that responsibility should be entirely encapsulated by
  the object[7][8].
* **Open/Closed Principle** - entities (classes, modules, functions, etc.) are open
  for extension, but closed for modification. Base rules may be extended, but
  not directly modifiablei. This is why directly styling HTML tags is not
  advisable - reduce the amount of gobal element styles in order to reduce the
  chances of breaking the open/closed principle[7][9].
* **Liskov Substitution Principle** - objects should be replaceable with instances
  or their sub-components without breaking. Sub-components that `@extend`
  a module should be interchangable with the base module itself. To keep true
  to this principle, a module's subcomponent(s) should never affect layout. For
  example: `.m-btn__primary--sign-up` should be interchangable with `.m-btn`
  [7].
* **Entity Segregation Principle** - If ever it becomes awkward to interchange
  a subcomponent with its base module, or if it becomes necessary to redefine
  too many properties, move the subcomponent into its own custom module. It is
  sometimes better to have multiple base modules oppossed to a single generic
  module with multiple sub-components. Never make sacrifices in functionality
  in the name of utility[7]. Taken too far, code abstaction becomes
  detrimental. When writing modular CSS, it's not about maintaining modularity
  in the actual code, but rather modularity in the actual design[10].
* **DRY** - (don't repeat yourself)

<a name="Naming Conventions and Structure"></a>
## Naming Conventions and Structure
An effective naming convention explicitly communicates the context and function of the
entity being named. 

### Module

Module
: a discrete component of the page, i.e. navigation bar, carousel, dialog,
widget, etc. that sits inside of a layout component, or within another module.
each module should be designed to exist as a stand-alone compoenent, it should
not have any dependancies to its container, and it should be able to be
relocated on the page without breaking[1].
* An independant entity that can be simple or compound (containing another
  block or multiple blocks)
* Must remain independant from siblings, children, and parents allowing for
  arbitrarily placement on the page. 
* Block name must be unique to project - only instances of same block can have
  same name.
* Re-using a block also means re-using its behavior. To use the same block with
  differing behavior requiers a modifier or a submodule.
* Never use HTML elements within css selectors or cascading selectors for
  multiple blocks - selectors must be context free.
* Block names must be consistient accross all programming languages necessary
  to impliment its view and functionality (same block name for CSS and JS)
* Avoid CSS ID seletors - blocks must remain-nonunique, able to appear multiple
  times on the page.
* JS - use data-attributes - blocks with similar behavior can be unequivically
  detected to apply the requiered dynamic behavior.[2]
* Module styles are indicated with the 'm-' prefix. Module styles should not
  declare any explicit size constraints, allowing the module to scale to it's
  parent container. When needed, explicit constraints may be applied to
  a module via layout style.

#### Element

Module Element
: a component of a module that performs a specific function

* Elements are context-dependant (only make sense within their parent block)
* Element name must be unique within the scope of its block
* Elements can be repeated within a block e.g. tabs or navigation elements
* Must have a unique name to be used within a css rule
* An element's class name includes its block-name and element-name to maintain
  the elements context, maintain control of the cascade, and avoid
  location-dependant selectors.[2]

#### Submodule

Submodule
: a variant of a module that inherits all the styles of its parent, but differs
in skin, layout, positioning, etc. By creating a subcomponent within an
existing module, modularity is maintained by avoiding location dependant
styles[1].

* A module's submodules are child elements of the module and are prefixed
  with the full name of the module followed by `__` (double underscores). This
  clearly indicates a submodule's relationship to its module and also
  prevents the submodule's styles from applying outside of the module's
  scope.

Submodule Component
: an element of a submodule

#### Modifier

Modifier
: a trivial variant of a module or submodule applied as an additional class on
the root module, e.ge `&.modifier`. Useful for when the module or submodule
modification is insignificant enough not to warrant an entirely new module or
submodule.

* Distinguishes an alteration in style or application without creating an
  entier new block
* A property of a block or an element that alters its look or behavior.
* Multiple modifiers may be used at once.
* Most common example of a modifier is the Module state e.g. .is-active,
  .is-collapsed, .is-disabled.
* A modifier is an aditional css class for a block or element. This clearly
  indicates a modifiers relationship to its module or subcomponent and also
  prevents the modifiers styles from applying outside of its scope.
* Element and block modifiers are applied in the same way.
* Modifiers are chainable, but doing so is good indicator of re-evaluating the
  necessity for moving those styles into their own module or submodule.

#### State

State
: a state is a type of module modifier that is triggered by an action

* State based styles are indicated with the 'is-' prefix. These style
  declarations can be shared by CSS and JS files[1]

#### Components

Module Component
: a general term that refers to a module's children - can be an element,
a submodule, a submodule's element, or another module. Module components make
up a module. 

Submodule Component
: a general term that refers to a submodule's children - can be an element,
another submodule or module. Submodule components makeup a module.

#### Component Modifiers
Use a multi-class pattern[3] for compoenent modifiers in order to reduce the
number of classes for a component when variations are scaled, to allow for easier
contectual based adjustments when necessary, and to help simplify class and
variable names. For example, state styles are extended via their own class,
rather than attaching a state-suffix to an existing module class.


### Layout
* All modules are fluid by their nature. They should never be given explicit
  width or height restraints.
* Heights should only be explicitly defined for elements which had dimensions
  before they entered the site; e.g. image, video. In all other cases use
  line-height instead which is far more flexible [13].
* A module's width is determined by its parent container in the case of
  compound modules, or the grid system.
* The grid system should never have styles or box-model properties directly
  applied - grid items contain content, but are not content in themselves.
* Layout based styles are indicated with the 'l-' prefix in order to
  distinguish them from module or state styles[1].
* Minor page components (modules) sit within majar components such as the
  header, or footer. Major page components are refered to as Layout styles

### Icons
* Icons do not belong in modules - By styling icons independently of the module
  where it was first used you are making an icon that can be used in future
  modules without the need for duplication of code.
* Split icon styles into ico-size & ico-img classes - Specifying icon size and
  image separately allows for maximum flexibility and minimal code
  repetition[1]
* Use Compass to manage sprits easily.
* Sprited Icons should be added to empty elements, to elements that have their
  text hidden off canvus
  
<a name="Selector Construct and Specificity"></a>
## Selector Construct and Specificity
>"The problem with such a depth is that it enforces a much greater dependency
>on a particular HTML structure. Components on the page can’t be easily moved
>around" - [Jonathan Snook](http://smacss.com/book/applicability)

 Minimize "depth of applicability" in order to avoid over-reliance on
 a predefined HTML structure and hindering modularity and flexibility of
 modules. This also helps to prevent introducing potential specificity issues
 which are notoriously difficult to debug. When selectors are kept succinct, it
 also becomes easier to convert modules into templates for dynamic content.[1]

### Naming Pattern
* Classes used as JavaScript hooks are indicated with the 'js-' prefix.
  A 'js-' prefixed class should never be referenced in a stylesheet. They used
  exclusively from JS files[3]

(or...use html5 custom data-attributes for js hooks!!??!!)
* Animate an interface using classes not inline styles Inline styles added by
  javascript are harder to update and maintainprefer to add classes using
  javascript.  CSS3 transitions can then handle any animations and if CSS3
  transitions are not supported the state will still be updated.  Source:
  [SMACSS on state](http://smacss.com/book/type-state)

**Pattern**
```
prefix-module-name
prefix-module-name.modifer-name
prefix-module-name--element-name
prefix-module-name--element-name.modifier-name
prefix-module-name__submodule-name
prefix-module-name__submodule-name.modifier-name
prefix-module-name__submodule-name--element-name

.is-state-name
.js-behavior-name

```
Example:
```
.m-current-events {...}
.m-current-events.is-collapsed {...}
.m-current-events--header {...}
.m-current-events--header.no-border {...}
.m-current-events__featured-story {...}
.m-current-events__featured-story--pull-quote {...}
.m-current-events__featured-story--pull-quote.no-border {...}
``` 
**Note:** _while this BEM[2] like syntax is fairly complex, it explicitly
communicates the function and conetext of the entity, as well as its
relationship to both child and parent components while avoiding deeply nested
selectors that tie content to container and make assumptions about markup.
This of course would make our code less modular. Here, we sacrifice simple
selectors in order to preserve our objected oriented principles. This is
a solid trade off when you take into account [IDE
Snippets](https://github.com/kwaledesign/SCSS-Snippets) and the fact that GZIP
handles repetition very well._

### CSS Class Semantics
Class names should remain content-independant[3]. By avoiding tightly coupled
class names and content semantics, code is more easily reused and modularized
to allow for increased scalability of your archetecture.  Because the most
reusable code components are those with content-independant class names, class
names should be derived from repeating structural or functional patterns. 

>"We shouldn't be afraid of making the connections between layers clear and
>explicit rather than having class names rigidly reflect specific content.
>Doing this doesn't make classes "unsemantic", it just means that their
>semantics are not derived from the content." - [Nicolas
>Gallager](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)

This notion is not necessarily in conflict with the goals of the SMACSS
goals of 
1. Increasing semantic value of a section of html and content, and 
2. Decrease the expectation of a specific HTML structure.

The distinction is that the HTML class attributes are semantic in the way they
convey meaning to the developer, rather than the content. Content receives it's
semantic value from its markup (HTML tags and ARIA attributes). Code receives
its semantic value from its classes.

The goal, of course is to maximize modularity of code, creating scalable CSS
archetyecture.

#### Specificity Guidelines

* Avoid using ID selectors within CSS selectors. [Sin and
  Syntax](http://www.kwaledesign.com/articles/sin-and-syntax)
* Do not use location based selectors. A location based selector is a selector
  that changes a modules appearance based on its location or region
  (main-content, side-bar, footer, etc).  Where a module has different
  appearances use a module subclass.  If the appearance and/or content is very
  different it would be better to use a different module[17].
* Always namespace state class names e.g. `.disabled`, `.mousedown`, `.hover`,
  `.selected`, and `.collapsed`. ex: `.button.is-disabled`, never explicitly
  style `.is-disabled`.
* Minimise the use of element selectors - Selectors that contain elements
  tightly coupled the CSS to specific markup. It is not a safe assumption that
  the semantics of the content will never change so authors should prefer
  classes which exist independent of markup and create more flexible CSS.[1]
* !important should be avoided as much as possible. State classes are the only
  acceptable use of important[1].
* Never qualify a selector with an element selector e.g. `ul.nav`, as this
  decreases selector performance, creates a context dependency, and increases
  the selector's specificity. These are all things to be avoided.[1][12]

<a name="Progressive Enhancement"></a>
## Progressive Enhancement
* Develope for _Mobile First_[14] as a baseline level of support for small
  screens and slow connections, emphasize content over navigation, and to maintain
  clear and focused User Experience goals. Then use modernizr, media queries,
  and conditional loading to provide an enhanced experience for more capable
  devices.
* Use browser prefixes correctly & Painlessly - When using prefixed properties
  ensure that MicrosoftOperaWebkit and Mozilla prefixes are supported and that
  the non prefixed version is too.  Make this less painful by abstracting it
  away into a preprocessor mixin.
* Use Conditional classes for Internet Explorer - Aim to avoid separate styles
  for internet explorerbut where unavoidable do not use conditional stylesheets
  or CSS hacks to target specific versions of IE.  Instead use conditional
  classes as explained by Paul Irish.  
  Source: [Conditional stylesheets vs CSS
    hacks…](http://paulirish.com/2008/conditional-stylesheets-vs-css-hacks-answer-neither/)
* Group IE Conditional styling with the code they alter - Aim to avoid separate
  styles for internet explorerbut where unavoidable place the "fixes" next to
  the style it effects.  Not doing so makes it easy to miss when updating to
  changing the original styles
* Normalise Don't Reset - A global reset is a convenient way of ensuring
  consistent styling cross browser but it is often overkill and it makes CSS
  harder to debug in developer tools.  Source:
  [Normalise.css](http://necolas.github.com/normalize.css/)
* Don't be over-zealous when normalising - When normalising it is important to
  not add styles to elements that then need to be overridden in future modules.
  Tables and form elements are great examples of this.  Source: [SMACSS on drop
  the base](http://smacss.com/book/drop-the-base)


<a name="Practical Example"></a>
## Practical example

An example of various conventions.

```css
/* ==========================================================================
   Grid layout
   ========================================================================== */

/**
* Column layout with horizontal scroll.
*
* This creates a single row of full-heightnon-wrapping columns that can
* be browsed horizontally within their parent.
*
* Example HTML:
*
* <div class="grid">
*   <div class="cell cell-3"></div>
*   <div class="cell cell-3"></div>
*   <div class="cell cell-3"></div>
* </div>
*/

/**
* Grid container
* Must only contain `.cell` children.
*/

.grid {
  height: 100%;
  /* Remove inter-cell whitespace */
  font-size: 0;
  /* Prevent inline-block cells wrapping */
  white-space: nowrap;
}

/**
* Grid cells
* No explicit width by default. Extend with `.cell-n` classes.
*/

.cell {
  position: relative;
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  height: 100%;
  /* Set the inter-cell spacing */
  padding: 0 10px;
  border: 2px solid #333; 
  vertical-align: top;
  /* Reset white-space */
  white-space: normal;
  /* Reset font-size */
  font-size: 16px;
}

/* Cell states */

.cell.is-animating {
  background-color: #fffdec;
}

/* Cell dimensions
   ==========================================================================
*/

.cell-1 { width: 10%; }
.cell-2 { width: 20%; }
.cell-3 { width: 30%; }
.cell-4 { width: 40%; }
.cell-5 { width: 50%; }

/* Cell modifiers
   ========================================================================== */

.cell--detail,
.cell--important {
  border-width: 4px;
}
```

<a name="Works Cited"></a>
### Works Cited:
1. [SMACSS](http://smacss.com/)
2. [BEM](http://bem.info/)
3. [About HTML Semantics and Front-End
   Archetecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)
4. [Principles of Writing Consistent, Idiomatic
   CSS](https://github.com/necolas/idiomatic-css)
5. [Github - CSS Styleguide](https://github.com/styleguide/css)a
6. [Object Oriented CSS](https://github.com/stubbornella/oocss/wiki)
7. [SOLID CSS](http://blog.millermedeiros.com/solid-css/)
8. [The Single Responsibilty Principle Applied to
   CSS](http://csswizardry.com/2012/04/the-single-responsibility-principle-applied-to-css/)
9. [The Open/Closed Principle Applied to
   CSS](http://csswizardry.com/2012/06/the-open-closed-principle-applied-to-css/)
10. [One Module or
    Two](http://snook.ca/archives/html_and_css/one-module-or-two)
11. [Our CSS Best Practices are Killing
    Us](http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/)
12. [Quasi-qualified CSS
    Selectors](http://csswizardry.com/2012/07/quasi-qualified-css-selectors/)
13. [CSSWizardry CSS Guidelines](https://github.com/csswizardry/CSS-Guidelines)
14. [Organizing Mobile - Luke
    Wroblewski](http://www.alistapart.com/articles/organizing-mobile/) 
15. [Component-Based HTML/CSS Naming Patterns](https://gist.github.com/1309546)
16. [OOCSS](http://www.slideshare.net/stubbornella/object-oriented-css)


<a name="license"></a>
### License:

CSS-Styleguide is licensed under the terms of the MIT
[license](https://github.com/kwaledesign/CSS-Styleguide/blob/master/LICENSE).
