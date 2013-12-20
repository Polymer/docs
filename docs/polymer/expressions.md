---
layout: default
title: Expressions
---

<p><buildbot-list project="polymer-expressions"></buildbot-list></p>

{% include toc.html %}

Within a `<polymer-element>` you can use {{site.project_title}}'s [expression library](https://github.com/polymer/polymer-expressions) to write inline expressions, statements, named scopes, repeats anywhere {%raw%}`{{}}`{%endraw%} bindings are used.

## Inline expressions

{{site.project_title}} supports expressions in {%raw%}`{{}}`{%endraw%} with a strict
subset of the JavaScript language. In order to use this feature, it's
important to understand its behavior and limitations:

- The goal for inline expressions is to allow the expression of simple value
concepts and relationships. It is generally bad practice to put complex logic
into your HTML (view).
- Expressions are never run (e.g. `eval`) as page script. They cannot access any
global state (e.g. `window`). They are parsed and converted to a simple
interpreted form which is provided the present values of paths contained in
the expression.

The specific subset of JavaScript which is supported is:

| Feature | Example | Explanation
|---------|
|Identifiers & paths | `foo`, `foo.bar.baz` | These values are treated as relative to the local model, extracted, observed for changes and cause the expression to be re-evaluated if one or more has changed.
| Logical not operator | `!` |
| Unary operators | `+foo`, `-bar` | Converted to `Number`. Or converted to `Number`, then negated.
| Binary operators | `foo + bar`, `foo - bar`, `foo * bar` | Supported: `+`, `-`, `*`, `/`, `%`
| Comparators | `foo < bar`, `foo != bar`, `foo == bar` | Supported: `<`, `>`, `<=`, `>=`, `==`, `!=`, `===`, `!==`
| Logical comparators | `foo && bar || baz` | Supported: `||`, `&&`
| Ternary operator | `a ? b : c` |
| Grouping (parenthesis) | `(a + b) * (c + d)` |
| Literal values | numbers, strings, `null`, `undefined` | Escaped strings and non-decimal numbers are not supported. |
| Array & Object initializers | `[foo, 1]`, `{id: 1, foo: bar}` |
{: .table }

### Statements

Expressions are parsed when they're within a mustache ({% raw %}`{{}}`{% endraw %}).
The expression can be a single statement, or an object.

Whenever the value of one or more paths in the expression change, the value of the expression re-evaluated and the result inserted as the value of the mustache:

{% raw %}
    <div>Jill has {{ daughter.children.length + son.children.length }} grandchildren</div>
{% endraw %}

may result in:

    <div>Jill has 100 grandchildren</div>

You can hand off an object a filter. For example, using an object with the `tokenList` filter, the value of the mustache will include the space-separated keys from the object whose corresponding expressions are truthy:

{% raw %}
    <div class="{{ {active: user.selected, big: user.type == 'super'} | tokenList }}"> 
{% endraw %}

may result in:

    <div class="active big"> 

## Named scopes

Named scopes are useful for referencing a model value from an "outer" model "scope".
For example:

{% raw %}
    <template repeat="{{ user in users }}">
      {{ user.name }}
      <template repeat="{{ file in user.files }}">
        {{ user.name }} owners {{ file.name }}
      </template>
    </template>
{% endraw %}

You can also use the key/value notation to get at the index of the current iteration:

{% raw %}
    <template repeat="{{ user, i in users }}">
      {{ user.name }}
      <template repeat="{{ file, j in user.files }}">
        {{ i }}:{{ j }} {{ user.name }} owners {{ file.name }}
      </template>
    </template>
{% endraw %}

The scope naming is available (but optional) inside the `template`, `bind`, and `repeat` directives.

{% raw %}
- `bind` syntax: `<template bind="{{ expression as identifier }}">`
- `repeat` syntax: `<template repeat="{{ identifier in expression }}">`, `<template repeat="{{ val, i in expression }}">`
{% endraw %}

**Note:** that `expression` can be a simple identifier, a path or a full
expression (including Object and Array literals).
{: .alert .alert-info }

### Nested scoping rules

If a `<template>` using a named scoped contains children `<template>`s,
all ancestor scopes are visible, up-to and including the first ancestor **not** using a named scope.

{% raw %}
    <template bind="{{ foo as foo }}">
      <!-- foo.* available -->
      <template bind="{{ foo.bar as bar }}">
        <!-- foo.* & bar.* available -->
        <template bind="{{ bar.bat }}">
          <!-- only properties of bat are available -->
          <template bind="{{ boo as bot }}">
            <!-- bot.* and properties of bat are available. NOT foo.* or bar.* -->
          </template>
        </template>
      </template>
    </template>
{% endraw %}

## Binding to attributes

Binding expressions to certain attributes can produce side effects in browsers that don't implement `<template>` natively. For example, running {% raw %}`<img src="/users/{{ id }}.jpg">`{% endraw %} under the polyfill produces a network request that 404s.

In addition, browsers such as IE sanitize certain attributes, disallowing {% raw %}`{{}}`{% endraw %} replacements in their text.

To counterattack these side effects, bindings in certain attributes can be prefixed with "_":

{% raw %}
    <img _src="/users/{{ id }}.jpg">
    <div _style="color: {{ color }}">
    <a _href="{{ url }}">Link</a>
    <input type="number" _value="{{ number }}">
{% endraw %}
