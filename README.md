# The Emerging Products Style Guide

General rules follow; but we also have language-specific guides for [Ruby](/lang/Ruby.md), [CoffeeScript](/lang/CoffeeScript.md), and [CSS](/lang/CSS.md).


## White Space

 - Use soft-tabs with a two-space indent
 
 - End every file with a blank newline
 > Why? This reduces diff-noise when a change is made to the last LOC in a file

 - In general, leave a blank line between methods. However, multiple line breaks can be used to separate "paragraphs" of related methods:

    ```ruby
    def render_header
      # ...
    end
   
    def render_contributions
      # ...
    end
   
   
   
    def contributions_for(contributor)
      # ...
    end
   
    def pledges_for(contributor)
      # ...
    end
    ```

 - `context` and `describe` blocks that begin a group of tests do not need an interior line break

    ```ruby
    context "whatever" do
      should "whatever" do
        # ...
      end
   
      should "whatever b" do
        # ...
      end
    end
    ```

## Commits

 - Commit as you go. Prefer small commits that describe a single change to monolithic commits.
 > Why? This helps to document your changes and also to break big projects down into small steps.

 - Write commit messages that assume the committer as the subject, have a past-tense verb, and describe your change in customers' terms.
 > Why? Houston generates release notes from commit messages.

    ```bash
    # bad
    git commit -m "update"
   
    # bad
    git commit -m "Don't leak the nuclear launch codes"
   
    # good
    git commit -m "Prevented unauthorized users from reading our launch codes"
    ```

 - Begin commit messages with a tag in square brackets.
 > Why? Houston uses these when generating release notes.
 >
 > Commits tagged `[feature]`, `[improvement]`, and `[fix]` will all be turned into
 > release notes that we show to customers. Commits tagged `[refactor]` and `[skip]`
 > will only be seen by developers.
 
    ```bash
    git commit -m "[feature] ..."
    ```

 - When a commit resolves a task, mention it in square brackets:<br/>
   —and when it resolves an Errbit err, mention that in curly braces:
 > Why? Houston will mark it resolved for you

    ```bash
    git commit -m "[feature] Added a progress indicator for long-running tasks [#63a]"
    git commit -m "[fix] Ensured that age is always a number {{err:89201}}"
    ```

 - Record the amount of time you spent on each commit:
 > Why? Um... because we've been doing that for a while. It might be useful :grin:

    ```bash
    git commit -m "[feature] Added the ability to determine if any program halts (13h)"
    git commit -m "[skip] Deleted a few lines of unused code (2m)"
    ```

 - When pair-programming, give credit to both pairs:

    ```bash
    ep pair gene chase
    git commit -m "[feature] Added the option to have tacos delivered to your door (4h)"
    ```
