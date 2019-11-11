---
description: A strongly typed code first schema builder for GraphQL
---

# GiraphQL

## Getting started

```typescript
import SchemaBuilder from '@giraphql/core'

type ContextShape = {}

const builder = new SchemaBuilder<{
  Input: {
    // optional place to define shapes for input types
    // or overwrite types of scalars in args
  };
  Output: {
    // backing models/shapes for objects and interfaces
    // can also overwrite the scalars return values in resolvers
    User: {
      first: string;
      last: string;
    };
  };
}, ContextShape>()

const User = builder.createObject('User', {
  shape: t => ({
    firstName: t.exposeString('first', {}),
    lastName: t.exposeString('first', {}),
    fullName: t.string({
      resolve: parent => `${parent.first} ${parent.last}`
    }),
  }),
})

const Query = builder.createObject('Query', () => {
  shape: t =>({
    user: t.field({
      type: 'User',
      resolve: () => { first: 'Leia', last: 'Organa' }
    })
  })
})

const schema = builder.toSchema([Query, User])
```





