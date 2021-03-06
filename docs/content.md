@typedef {can-stache.sectionRenderer} can-component/content <content>
@parent can-component.elements

Positions the `LIGHT_DOM` within a component's [can-component.prototype.template].

@signature `&lt;content>DEFAULT_CONTENT&lt;/content>`

When a user creates a new component in a template, the content between the tags is the
`LIGHT_DOM`.  For example, `Hello <b>World</b>` is the `LIGHT_DOM` in the following:

```
<my-tag>Hello <b>World</b></my-tag>
```

The `<content>` tag can be used within `my-tag` to position the `LIGHT_DOM`.  For
example, to position the `LIGHT_DOM` within an `<h1>`, `<my-tag>` could be defined like:

```
Component.extend({
	tag: "my-tag",
	template: stache("<h1><content/></h1>")
});
```

   @param {can-stache.sectionRenderer} [DEFAULT_CONTENT] The content that should be
   used if there is no `LIGHT_DOM` passed to the component.

   The following, makes `my-tag` show `Hi There!` if no `LIGHT_DOM` is passed:

   ```
   Component.extend({
   	tag: "my-tag",
   	template: stache("<h1><content>Hi There!</content></h1>")
   });
   ```
