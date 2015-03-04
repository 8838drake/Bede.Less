# Bede.Less
### Table of Contents
**[File Naming](#file-naming)**  
**[Formatting](#formatting)**  
**[- Unitless Zeroes](#unitless-zeroes)**  
**[- Semicolons](#semicolons)**  
**[- Quotations](#quotes)**  
**[- Nesting](#nesting)**  
**[- Colours](#colours)**  
**[- Pseudo Elements](#pseudo-elements)**  
**[Selectors](#selectors)**  
**[Naming Conventions](#naming-conventions)**  
**[JS Prefixes](#js-prefixes)**  
**[Use of IDs](#use-of-ids)**  
**[Specificity](#specificity)**  
**[Mixins](#mixins)**  
**[- Mixin Presentation](#mixin-presentation)**  
**[- Mixin Use](#mixin-use)**  
**[Media Queries](#media-query-usage)**

##File Naming
File Names should be camelcased and, if necessary, be preceded by platform and file type which `should be separated with a hyphen.

#####Good
```
paymentWidget.less
paymentWidget-mobile.less
paymentWidget-variables.less
paymentWidget-mobile-variables.less
{{component}}-{{platform}}-{{fileType}}.less
```

#####Bad
```
payment-widget.less
payment-widget-mobile.less
paymentWidgetMobile.less
payment-widget-mobile-variables.less
paymentWidgetMobileVariables.less
```

#####Why?
Following a file naming convention can make finding files more efficient and easier to maintain.

##Formatting

###Unitless Zeroes
Units are not allowed on zero values.

#####Good
```less
margin: 0 10px 5px;
```

#####Bad
```less
margin: 0px 10px 0em;
```

#####Why?
Zero values do not require a unit to be present as 0px, 0em and 0rem are all equal; removing the unit creates more readable code.

###Semicolons
All properties should precede a semicolon.

#####Good
```less
.payment-widget {
	margin: 10px;
	color: #000;
}
```

#####Bad
```less
.payment-widget {
	margin: 10px;
	color: #000
}
```

#####Why?
Semicolons in LESS should always be used. Ending each property value with a semicolon will avoid compile errors when editing the file in future.

###Quotes
Single quotations should be used instead of double quotations.

#####Good
```less
content: '';
```

#####Bad
```less
content: "";
```

#####Why?
Although there is no benefit to using either quotation apart from consistency, single quotations should be used in LESS and Javascript leaving double quotations for HTML files.

###Nesting
Nested selectors, if following CSS properties, should be separated with a new line, otherwise should immediately follow the previous selector.  
*Note, you should only nest selectors if you need to overwrite specificity or give an element a high specificity rating.*

#####Good
```less
.payment-widget {
	.promotion-container {
		width: 100%;
		position: relative;
	}

	.form-control {
		border: 1px solid #ddd;
	}
}

.promotion-box {
	float: left;
	display: block;

	.promotion-item {
		color: #444;

		&:nth-child(odd) {
			color: #ccc;
		}
	}
}
```

#####Bad
```less
.payment-widget {

	.promotion-container {
		width: 100%;
		position: relative;
	}
	.form-control {
		border: 1px solid #ddd;
	}
}

.promotion-box {
	float: left;
	display: block;
	.promotion-item {
		color: #444;
		&:nth-child(odd) {
			color: #ccc;
		}
	}
}
```

#####Why?
Separating new selectors with a space creates more readable code and clearly shows a new selector has been created.

###Colours
Colours should always use HEX value, if an RGBA value is needed it should make use of the [Less Fade](http://lesscss.org/functions/#color-operations-fade) function.

#####Good
```less
@cyan = #31FFFF;
color: fade(@cyan, 20%);
background: fade(#000, 10%);
```

#####Bad
```less
color: rgba(49, 255, 255, 0.2);
background: rgba(0, 0, 0, 0.1);
```

#####Why?
Hex values and LESS variables should be used to create transparent colours to create consistency and improve readability.

##Pseudo Elements
Pseudo elements such as `before`, `after` and `nth-child` should be separated with two colons and pseudo states should be separated with a single colon.

#####Good
```less
.payment-widget {
	&:hover {
		color: #ccc;
	}

	&::before {
		content: '';
	}
}
```

#####Bad
```less
.payment-widget {
	&:hover {
		color: #ccc;
	}

	&:before {
		content: '';
	}
}
```

#####Why?
Two colons are supported by all major browsers (IE8+) and create a greater difference between pseudo elements and pseudo states as well as making all pseudo elements easier to target in text editors.

##Selectors
Element tags `(p, h1, blockquote, ul)` should only be targeted if the applied style will affect all of those tags or is generated by the CMS. As a rule, we should add a class name when overwriting default styles.

#####Good
```less
ul {
	margin: 0;
}

.payment-form {
	.payment-list {
		margin: 10px;
	}
}
```

```html
<div class="payment-form">
  	<ul class="payment-list">
		<li class="payment-item">....</li>
	</ul>
</div>
```

#####Bad
```less
ul {
	margin: 0;
}

.payment-form {
	ul {
  		margin: 0;

  		li {
  			padding: 0;
  		}
	}
}
```

```html
<div class="payment-form">
	<ul>
		<li>....</li>
	</ul>
</div>
```

#####Why?
Browsers read CSS selectors from right to left. Meaning the following selector, `.widget li {}` would first search every single li in the DOM and then narrow it's search to the .widget class. Using a class selector over an element selector improves browser performance and readability.

##Naming Conventions
Class Names should always be hyphenated and IDs should always be camelcased.

#####Good
```less
#paymentWidget
.payment-widget
```

#####Bad
```less
#payment-widget
.paymentWidget
```

##JS Prefixes
Classes in HTML files that are used in Javascript should be prefixed with `js-`.

#####Good
```html
<div class="js-payment-widget payment-widget-container">
	...
</div>
```

```javascript
$('.js-payment-widget').addClass('active');
```

```less
.payment-widget {
	float: left;
}

.js-active {
	display: block;
}
```

#####Bad
```html
<div class="js-payment-widget">
	<div class="payment-group">
		...
	</div>
</div>
```

```javascript
$('.payment-group').addClass('active');
```

```less
.js-payment-widget {
	float: left;
}

.active {
	display: block;
}
```

#####Why?
When looking at a HTML file we should be able to easily and instantly identify which classes are used in LESS and which are used in Javascript. Using prefixes improve readability and become easier to update an existing project. Deleting a js prefixed class should have no negative implications to the appearance of a page and deleting an unprefixed class should have no negative implications to the Javascript on the page.

#####When adding a class in Javascript should it be js-prefixed?
No, the purpose of prefixing classes is to improve readability in HTML files. There is no need to create a prefixed class in Javascript unless it will be targeted later on.

##Use of IDs
IDs should not be used in LESS.

#####Why?
IDs are too specific and rigid; IDs should be reserved for Javascript and semantic use only. Classes can do everything IDs can do and can be utilised multiple times on a page. Although IDs are slightly faster in terms of performance, they are not fast enough to be beneficial and are actually worse than classes for performance when used to namespace elements [[1]](http://oli.jp/2011/ids/). Using an ID can also provide issues later on by creating overly specific code.

##Specificity
Selectors should be nested a maximum of 4 levels deep.

#####Why?
If two selectors are applied to the same element, the one with the highest specificity will win, regardless of it’s position in the stylesheet; because of this we should only be specific when absolutely necessary in order to avoid writing messy and unnecessary code; this is another reason why we do not use IDs in LESS, as they have a higher specificity rating [[2]](http://cssspecificity.com/).

When possible and viable, always use the lowest specificity possible.

##Mixins

###Mixin Presentation
Mixins should be declared at the top of the ruleset and the final mixin should have a break line before the first css property is declared.

#####Good
```less
.icon {
	.animation(spin 0.5s linear);
	.transition(0.3s all linear);

	position: absolute;
}
```

#####Bad
```less
.icon {
	.animation(spin 0.5s linear);
	position: absolute;
	.transition(0.3s all linear);
}
```

#####Why?
This clearly separates mixins and LESS properties making it easier to read and find mixins as well as being easier to overwrite certain properties within the called mixin.

###Mixin Use
Mixins should only use one argument and, if necessary, comma separated values can be passed through by escaping the mixin with a semi-colon before the closing bracket.

#####Good
```less
.fade {
	.transition(0.3s all linear);
}

.fade-scale {
	.transition(0.3s opacity linear, 0.3s transform linear;);
}
```

#####Bad
```less
.fade {
	.transition(0.3s, all, linear);
}
```

#####Why?
Passing multiple variables into a mixin when one can be used sets limitations to what we can do with the mixin and decreases readability.

##Media Query Usage
When possible media queries should be nested within the desired selector.

#####Good
```less
.payment-widget {
	padding: 15px;

	@media @screen-md {
		padding: 10px;
	}
}
```

#####Bad
```less
.payment-widget {
	padding: 15px;
}

...

...

@media @screen-md {
	.payment-widget {
		padding: 10px;
	}
}
```

#####Why?
We should be able to see if any media queries are used by looking at the relevant selector instead of looking at different parts of the file or even different files; this improves readability and knowledge of what code is attached to a selector.