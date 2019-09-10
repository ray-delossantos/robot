


A __transition__ is used to move from one state to another. Transitions are triggered by *events*, the first argument to the `transition` function. The second argument is the *destination state*.

Optionally you can add [guards](#guard) and [reducers](#reduce) to transitions. Guards will be applied first; if a guard returns `false` the reducers will not run.

Transitions are *always* a child of either [state](#state) or [invoke](#invoke) types. Here's a typical transition from one state to another:

```js
import { createMachine, state, transition } from '@matthewp/robot';

const machine = createMachine({
  sleep: state(
    transition('wake', 'breakfast')
  ),
  breakfast: state(
    transition('eat', 'work')
  )
});
```

And similarly when used with an [invoke](#invoke) state:
---
layout: api.njk
title: transition
tags: api
permalink: api/transition.html
---

# transition

```js
import { createMachine, invoke, state, transition } from '@matthewp/robot';
import { loadUsers } from './api.js';

const machine = createMachine({
  idle: state(
    transition('load', 'loading')
  )
  loading: invoke(loadUsers,
    transition('done', 'show',
      reduce((ev, ctx) => ({ ...ctx, users: ev.data })))
  )
}, () => ({ users: [] }));
```

There can be __multiple transitions__ for the same event, in which case the first will be chosen if its [guards](#guard) pass:

```js
import { createMachine, guard, state, transition } from '@matthewp/robot';

const machine = createMachine({
  shopping: state(
    transition('buy', 'food',
      guard(amHungry)
    ),
    transition('buy', 'clothes')
  )
});
```