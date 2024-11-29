template
==
A template processor.

[![standwithukraine](docs/StandWithUkraine.svg)](https://ukrainewar.carrd.co/)
[![](https://github.com/paiv/template-py/actions/workflows/build.yml/badge.svg)](https://github.com/paiv/template-py/actions)


Install
--

Install with pip:
```sh
pip install 'git+https://github.com/paiv/template-py'
```


Usage
--

```py
import template
template.format(text, context=None, varchar='&', /, **kwargs)
```

The only substitution is of `&` variables in the form:
- `&name`
- `&{name}`

Basic substitution:
```py
template.format('&v &{v}', v='hello')
'hello hello'
```

If a value is not provided, the variable will not be substituted.
```py
template.format('&v &s, &{v} &{s}', v='hello')
'hello &s, hello &{s}'
```

Environment can be provided in a dictionary:
```py
context = dict(v='hello')
template.format('&v &{v}', context)
'hello hello'
```

Environment values can be of the types:
- `str` - simple string
- `generator` - a generator of strings
- `callable` - a function returning `str` or a `generator`
- any other type will be converted to `str`

The leading character of variables can be changed:
```py
template.format('#v #{v}', None, '#', v='hello')
'hello hello'
```

Example
--

```py
import template

def gen():
    yield 'alice\n'
    yield 'bob\n'

context = dict()
context['data'] = gen

tpl = '''
Greetings,
  &data
'''

template.format(tpl, context)
'''
Greetings,
  alice
  bob
'''
```
