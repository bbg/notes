Tired of writing such imports?

```js
import test from '../../../components/Test.tsx';
```

If you're using a typescript, you can create a path alias instead:

```js
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@/*": ["./*"] 
    }
  }
}
```

Then you can import more conveniently:

```js
import test from '@/components/Test.tsx';
```

Don't forget to restart your running task to affect changes.
