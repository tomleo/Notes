## urlencode

### Email

```html
<a href="mailto:{{ email|urlencode:"@" }}
```

## Counting Things

``` python
from functools import partial
from collections import defaultdict
totals = defaultdict(partial(defaultdict, int))
for obj in Obj.objects.all():
    totals[obj.foo][obj.bar] += 1
```

Unfortunatly django templates cannot iterate over defaultdicts. And
simply doing `dict(totals)` will not work because this only changes
the outer default dict.

So the final step is to make it work in templates is:

``` python
totals = {k: dict(v) for k, v in totals.items()}
```

Now you can do things like this:

``` django
{% for k, dd in totals %}
    {% for dk, dv in dd %}
        {{ k }} | {{ dk }} | {{ dv }}
{% endfor %}
```

### Using Pandas

``` javascript
import pandas as pd
records = (
    Obj.objects.values('foo', 'bar')
    .annotate(count=Count('bar'))
    .order_by('foo', 'bar')
)

df = pd.DataFrame.from_records(records, index=['foo', 'bar'])
df.loc[('foo1', 'bar1')]
```
