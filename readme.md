# IAS Javascript Style Guide

## Table of Contents

 * [Whitespace](#whitespace)
 * [Beautiful Syntax](#spacing)
 * [Type Checking](#type)
 * [Conditional Evaluation](#cond)
 * [Practical Style](#practical)
 * [Naming](#naming)
 * [Misc](#misc)
 * [Native & Host Objects](#native)
 * [Comments](#comments)
 * [One Language Code](#language)

------------------------------------------------

## Preface

The benefit from using a style guide is consisteny. All code should be indistinguishable in style and appear as it was written by a single person. This improves readabilty and understanding while reducing time spent fixing bugs and maintaining the code.


## IAS Style Rules

1. <a name="whitespace">Whitespace</a>
  - indent using 2 spaces.
  - If your editor supports it, always work with the "show invisibles" setting turned on. The benefits of this practice are:
      - Enforced consistency
      - Eliminating end of line whitespace
      - Eliminating blank line whitespace
      - Commits and diffs that are easier to read
  - Check if your IDE has plugin support for [editorconfig](http://editorconfig.org/). (Intellij IDEA has built in support). It'll configure your IDE editor settings for 2 space indents.

2. <a name="spacing">Beautiful Syntax</a>

    A. Spacing, Parens, Braces, Linebreaks

     Parens in conditionals and loops. If, else, for, while, and try should always have spaces, braces and span multiple lines in order to encourage readability.
    
    ```javascript     
    // 2.A.1.1 - Spacing for conditionals and loops
    // eslint: "space-in-parens": ["error", "always"]

	// Bad
	if(condition) doSomething();

    while(condition) iterating++;

    for(var i=0;i<100;i++) someIterativeFn();

	// Good
    if ( condition ) {
      // statements
    }

    while ( condition ) {
      // statements
    }

    if ( true ) {
      // statements
    } else {
      // statements
    }
    ```
        
    Function calls and defintions get spaces around parens also. No space is needed for calls without params. Parens to change the order of operation get spaces as well.
    
    ```javascript
	// 2.A.1.2 - Parens in function calls.
    // eslint: "space-in-parens": ["error", "always"]
    
	// Good
    foo( 'bar' );
    foo();    
    
	var foo = ( 1 + 2 ) * 3;
	( function () { return 'bar'; }() );
    
    function foo () {
      // statements
    }    
    ```
    
    B. Assignments, Declarations, Functions ( Named, Expression, Constructor )

	Use the var keyword for each variable declaration. Declaring unititialized variables are the exception, and should go on a single line.
    
	```javascript
    // 2.B.1.1 - multiple var
    // eslint one-var: ["warn", {"initialized": "never", "uninitialized": "always"}]	

    // Bad
    var foo = "",
      bar = "";
    var qux;

    // Good
    var a, b, c; // unitialized can be declared together
    var foo = "";
    var bar = "";
    var qux;

	```
    
	Declarations using var, const, and let should always be at the top of their respective scope.
    
    ```javascript
    // 2.B.1.2 - vars on top
    // eslint vars-on-top: "error"

    // Bad
    function doSomething() {
      var first;    
      if (true) {
          first = true;
      }
      var second;
	}
	// Also Bad, variable declaration in for initializer:
	function doSomething() {
      for ( var i=0; i < 10; i++ ) {}
	}    
        
    // Good
    function doSomething() {
      var first;
      var second; //multiple declarations are allowed at the top
    
      if (true) {
        first = true;
      }
      // all statements after the variables declarations.
	}

	```
    
    Const and let, from ES6, should likewise be at the top of their scope (block).
    
    ```javascript
    // 2.B.1.4 - let on top
    // eslint vars-on-top: "error"
    
    // Bad
    function foo() {
      let foo;
      let bar;
      if ( condition ) {
        bar = '';
        // statements
      }
    }
    // Good
    function foo() {
      let foo;
      if ( condition ) {
        let bar = "";
        // statements
      }
    }
    ```

    ```javascript
    // 2.B.2.1
    // Named Function Declaration
    function foo( arg1, argN ) {

    }

    // Usage
    foo( arg1, argN );


    // 2.B.2.2
    // Named Function Declaration
    function square( number ) {
      return number * number;
    }

    // Usage
    square( 10 );

    // Really contrived continuation passing style
    function square( number, callback ) {
      callback( number * number );
    }

    square( 10, function( square ) {
      // callback statements
    });


    // 2.B.2.3
    // Function Expression
    var square = function( number ) {
      // Return something valuable and relevant
      return number * number;
    };

    // Function Expression with Identifier
    // This preferred form has the added value of being
    // able to call itself and have an identity in stack traces:
    var factorial = function factorial( number ) {
      if ( number < 2 ) {
        return 1;
      }

      return number * factorial( number - 1 );
    };


    // 2.B.2.4
    // Constructor Declaration
    function FooBar( options ) {
      this.options = options;
    }

    // Usage
    var fooBar = new FooBar({ a: "alpha" });

    fooBar.options;
    // { a: "alpha" }
    ```

    C. Exceptions, Slight Deviations

    ```javascript
    // 2.C.1.1
    // Functions with callbacks
    
    foo(function() {
      // Note there is no extra space between the first paren
      // of the executing function call and the word "function"
    });

    // Function accepting an array, no space
    foo([ "alpha", "beta" ]);

    // 2.C.1.2
    // Function accepting an object, no space
    foo({
      a: "alpha",
      b: "beta"
    });

    // Single argument string literal, no space
    foo("bar");

    // Expression parens, no space
    if ( !("foo" in obj) ) {
      obj = (obj.bar || defaults).baz;
    }
    ```

    D. Consistency Always Wins

    In sections 2.A-2.C, the whitespace rules are set forth as a way to promote consistency.
    The important thing is that only one style should exist across the entire source of our project.

    E. Quotes

    We will use single quotes in all js files. Note that json files require double quotes and are not part of this guide. 

    F. End of Lines and Empty Lines

    Whitespace can ruin diffs and make changesets impossible to read.

3. <a name="type">Type Checking</a>

    A. We will use lodash (https://lodash.com/docs/4.16.4#isArguments) "lang" functions to perform type checking. It covers all common cases and makes the code more readable.
    
    Not using Lodash (Don't do this)

	    typeof variable === "string"
        typeof variable === "number"
        typeof variable === "boolean"

    Using Lodash (Do this)
    
    The following examples are for String, Number, Boolean, Object, Array, Element, and null.

        _.isString( "string" );

        _.isString( 3 );

		_.isBoolean(false);

        _.isObject({});

        _.isArray([1, 2, 3]);

        _.isElement(document.body);

		_.isNull(null);

	Check for null or undefined, use isNil.

		_.isNil(null);

    Check for object properties with has.

		var object = { 'a': 'b': 2 };
		_.has(object, 'a');


    B. Coerced Types

    Consider the implications of the following...

    Given this HTML:

    ```html

    <input type="text" id="foo-input" value="1">

    ```
    and this javascript..

    ```javascript
    // 3.B.1.1

    // `foo` has been declared with the value `0` and its type is `number`
    var foo = 0;

    // typeof foo;
    // "number"
    ...

    // Somewhere later in your code, you need to update `foo`
    // with a new value derived from an input element

    foo = document.getElementById("foo-input").value;

    // If you were to test `typeof foo` now, the result would be `string`
    // This means that if you had logic that tested `foo` like:

    if ( foo === 1 ) {
      importantTask();
    }

    // `importantTask()` would never be evaluated, even though `foo` has a value of "1"

    // 3.B.1.2

    // You can preempt issues by using smart coercion with unary + or - operators:

    foo = +document.getElementById("foo-input").value;
    //    ^ unary + operator will convert its right side operand to a number

    // typeof foo;
    // "number"

    if ( foo === 1 ) {
      importantTask();
    }

    // `importantTask()` will be called
    ```

    Here are some common cases along with coercions:

    ```javascript

    // 3.B.2.1

    var number = 1;
    var string = "1";
    var bool = false;

    number;
    // 1

    number + "";
    // "1"

    string;
    // "1"

    +string;
    // 1

    +string++;
    // 1

    string;
    // 2

    bool;
    // false

    +bool;
    // 0

    bool + "";
    // "false"
    ```


    ```javascript
    // 3.B.2.2

    var number = 1;
    var string = "1";
    var bool = true;

    string === number;
    // false

    string === number + "";
    // true

    +string === number;
    // true

    bool === number;
    // false

    +bool === number;
    // true

    bool === string;
    // false

    bool === !!string;
    // true
    ```

    ```javascript
    // 3.B.2.3

    var array = [ "a", "b", "c" ];

    !!~array.indexOf("a");
    // true

    !!~array.indexOf("b");
    // true

    !!~array.indexOf("c");
    // true

    !!~array.indexOf("d");
    // false

    // Note that the above should be considered "unnecessarily clever"
    // Prefer the obvious approach of comparing the returned value of
    // indexOf, like:

    if ( array.indexOf( "a" ) >= 0 ) {
      // ...
    }
    ```

    ```javascript
    // 3.B.2.4

    var num = 2.5;

    parseInt( num, 10 );

    // is the same as...

    ~~num;

    num >> 0;

    num >>> 0;

    // All result in 2


    // Keep in mind however, that negative numbers will be treated differently...

    var neg = -2.5;

    parseInt( neg, 10 );

    // is the same as...

    ~~neg;

    neg >> 0;

    // All result in -2
    // However...

    neg >>> 0;

    // Will result in 4294967294
    ```


4. <a name="cond">Conditional Evaluation</a>

    ```javascript
    // 4.1.1
    // When only evaluating that an array has length,
    // instead of this:
    if ( array.length > 0 ) ...

    // ...evaluate truthiness, like this:
    if ( array.length ) ...


    // 4.1.2
    // When only evaluating that an array is empty,
    // instead of this:
    if ( array.length === 0 ) ...

    // ...evaluate truthiness, like this:
    if ( !array.length ) ...


    // 4.1.3
    // When only evaluating that a string is not empty,
    // instead of this:
    if ( string !== "" ) ...

    // ...evaluate truthiness, like this:
    if ( string ) ...


    // 4.1.4
    // When only evaluating that a string _is_ empty,
    // instead of this:
    if ( string === "" ) ...

    // ...evaluate falsy-ness, like this:
    if ( !string ) ...


    // 4.1.5
    // When only evaluating that a reference is true,
    // instead of this:
    if ( foo === true ) ...

    // ...evaluate like you mean it, take advantage of built in capabilities:
    if ( foo ) ...


    // 4.1.6
    // When evaluating that a reference is false,
    // instead of this:
    if ( foo === false ) ...

    // ...use negation to coerce a true evaluation
    if ( !foo ) ...

    // ...Be careful, this will also match: 0, "", null, undefined, NaN
    // If you _MUST_ test for a boolean false, then use tripple equals
    if ( foo === false ) ...


    // 4.1.7
    // When only evaluating a ref that might be null or undefined, but NOT false, "" or 0,
    // instead of this:
    if ( foo === null || foo === undefined ) ...

    // ...take advantage lodash's isNil, like this:
    if ( _.isNil(foo) ) ...
    ```
    
    
    ALWAYS evaluate for the best, most accurate result - the above is a guideline, not a dogma.

    ```javascript
    // 4.2.1
    // Type coercion and evaluation notes

    // Prefer lodash type functions over `===`, always avoid truthy evaluation using `==`

    // === does not coerce type, which means that:

    "1" === 1;
    // false

    // == does coerce type, which means that:

    "1" == 1;
    // true

    // 4.2.2
    // Booleans, Truthies & Falsies

    // Booleans:
    true, false

    // Truthy:
    "foo", 1

    // Falsy:
    "", 0, null, undefined, NaN, void 0
    ```


5. <a name="practical">Practical Style</a>

    ```javascript
    // 5.1.1
    // A Practical RequireJS Module
    
	define( function ( require ) {
      var ko = require( 'knockout' );
      require( 'viewmodel' );

      var dataVizColors = ['#008ACB', '#9CD59B'];

      return {
        
        dataVizColors: dataVizColors,

        observableStringToBoolean: function observableStringToBoolean( observable ) {
          var value = observable();
          observable( value === 'true' );
        },

    	observableBooleanToString: function observableBooleanToString( observable ) {
	      var value = observable();
          observable( value ? 'true' : 'false' );
        }
        
      };
	} );    		
    ```

    ```javascript
    // 5.2.1
    // A Practical Constructor

      define( function ( require ) {
          var $ = require( 'jquery' );
          var ko = require( 'knockout' );
          var viewmodel = require( 'viewmodel' );
          var bb = require( 'bluebird-utils' );
          var iasUtils = require( 'ias-utils' );
          var time = require( 'time' );


          function TicketBanner( params ) {
              var self = this;
              self.ticket = params.ticket;

              self.isAssignee = function () {
                  return self.ticket.assigned_to() === userName;
              };

              self.masq = function () {
                  var masqUrl = '/account/masq?&url=/ddos/config/{service}&account_id={accountId}'
                          .replace( '{service}', self.ticket.service() )
                          .replace( '{accountId}', accountID );
                  window.open( masqUrl );
              };
          }

          return TicketBanner;
      } );

    ```



6. <a name="naming">Naming</a>

    A. You are not a human code compiler/compressor, so don't try to be one. Use descriptive names to help document the code. Prefer to spell out names and 		avoid abbreviations.

    The following code is an example of egregious naming:

    ```javascript
    // 6.A.1.1
    // Example of code with poor names

    function q(s) {
      return document.querySelectorAll(s);
    }
    var i,a=[],els=q("#foo");
    for(i=0;i<els.length;i++){a.push(els[i]);}
    ```

    Without a doubt, you've written code like this - hopefully that ends today.

    Here's the same piece of logic, but with kinder, more thoughtful naming (and a readable structure):

    ```javascript
    // 6.A.2.1
    // Example of code with improved names

    function query( selector ) {
      return document.querySelectorAll( selector );
    }

    var index = 0;
    var elements = [];
    var matches = query( "#foo" );
    var length = matches.length;

	_.each( matches, function ( match ){
	  elements.push( matches[ index ] );	
	});
    ```

    A few additional naming pointers:
    
    ```javascript
    // 6.A.3.1
    // Naming strings. Plural and singular names should inciate types. 
    // A plural name should inciate a array, singular names a string or object.

    `dog` is a string

    // 6.A.3.2
    // Naming arrays

    `dogs` is an array of `dog` strings


    // 6.A.3.3
    // Naming functions, objects, instances, etc

    camelCase; function and var declarations


    // 6.A.3.4
    // Naming constructors, prototypes, etc.

    PascalCase; constructor function


    // 6.A.3.5
    // Naming regular expressions

    rDesc = //;


    // 6.A.3.6
    // From the Google Closure Library Style Guide

    functionNamesLikeThis;
    variableNamesLikeThis;
    ConstructorNamesLikeThis;
    EnumNamesLikeThis;
    methodNamesLikeThis;
    SYMBOLIC_CONSTANTS_LIKE_THIS;
    ```

    B. Faces of `this`

    Beyond the generally well known use cases of `call` and `apply`, always prefer `.bind( this )` or a functional equivalent, for creating `BoundFunction` definitions for later invocation. Only resort to aliasing when no preferable option is available.

    ```javascript
    // 6.B.1
    function Device( opts ) {
      this.value = null;

      // open an async stream,
      // this will be called continuously
      stream.read( opts.path, function( data ) {

        // Update this instance's current value with the most recent value from the data stream
        this.value = data;

      }.bind(this) );

      // Throttle the frequency of events emitted from
      // this Device instance
      setInterval(function() {

        // Emit a throttled event
        this.emit("event");

      }.bind(this), opts.freq || 100 );
    }

    // Just pretend we've inherited EventEmitter ;)

    ```

    When unavailable, functional equivalents to `.bind` exist in many modern JavaScript libraries.

    ```javascript
    // 6.B.2
    // eg. lodash/underscore, _.bind()
    
    function Device( opts ) {

      this.value = null;

      stream.read( opts.path, _.bind(function( data ) {

        this.value = data;

      }, this) );

      setInterval(_.bind(function() {

        this.emit("event");

      }, this), opts.freq || 100 );
    }
    ```

    As a last resort, create an alias to `this` using `self` as an Identifier. This is extremely bug prone and should be avoided whenever possible.

    ```javascript
    // 6.B.3

    function Device( opts ) {
      var self = this;

      this.value = null;

      stream.read( opts.path, function( data ) {

        self.value = data;

      });

      setInterval(function() {

        self.emit("event");

      }, opts.freq || 100 );
    }
    ```

    C. Use `thisArg`

    Several prototype methods of ES 5.1 built-ins come with a special `thisArg` signature, which should be used whenever possible

    ```javascript
    // 6.C.1

    var obj;

    obj = { f: "foo", b: "bar", q: "qux" };

    Object.keys( obj ).forEach(function( key ) {

      // |this| now refers to `obj`

      console.log( this[ key ] );

    }, obj ); // <-- the last arg is `thisArg`

    // Prints...

    // "foo"
    // "bar"
    // "qux"

    ```

    `thisArg` can be used with `Array.prototype.every`, `Array.prototype.forEach`, `Array.prototype.some`, `Array.prototype.map`, `Array.prototype.filter`

7. <a name="misc">Misc</a>

    This section will serve to illustrate ideas and concepts that should not be considered dogma, but instead exists to encourage questioning practices in an attempt to find better ways to do common JavaScript programming tasks.

    A. Using `switch` should be avoided, modern method tracing will blacklist functions with switch statements

    There seems to be drastic improvements to the execution of `switch` statements in latest releases of Firefox and Chrome.
    http://jsperf.com/switch-vs-object-literal-vs-module

    Notable improvements can be witnessed here as well:
    https://github.com/rwldrn/idiomatic.js/issues/13

    ```javascript
    // 7.A.1.1
    // An example switch statement

    switch( foo ) {
      case "alpha":
        alpha();
        break;
      case "beta":
        beta();
        break;
      default:
        // something to default to
        break;
    }
    ```

    B. Return at the start of a function or the end, but not in the middle. Use a guard to return if a condition is not met, otherwise return
       at the end of the function. This keeps return points predictable and easy to debug.

    ```javascript
    // 7.B.1.1
    // Good:
    function returnLate( foo ) {
      var ret;
      
      // guards can return early
      if( !foo ) {
        return;
      }

      if ( foo.a ) {
        ret = "fooa";
      } else {
        ret = "foo";
      }
      
	  // return at end of function
      return ret;
    }

    // Bad:
    function returnEarly( foo ) {

      if ( foo ) {
        return "foo";
      }
      return "quux";
    }
    ```


8. <a name="native">Native & Host Objects</a>

    The basic principle here is:

    ### Don't do stupid stuff and everything will be ok.

    To reinforce this concept, please watch the following presentation:

    #### “Everything is Permitted: Extending Built-ins” by Andrew Dupont (JSConf2011, Portland, Oregon)

    http://www.everytalk.tv/talks/441-JSConf-Everything-is-Permitted-Extending-Built-ins


9. <a name="comments">Comments</a>

    #### Single line above the code that is subject
    #### Multiline is good
    #### End of line comments are prohibited!
    #### JSDoc style is good, but requires a significant time investment


10. <a name="language">One Language Code</a>

    Programs should be written in one language, whatever that language may be, as dictated by the maintainer or maintainers.


# IAS Architecture Guide

1. Knockout
  A. DOM Interactions. 
  	- Minimize DOM interaction as much as possible by letting KO do it. Bindings should insulate much of the viewmodel code from using the DOM. The idea
  	  is to interact with observables, read and write to those, and then let KO interact with the view.
  	  
  	- When you do have to work with the DOM, do it using a custom binding. This keeps DOM interactions isolated to a certain layer of our application. JQuery plugins should be wrapped in custom bindings.

  B. Components.
    - Use componets as a way to share js and html across different pages and different portals.
    
    - Sizing is entirely subjective, but aim for the Golldilock size. This means components aren't too big or too small. Components that are too big couple together the pages that use them. The pages then become less flexible to changes since they are forced to change together. Using smaller components increases the chance that page specific changes could be made independently, if needed. If components are too small they require more work to create and manage and the benefit of using them is diminished.
    
    - Names should follow a common convention. PATHS or PACKAGES?
    
    - Components should communicate among each other in a standarized way. We plan to implement a pub/sub framework but haven't yet. For the time being, observables can be passed to children components or jQuery events can be used for wider component communcation.
    	


## Appendix

### Comma First.

Any project that cites this document as its base style guide will not accept comma first code formatting, unless explicitly specified otherwise by that project's author.


### Editing markdown

Here's a good gitlab flavored markdown editor. https://jbt.github.io/markdown-editor/



----------


<a rel="license" href="http://creativecommons.org/licenses/by/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by/3.0/80x15.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Principles of Writing Consistent, Idiomatic JavaScript</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/rwldrn/idiomatic.js" property="cc:attributionName" rel="cc:attributionURL">Rick Waldron and Contributors</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/3.0/deed.en_US">Creative Commons Attribution 3.0 Unported License</a>.<br />Based on a work at <a xmlns:dct="http://purl.org/dc/terms/" href="https://github.com/rwldrn/idiomatic.js" rel="dct:source">github.com/rwldrn/idiomatic.js</a>.
