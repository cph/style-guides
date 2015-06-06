# CoffeeScript Style Guide



## Naming

 - Use `camelCase` for local variables, instance variables, and function names

    ```coffee
    localVariable = 'good'

    functionName = ->
      console.log 'hi'
    ```
 
 - Use `TitleCase` for classes and modules

    ```coffee
    @App ?= {}
    @App.Views ?= {}
    class @App.Views.ClassName
      # ...
    ```

 - Use `SCREAMING_SNAKE_CASE` for other constants.
