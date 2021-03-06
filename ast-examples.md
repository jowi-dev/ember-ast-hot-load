Examples 
------------------------------------------------------------------------------

[hbs-ast-builders](https://github.com/glimmerjs/glimmer-vm/blob/master/packages/%40glimmer/syntax/lib/builders.ts)

[glimmer hot-reload test #1](https://github.com/emberjs/ember.js/blob/master/packages/%40ember/-internals/glimmer/tests/integration/application/hot-reload-test.js)


[glimmer hot-reload test #2](https://github.com/emberjs/ember.js/blob/master/packages/%40ember/-internals/glimmer/tests/integration/application/hot-reload-test.js#L106
)

https://github.com/adopted-ember-addons/ember-cli-hot-loader

refs [astexporer](https://astexplorer.net/#/gist/9cdbd763be462d0b76ed6f442f62d5fe/b84f902de115f4cc32d43b9b4d9170067ed391b3)

```hbs 
<FooBar />
<Baz as |ctx|>
  <ctx.boo />
</Baz>
<Boo as |Foo|>
	<Foo />
</Boo>
```

to 

```hbs
{{#let (component 'foobar') as |FooBar|}}
  <FooBar />
{{/let}}
{{#let (component 'baz') as |Baz|}}
  <Baz as |ctx|>
    <ctx.boo />
  </Baz>
{{/let}}
{{#let (component 'boo') as |boo|}}
  <Boo as |Foo|>
	  <Foo />
  </Boo>
{{/let}}
```

```hbs
{{component 'foo-bar' a="1"}}
{{component (mut 1 2 3) b c d e="f"}}
{{foo-bar
  baz="stuff"
}}
{{#foo-boo}}
dsfsd
{{/foo-boo}}
{{doo-bar 1 2 3 hoo-boo name=(goo-boo "foo")}}
{{test-component}}
{{component @fooBar}}
{{component this.fooBar}}
{{component fooBar}}
{{component args.fooBar}}
```

will be transformed to 

```hbs
{{component (hot-load "foo-bar") a="1"}}
{{component (hot-load (mut 1 2 3)) b c d e="f"}}
{{component (hot-load "foo-bar") baz="stuff"}}
{{#component (hot-load "foo-boo")}}dsfsd
{{/component}}
{{component (hot-load "doo-bar") 1 2 3 hoo-boo name=(goo-boo "foo")}}
{{component (hot-load "test-component")}}
{{component (hot-load @fooBar)}}
{{component (hot-load this.fooBar)}}
{{component (hot-load fooBar)}}
{{component (hot-load args.fooBar)}}
```

and `hot-load` helper will manage component reloading
