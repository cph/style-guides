# The Emerging Products Style Guide

General rules follow; but we also have language-specific guides for [Ruby](/lang/Ruby.md), [CoffeeScript](/lang/CoffeeScript.md), and [HTML/CSS](/lang/CSS.md).



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

 - Use blank lines (but no more than one) _within_ methods to separate logical paragraphs:

    ```ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
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

- Make commits as small as possible while making sure not to commit code when the project is in a broken state.
> Why? Smaller commits help to document your changes and also to break big projects down into small steps.

- Minimize "commit noise", i.e., changes that don't affect the code or state of the project, especially whitespace changes.

- Begin commit messages with a tag in square brackets.
> Why? Houston uses these when generating release notes.
> Commits tagged `[feature]`, `[improvement]`, and `[fix]` will all be turned into release notes that we show to customers. Commits with other tags will only be seen by developers.

  ```bash
  git commit -m "[feature] ..."
  ```

  ###### Tags
    - `[feature]` - this commit introduces a new, user-facing feature.
    - `[improvement]` - this commit significantly improves an existing user-facing feature.
    - `[fix]`/`[bugfix]` - this commit fixes a significant, user-facing bug.
    - `[skip]` - this commit is not significant enough for the customer to care about it.
    - `[wip]` - only use for a work-in-progress commit you don't intend to merge as-is.
    - `[squash]` - **(experimental)** this commit should ultimately be squashed into a previous commit.

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

- Mention alerts and tasks in your commits.
> Why? Houston uses these to make things run more smoothly.

   ```bash
   git commit -m "[fix] Resolved this Errbit problem {{err:123}}"
   git commit -m "[fix] Resolved this ITSM {{itsm:123}}"
   git commit -m "[fix] Resolved this Zendesk ticket {{zendesk:123}}"
   git commit -m "[fix] Upgraded a vulnerable dependency {{cve:123}}"
   git commit -m "[improvement] Completed this sprint task [#1234a]"
   ```

- Include the amount of time spent writing the code in parentheses with a one-character unit following each number. E.g., `(1h)` is one hour, `(30m)` is 30 minutes, etc.
> Why? Um... because we've been doing that for a while. It might be useful :grin:

   ```bash
   git commit -m "[feature] Added the ability to determine if any program halts (13h)"
   git commit -m "[skip] Deleted a few lines of unused code (2m)"
   ```

- If you need to further explain your commit, use the description field.


- When pair-programming, use `ep pair` to change your git identity to the combined identities of those pairing. Use `ep unpair` to go back to committing as only yourself.

    ```bash
    ep pair ben chase matt
    git commit -m "[feature] Added the option to have tacos delivered to your door (4h)"
    ```
