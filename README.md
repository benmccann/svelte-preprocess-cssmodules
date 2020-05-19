# Svelte preprocess CSS Modules

Generate CSS Modules classname on Svelte components

```bash
npm install --save-dev svelte-preprocess-cssmodules
```

## Rollup Configuration

To be used with the plugin `rollup-plugin-svelte`.

```js
import svelte from 'rollup-plugin-svelte';
import cssModules from 'svelte-preprocess-cssmodules';

export default {
  ...
  plugins: [
    svelte({
      preprocess: [
        cssModules(),
      ]
    }),
  ]
  ...
}
```

## Options
Pass an object of the following properties

| Name | Type | Default | Description |
| ------------- | ------------- | ------------- | ------------- |
| `localIdentName` | `{String}` | `"[local]-[hash:base64:6]"` |  A rule using any available token from [webpack interpolateName](https://github.com/webpack/loader-utils#interpolatename) |
| `includePaths` | `{Array}` | `[]` (Any) | An array of paths to be processed |


## Usage on Svelte Component

**On the HTML markup** (not the CSS), Prefix any class name that require CSS Modules by *$style.*  => `$style.My_CLASSNAME`

```html
<style>
  .red { color: red; }
</style>

<p class="$style.red">My red text</p>
```

The component will be transformed to

```html
<style>
  .red-30_1IC { color: red; }
</style>

<p class="red-30_1IC">My red text</p>
```

### Replace only the required class

CSS Modules classname are generated to the html class values prefixed by `$style.`. The rest is left untouched.

*Before*

```html
<style>
  .blue { color: blue; }
  .red { color: red; }
  .text-center { text-align: center; }
</style>

<p class="blue text-center">My blue text</p>
<p class="$style.red text-center">My red text</p>
```

*After*

```html
<style>
  .blue { color: blue;
  }
  .red-2iBDzf { color: red; }
  .text-center { text-align: center; }
</style>

<p class="blue text-center">My blue text</p>
<p class="red-2iBDzf text-center">My red text</p>
```

### Remove unspecified class

If a CSS Modules class has no css style attached, it will be removed from the class attribute.

*Before*

```html
<style>
  .text-center { text-align: center; }
</style>

<p class="$style.blue text-center">My blue text</p>
```

*After*

```html
<style>
  .text-center { text-align: center; }
</style>

<p class="text-center">My blue text</p>
```

### Target any classname format

kebab-case or camelCase, name the classes the way you're more comfortable with.

*Before*

```html
<style>
  .red { color: red; }
  .red-crimson { color: crimson; }
  .redMajenta { color: magenta; }
</style>

<span class="$style.red">Red</span>
<span class="$style.red-crimson">Crimson</span>
<span class="$style.redMajenta">Majenta</span>
```

*After*

```html
<style>
  .red-2xTdmA { color: red; }
  .red-crimson-1lu8Sg { color: crimson; }
  .redMajenta-2wdRa3 { color: magenta; }
</style>

<span class="red-2xTdmA">Red</span>
<span class="red-crimson-1lu8Sg">Crimson</span>
<span class="redMajenta-2wdRa3">Majenta</span>
```

## Example

*Rollup Config*

```js
export default {
  ...
  plugins: [
    svelte({
      preprocess: [
        cssModules({
          includePaths: ['src'],
          localIdentName: '[hash:base64:10]',
        }),
      ]
    }),
  ]
  ...
}
```

*Svelte Component*

```html
<style>
  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    z-index: 10;
    width: 400px;
    height: 80%;
    background-color: #fff;
    transform: translateX(-50%) translateY(-50%);
  }

  section {
    flex: 0 1 auto;
    flex-direction: column;
    display: flex;
    height: 100%;
  }
  header {
    padding: 1rem;
    font-size: 1.2rem;
    font-weight: bold;
    border-bottom: 1px solid #d9d9d9;
  }

  .body {
    padding: 1rem;
    flex: 1 0 0;
    min-height: 0;
    max-height: 100%;
    overflow: scroll;
    -webkit-overflow-scrolling: touch;
  }

  footer {
    padding: 1rem;
    text-align: right;
    border-top: 1px solid #d9d9d9;
  }
  button {
    padding: .5em 1em;
    min-width: 100px;
    font-size: .8rem;
    font-weight: 600;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .cancel {
    background-color: #f2f2f2;
  }
</style>

<div class="$style.modal">
  <section>
    <header>My Modal Title</header>
    <div class="$style.body">
      <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit.</p>
    </div>
    <footer>
      <button>Ok</button>
      <button class="$style.cancel">Cancel</button>
    </footer>
  </section>
</div>
```

*Final html code generated by svelte*

```html
<style>
  ._329TyLUs9c {
    position: fixed;
    top: 50%;
    left: 50%;
    z-index: 10;
    width: 400px;
    height: 80%;
    background-color: #fff;
    transform: translateX(-50%) translateY(-50%);
  }

  section {
    flex: 0 1 auto;
    flex-direction: column;
    display: flex;
    height: 100%;
  }
  header {
    padding: 1rem;
    font-size: 1.2rem;
    font-weight: bold;
    border-bottom: 1px solid #d9d9d9;
  }

  ._1HPUBXtzNG {
    padding: 1rem;
    flex: 1 0 0;
    min-height: 0;
    max-height: 100%;
    overflow: scroll;
    -webkit-overflow-scrolling: touch;
  }

  footer {
    padding: 1rem;
    text-align: right;
    border-top: 1px solid #d9d9d9;
  }
  button {
    padding: .5em 1em;
    min-width: 100px;
    font-size: .8rem;
    font-weight: 600;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  ._1xhJxRwWs7 {
    background-color: #f2f2f2;
  }
</style>

<div class="_329TyLUs9c">
  <section class="svelte-1s2ez3w">
    <header class="svelte-1s2ez3w">My Modal Title</header>
    <div class="_1HPUBXtzNG">
      <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit.</p>
    </div>
    <footer class="svelte-1s2ez3w">
      <button class="svelte-1s2ez3w">Ok</button>
      <button class="_1xhJxRwWs7 svelte-1s2ez3w">Cancel</button>
    </footer>
  </section>
</div>
```

**Note:** The svelte scoped classes are still being applied to the html elements with a style.

## Why CSS Modules on Svelte

While the native CSS Scoped system should be largely enough to avoid class conflict, it could find its limit when working on a hybrid project. On a non full javascript front-end where Svelte is used to enhance pieces of the page, the thought on the class naming is no less different than the one on a regular html page. For example, on the modal component above, It would have been wiser to namespace some of the classes such as `.modal-body` and `.modal-cancel` to avoid inheriting styles from other `.body` and `.cancel`.

## License

[MIT](https://opensource.org/licenses/MIT)