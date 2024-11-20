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

The only substitution is of `&`-variables in the form:
- `&name`
- `&{name}`

Basic substitution:
```py
import template
template.format('&v &{v}', v='hello')
'hello hello'
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

Example:
```py
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
