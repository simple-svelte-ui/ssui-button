# @simple-svelte-ui/button

Native HTML button component enhanced with design to provide easy integration with Svelte.

## Installation

Install with npm:

```bash
npm install @simple-svelte-ui/button
```

Install with yarn:

```bash
yarn add @simple-svelte-ui/button
```

## Usage/Examples

Svelte REPL example is avaliable [here](https://svelte.dev/repl/e1d1e77a86a94ff3947c68f0e99c4e00?version=3.49.0)

```js
<script>
    import {Button} from '@simple-svelte-ui/button';
</script>
<Button>Click me</Button>
```

Further more  `CButton` is also provided in this package with following customization

```js
<script>
    import {CButton} from '@simple-svelte-ui/button';
</script>
<CButton
  variant = 'contained'
  backgroundColor = 'red'
  foregroundColor = 'black'
  rippleColor='blue'
  textTransform='lowercase'
  fontWeight='bold'
  >
  Hello</CButton>
```
