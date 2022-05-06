# Defining families

Recall that families define rules regarding components. Geary lets us define those rules using the `family {}` builder:

The builder has three selectors: `and, not, or`, and a `has` operation which acts like the same operation on entities.

Suppose we have three components, `A, B, C`, let's try to build some families from them.

=== ":octicons-file-code-16: A and B"
    
    ```kotlin
    family {
        and {
            has<A>()
            has<B>()
        }
    }
    ```
    Families default to the and selector, so this is equivalent to the following:
    
    ```kotlin
    family {
        has<A>()
        has<B>()
    }
    ```

=== ":octicons-file-code-16: A or B or C"

    ```kotlin
    family {
        or {
            has<A>()
            has<B>()
            has<C>()
        }
    }
    ```

=== ":octicons-file-code-16: (A or B) and not C"

    ```kotlin
    family {
        or {
            has<A>()
            has<B>()
        }
        not {
            has<C>()
        }
    }
    ```

=== ":octicons-file-code-16: (A or B) and not (B and C)"

    ```kotlin
    family {
        or {
            has<A>()
            has<B>()
        }
        not {
            and {
                has<B>()
                has<C>()
            }
        }
    }
    ```

## Usage

Once created, a family can check if an entity matches it with `#!kotlin entity in family // Boolean`. More importantly, we can now use them in our systems for fast pattern matching.

