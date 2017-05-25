Postfix Conditionals
==============================

Postfix conditionals is code that looks like the following:

```ruby
do_stuff if admin?
```

It results in a terser syntax, but one that could mislead other developers.

Why Not
-----------------------------

### Yoda Conditionals

Postfix conditionals are hard to look at and parse, largely because the
structure doesn't match typical English syntax.

```ruby
if admin?
  do_stuff
end
```

This reads in English as "if I'm an admin then do stuff".

```ruby
do_stuff if admin?
```

This reads in English as "do stuff, if an admin I am." When conditionals are at
the end you have to mentally backtrack and place a restriction on behavior,
rather than knowing upfront that restrictions exist.

### Hiding Intent

A large reason to use postfix conditionals is that it results in terser syntax.
A lot of functionality can happen in a single line which would otherwise take
three. However, this is often bad as it can hide complex branching behavior
which would otherwise be refactored away.

For instance:

```ruby
def admin?
  return true if has_access_to? Item
  return true if super_admin?
  false
end
```

Three lines doesn't look like something to refactor. However, some complex
branching is happening here which is quickly displayed by removing the
postfixes:

```ruby
def admin?
  if has_access_to? Item
    true
  elsif super_admin?
    true
  else
   false
  end
end
```

It's easier to look at a big branch and see the future pain and violation of
single responsibility principle.


Recommendation
-----------------------------

Avoid postfix conditionals. They're confusing to look at, hard to parse, and
actively hide functionality that's ripe for extraction or simplification.
