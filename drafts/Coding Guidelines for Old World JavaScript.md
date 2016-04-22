# JavaScript Afspraken

1. [Style](#style) 
2. [Patterns](#patterns)
3. [Testing](#testing)
4. [Performance](#performance)
5. [Libraries](#libraries)
6. [Resources](#resources)

Our goal is to write JavaScript that does what it's supposed to, is easily understood and maintained, is fast and has test good coverage.

The following guidelines apply to all new code and should also be applied when possible during the re-writing of old code.


## <a name="style">&nbsp;</a>Style

- Indent with 4 spaces 
- DRY - avoid repition
- Apply the single responsibility principle (each piece of code should have one responsibility - no 100 line functions)
- use 'use strict';
- Declare variables at the top of the function (as much as possible)
- Avoid complex nested if/then logic
- Test equality with === instead of == (as much as possible. See next point for more details)
- Read [Type checking, coerced types and conditional evaluation](type-coercion-conditional.html 'Type checking, coerced types and conditional evaluation')
- Write useful comments to clarify intent and use. Documentation is generated via comments. ([example](../docco/dropdown.html))
- Use [JSHint](http://www.jshint.com/ 'JSHint') (&nbsp;*Zie ook: [Integrating JSHint into Visual Studio](http://blog.stevensanderson.com/2012/08/17/using-jshint-inside-visual-studio-the-basics/ 'Integrating JSHint into Visual Studio')&nbsp;*)
 
JSHint settings (for existing code):

	/*jshint forin:true, noarg:true, noempty:true, eqeqeq:true, bitwise:true, strict:true, undef:true, unused:true, curly:true, browser:true, jquery:true, indent:4, maxerr:50, */
	/*global Funda*/ 
	
For new code:
    
    /*jshint forin:true, noarg:true, noempty:true, eqeqeq:true, bitwise:true, strict:true, undef:true, unused:true, curly:true, browser:true, jquery:true, indent:4, maxerr:50, maxcomplexity: 5, maxstatements: 5, maxparams: 3, maxdepth: 2, immed: true, indent: 4, latedef:true, newcap:true, es5:true*/
	/*global Funda*/ 

## Patterns<a name="patterns">&nbsp;</a>
- Keep you code modular and enclosed in a module format. Not only is this a good best practice, while we don't currently use an AMD loader and ECMAScript Harmony modules are not yet practical for our use, but by keeping our code in a module format, we can gain the benefits and the capacity to make the switch with minimal effort. ([module-template](js-patterns.html))
- Modularize code and enclose each module in an anonymous self executing function [template](js-patterns.html#basic-module)
- Know your design patterns (see [resources](#resources))
- Use a template. Our most frequently used templates are here:<br/>([self-revealing module](js-patterns.html#revealing-module), [prototypal inheritance module](js-patterns.html#prototype-module), [jquery plugin module](js-patterns.html#prototype-module))
- Add code to the Funda namespace if you need to refer to it elsewhere.
- Anonymously scope JavaScript if you’re never going to call it elsewhere (think about inline JavaScript).



## Testing<a name="testing">&nbsp;</a>

- New functionality should be tested with Jasmine (*&nbsp;[Docs](http://pivotal.github.com/jasmine/ 'Jasmine Docs') en [Wiki](https://github.com/pivotal/jasmine/wiki 'Jasmine Wiki')&nbsp;*)
- When possible write tests when refactoring / modifying existing functionality.

## Performance<a name="performance">&nbsp;</a>

- Cache DOM elements as much as possible

  	var $el = $('.my-form'),
  	    $li = $el.find('li');
  	$el.doStuff();
  	$li.doStuff();

- Manipulate the DOM as little as possible (reduces page repaint) (for example: make use of documentFragment)
- Be careful with events that are frequently executed (ie. resizing / scrolling)


## Libraries<a name="libraries">&nbsp;</a>

- jQuery is great for handling the differences between browser implementation of js - in particular when working with the DOM. However many times, it's not needed at all. If it's not needed, avoid the use of jQuery.

<span style="display:none;">

- [Lodash](http://lodash.com/) voor utility functionaliteit
- [Backbone](backbonejs.org) voor MVC

</span>

## Resources<a name="resources">&nbsp;</a>
- **Templates in use at Funda**: [JavaScript Templates](js-patterns.html 'JavaScript Templates')
- **Best Practices**: *[JavaScript: the Good Parts](../resources/javascript-the-good-parts.pdf 'JavaScript: the Good Parts')*  ( *ebook download* )
- **Pragmatics**: *[Effective JavaScript](../resources/effective-javascript.pdf 'Effective JavaScript')*  ( *ebook  download* )
- [Type checking, coerced types and conditional evaluation](type-coercion-conditional.html 'Type checking, coerced types and conditional evaluation')
- **Design Patterns**: <br/>*[JavaScript Patterns](../resources/javascript-patterns.pdf 'JavaScript Patterns')* ( *recommended ebook but file not yet present* ) <br/> *Learning JavaScript Design Patterns* [web](http://www.addyosmani.com/resources/essentialjsdesignpatterns/book/) or [ebook](../resources/learning-js-patterns.zip 'Learning JS Design Patterns')

- **DOM / Browser reference**: *[Professional Javascript for web developers](../resources/professional-javascript.pdf 'Professional JavaScript for web developers')*  ( *ebook: file not yet present* )
- **Performance**: *[High Performance JavaScript](../resources/high-performance-javascript.pdf 'High Performance JavaScript')*  ( *ebook: file not yet present* )
- **[JS Assessment](https://github.com/rmurphey/js-assessment 'JS Assessment')**: A test-driven approach to assessing JS skills.