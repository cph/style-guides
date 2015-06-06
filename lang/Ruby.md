# Ruby Style Guide

We've adapted this from [GitHub's Ruby style guide](https://github.com/styleguide/ruby). Feel free to make a pull request to add to this guide!



## Naming

 - Use `snake_case` for methods and variables.

 - Use `TitleCase` for classes and modules. (Keep acronyms like HTTP, RFC, and XML uppercase.)

 - Use `SCREAMING_SNAKE_CASE` for other constants.

 - The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. `Array#empty?`).

 - The names of potentially "dangerous" methods (i.e. methods that modify `self` or the arguments, `exit!`, etc.) should end with an exclamation mark. Bang methods should only exist if a non-bang method exists. ([More on this](http://dablog.rubypal.com/2007/8/15/bang-methods-or-danger-will-rubyist)).



## Spaces

 - Use spaces around operators, after commas, colons, and semicolons, around `{` and before `}`.

    ```ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? false : true; puts "Hi"
    [1, 2, 3].each { |e| puts e }
    ```

 - Don't put spaces after `(`, `[` or before `]`, `)`.

    ```ruby
    some(arg).other
    [1, 2, 3].length
    ```

 - Don't put spaces after `!`.

    ```ruby
    !array.include?(element)
    ```

 - Don't put spaces between a method name and `(`.

    ```ruby
    # bad
    f (3 + 2) + 1
    
    # good
    f(3 + 2) + 1
    ```

 - Don't put spaces around the `=` operator when assigning default values to method parameters:

    ```ruby
    # bad
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    
    # good
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end
    ```



## Indentation

 - Indent `when` as deep as `case`.

    ```ruby
    case
    when song.name == "Misty"
      puts "Not again!"
    when song.duration > 120
      puts "Too long!"
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end
    ```

 - Indent `public/protected/private` keywords as deep as `class`.
 > Why? These keywords apply — not just to the next method — but to all subsequent ones. They need to stand out from the level of the `def` statements.

    ```ruby
    class SomeClass
      
      def public_method
        # ...
      end
      
    private
      
      def private_method
        # ...
      end
      
    end
    ```



## Parentheses

 - Use `def` with parentheses when the method takes arguments. Omit the parentheses when it doesn't.

    ```ruby
    def some_method
      # ...
    end

    def some_method_with_arguments(arg1, arg2)
      # ...
    end
    ```

 - Don't use parentheses around the condition of an `if/unless/while`.

    ```ruby
    # bad
    if (x > 10)
      # ...
    end
    
    # good
    if x > 10
      # ...
    end
    ```

 - If the first argument to a method begins with an open parenthesis, always use parentheses in the method invocation.

    ```ruby
    # bad
    f (3 + 2) + 1
    
    # good
    f((3 + 2) + 1)
    ```



## Rubyisms

 - Never use `for`, unless you know exactly why. Use iterators (e.g. `each`) instead.
 > `for` is implemented in terms of `each` (so you're adding a level of indirection), but with a twist - `for` doesn't introduce a new scope (unlike `each`) and variables defined in its block will be visible outside it.

    ```ruby
    # bad
    for elem in arr do
      puts elem
    end
    
    # good
    arr.each do |elem|
      puts elem
    end
    ```

 - Prefer string interpolation to concatenation:

    ```ruby
    # bad
    email_with_name = user.name + " <" + user.email + ">"
    
    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

 - Use double-quoted strings.
 > Why? Interpolation and escaped characters will always work without a delimiter change, and `'` is a lot more common than `"` in string literals.

    ```ruby
    # bad
    name = 'Bozhidar'
    
    # good
    name = "Bozhidar"
    ```

 - Use `&&` and `||`, not `and` and `or`.

 - Avoid `return` where not required. However, a great place to use an explicit `return` is with an `if/unless` in the postfix position as a guard clause.

    ```ruby
    # bad
    def size_of(list)
      return list.size
    end
    
    # good
    def size_of(list)
      list.size
    end
    
    # good
    def size_of(list)
      return 0 unless list.respond_to?(:size)
      list.size
    end
    ```

 - Avoid explicit use of `self` as the recipient of internal class or instance messages unless assigning a value (without `self`, Ruby will create a local variable).

    ```ruby
    class Person
      attr_accessor :name, :address
      
      # bad
      def mailing_label
        [self.name, self.address].join "\n"
      end
      
      # good
      def mailing_label
        [name, address].join "\n"
      end
      
      # bad (declares a new local variable)
      def move_to(new_address)
        address = new_address
      end
      
      # good
      def move_to(new_address)
        self.address = new_address
      end
      
    end
    ```

 - Prefer symbols to strings as hash keys (and prefer the JSON-style syntax over the hashrocket syntax).

    ```ruby
    # ok
    hash = { "one" => 1, "two" => 2, "three" => 3 }
    
    # ok
    hash = { :one => 1, :two => 2, :three => 3 }
    
    # best
    hash = { one: 1, two: 2, three: 3 }
    ```

 - Use `%w` freely for arrays of short strings.

    ```ruby
    STATES = %w(draft open closed)
    ```

 - Use `x` modifier for complex regular expressions. This makes them more readable and you can add some useful comments. Just be careful as spaces are ignored.

    ```ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

 - Keyword arguments are recommended but not required when a method's arguments may otherwise be opaque or non-obvious when called. Additionally, prefer them over the old "Hash as pseudo-named args" style from pre-2.0 ruby.

   So instead of this:

    ```ruby
    def remove_member(user, skip_membership_check=false)
      # ...
    end
    
    # Elsewhere: what does true mean here?
    remove_member(user, true)
    ```
   
   Do this, which is much clearer.
   
    ```ruby
    def remove_member(user, skip_membership_check: false)
      # ...
    end
    
    # Elsewhere, now with more clarity:
    remove_member user, skip_membership_check: true
    ```



## Conditionals

 - Never use `then` for multi-line `if/unless`.

    ```ruby
    # bad
    if some_condition then
      # ...
    end
    
    # good
    if some_condition
      # ...
    end
    ```

 - Avoid the ternary operator (`?:`) except in cases where all expressions are extremely trivial. However, do use the ternary operator over `if/then/else/end` constructs for single line conditionals. (Never use the ternary operator for multi-line conditionals)

    ```ruby
    # bad
    result = if some_condition then something else something_else end
    
    # good
    result = some_condition ? something : something_else
    ```

 - Use one expression per branch in a ternary operator. This also means that ternary operators must not be nested. Prefer `if/else` constructs in these cases.

    ```ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else
    
    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

 - Favor modifier (postfix) `if/unless` usage when you have a single-line body.

    ```ruby
    # bad
    if some_condition
      do_something
    end
   
    # good
    do_something if some_condition
    ```

 - Never use `unless` with `else`. Rewrite these with the positive case first.

    ```ruby
    # bad
    unless success?
      puts "failure"
    else
      puts "success"
    end
    
    # good
    if success?
      puts "success"
    else
      puts "failure"
    end
    ```



## Blocks

 - Prefer `{...}` over `do...end` for single-line blocks.

    ```ruby
    # bad
    names.select do |name|
      name.start_with?("S")
    end

    # good
    names.select { |name| name.start_with?("S") }
    ```
 
 - Prefer single-line `{...}` blocks when several blocks are chained.

    ```ruby
    # bad
    names.select do |name|
      name.start_with?("S")
    end.map { |name| name.upcase }
    
    # good
    names
      .select { |name| name.start_with?("S") }
      .map { |name| name.upcase }
    ```

 - Prefer multiline `do...end` for blocks that do control flow or method definitions.

    ```ruby
    File.open("~/Desktop/diary.txt") do |f|
      f.write "I love Ruby so much"
    end

    task :escape do
      system "sudo shutdown now"
    end
    ```

 - Prefer the symbol shorthand to spelling out a proc that calls a method on its argument.

    ```ruby
    # ok
    names.map { |name| name.upcase }

    # better
    names.map(&:upcase)
    ```

 - Use `_` for unused block parameters.

    ```ruby
    # ok
    names = results.map { |name, age, favorite_marvel_movie| name }
    
    # better
    names = results.map { |name, _, _| name }
    ```



## Dragons

 - Don't use the `===` (threequals) operator to check types.
 > Why? `===` is mostly an implementation detail to support Ruby features like `case`, and it's not commutative. For example, `String === "hi"` is true and `"hi" === String` is false. Instead, use `is_a?` or `kind_of?` if you must. Refactoring is even better. It's worth looking hard at any code that explicitly checks types.

 - Avoid the usage of class (`@@`) variables. (Prefer class instance variables)
 > Why? All the classes in a class hierarchy actually share one class variable. This often has unintended results.

    ```ruby
    class Parent
      @@class_var = "parent"
    
      def self.print_class_var
        puts @@class_var
      end
    end
    
    class Child < Parent
      @@class_var = "child"
    end
    
    Parent.print_class_var # => will print "child"
    ```

 - Prefer `<<` to concatenate strings.
 > Why? `String#<<` mutates the string instance in-place and is always faster than `String#+`, which creates a bunch of new string objects.

    ```ruby
    # good and also fast
    html = ""
    html << "<h1>Page title</h1>"
    
    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

 - Be careful with `^` and `$` as they match start/end of line, not string endings. If you want to match the whole string use: `\A` and `\z`.

    ```ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```


