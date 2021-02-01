# CoffeeScript Style Guide



## Naming

 - Use `camelCase` for local variables, instance variables, and function names

    ```coffee
    localVariable = "good"

    functionName = ->
      console.log "hi"
    ```
 
 - Use `TitleCase` for classes and modules

    ```coffee
    @App ?= {}
    @App.Views ?= {}
    class @App.Views.ClassName
      # ...
    ```

 - Use `SCREAMING_SNAKE_CASE` for other constants.



## Spaces

 - Use spaces around operators and after commas and semicolons.

    ```coffee
    sum = 1 + 2
    [a, b] = [1, 2]
    c = 3; d = 4
    ```

 - Use a space between a function's argument list and it's stabby lambda.

    ```coffee
    # bad
    foo = (arg1, arg2)->
    
    # good
    foo = (arg1, arg2) -> # Yes
    ```

 - Don't put spaces within `(...)` or `[...]`.

    ```coffee
    some(arg).other
    [1, 2, 3].length
    ```

 - Don't put spaces before `(` when calling a function.

    ```coffee
    # bad
    f (3 + 2) + 1
    
    # good
    f(3 + 2) + 1
    ```

 - Don't put spaces around the `=` operator when assigning default values to method parameters.

    ```coffee
    # bad
    test: (param = null) ->
    
    # good
    test: (param=null) ->
    ```



## Indentation

 - When method calls are chained, put each on a line and indent with two spaces.

   ```coffee
   # good
   [1..3]
     .map (x) -> x * x
     .concat [10..12]
     .filter (x) -> x < 11
     .reduce (x, y) -> x + y
   ```



## Chaff

 - Avoid trailing semicolons.

    ```coffee
    # bad
    console.log("they aren't necessary");
    ```

 - Avoid trailing commas when an array or object definition doesn't require them.

    ```coffee
    # bad
    foo = [
      "some",
      "string",
      "values"
    ]
    bar:
      label: "test",
      value: 87
    
    # good
    foo = [
      "some"
      "string"
      "values"
    ]
    bar:
      label: "test"
      value: 87
    ```

 - Avoid `return` where not required. However, a great place to use an explicit `return` is with an `if/unless` in the postfix position as a guard clause.

    ```coffee
    # bad
    size_of: (list) ->
      return list.size
    
    # good
    size_of: (list) ->
      list.size
    
    # good
    size_of: (list) ->
      return 0 unless list.size?
      list.size
    ```



## Strings

 - Prefer string interpolation to concatenation:

    ```coffee
    # bad
    fancyEmail = user.name + " <" + user.email + ">"
    
    # good
    fancyEmail = "#{user.name} <#{user.email}>"
    ```

 - Use double-quoted strings.
 > Why? Interpolation and escaped characters will always work without a delimiter change, and `'` is a lot more common than `"` in string literals.

    ```coffee
    # bad
    name = 'Bozhidar'
    
    # good
    name = "Bozhidar"
    ```



## Conditionals

 - Never use `then` for multi-line `if/unless`.

    ```coffee
    # bad
    if some_condition then
      # ...
    
    # good
    if some_condition
      # ...
    ```

 - Favor modifier (postfix) `if/unless` usage when you have a single-line body.

    ```coffee
    # bad
    if somethingIsTrue
      doSomething
   
    # good
    doSomething if somethingIsTrue
    ```

 - Never use `unless` with `else`. Rewrite these with the positive case first.

    ```coffee
    # bad
    unless success
      alert "failure"
    else
      callback()
    
    # good
    if success
      callback()
    else
      alert "failure"
    ```

 - Don't use `if boolean is false` over `if not boolean`
 
   ```coffee
   # nope
   if coffeeScriptIsDead is false
     keepDancing()
   else
     weAreInTrouble()
   
   # yep
   if not coffeeScriptIsDead
     keepDancing()
   else
     weAreInTrouble()
   ```


## Idiomatic CoffeeScript

 - `and` is preferred over `&&`.
 - `or` is preferred over `||`.
 - `is` is preferred over `==`.
 - `isnt` is preferred over `!=`.
 - `not` is preferred over `!`.
 - `@` is preferred over `this`.

    ```coffee
    # bad
    fn (property=null) ->
      return this[property] if property?
      this
    
    # good
    fn (property=null) ->
      return @[property] if property?
      @
    ```

 - Use the `::` shorthand for accessing an object's prototype.

    ```coffee
    # bad
    Array.prototype.slice
    
    # good
    Array::slice
    ```

 - Take advantage of comprehensions.

    ```coffee
    # bad
    results = []
    for item in array
      results.push item.name
    
    # good
    result = (item.name for item in array)
    ```

 - Avoid hash-rockets (`=>`) when they are unnecessary.

    ```coffee
    # bad
    $el.on "click", ->
      $.get(url).then =>
        console.log "this (@) is never used"
    
    # good
    $el.on "click", ->
      $.get(url).then ->
        console.log "this (@) is never used"
    ```



## Other

 - Always throw Errors, not strings (c.f. [documented Error types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#Error_types))

    ```coffee
    # bad
    throw "expected string, got number"
    
    # good
    throw new TypeError "expected string, got number"
    ```

