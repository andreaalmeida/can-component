@property {can-stache.renderer} [can-component.prototype.template] template
@parent can-component.prototype

Provides a template to render directly within the component's tag. The template is rendered with the
component's [can-component::ViewModel] instance.  [can-component/content] elements within the template are replaced by the source elements within the component's tag.

@option {can-stache.renderer} A [can-stache.renderer] returned by [can-stache]. For example:

    Component({
      tag: "my-tabs",
      template: stache("<ul>{{#panels}}<li>{{title}}</li> ...")
    });


@body


## Use

The template specified by the `template` property works similar to
the [http://www.w3.org/TR/shadow-dom/ W3C Shadow DOM proposal]. It represents the contents
of a custom element, while being able to reposition the user provided __source__ elements
with the [can-component/content] tag.

There are three things to understand about a [can-component]'s template:

 - It is inserted into the component's tag.
 - It is rendered with access to the component instance's viewModel.
 - [can-component/content] tags within the template act as insertion points for the source elements.

The following example demonstrates all three features:

@demo demos/can-component/my_greeting_full.html

The following explains how each part works:

__Component:__

```js
Component({
	tag: "my-greeting",
	template: stache("<h1><content/></h1>"),
	ViewModel: DefineMap.extend({
		title: {
			value: "can-component"
		}
	}),
	leakScope: true
});
```

This registers a component for elements like `<my-greeting>`. Its template
will place an `<h1>` element directly within `<my-greeting>` and put
the original contents of `<my-greeting>` within the `<h1>`. The component's
[can-component::ViewModel] adds a title value.

__Source template:__

    <header>
      <my-greeting>
         {{site}} - {{title}}
      </my-greeting>
    </header>

The source template is the template that
uses `<my-greeting>`.  In the demo, this is defined within a `<script>`
tag.

Notice:

 - There is content within `<my-greeting>`.
 - The content looks for a `site` and `title` value.

__Source data:__

		stache("...")({
			site: "CanJS"
		})

This is how we render the source template that uses `<my-greeting>`. The template is rendered with `site` in its [can-component::ViewModel].

__HTML Result:__

    <header>
      <my-greeting>
        <h1>CanJS - can-component</h1>
      </my-greeting>
    </header>

This is the result of the template transformations. The
content within the original `<my-greeting>` is placed within the `<h1>`
tag.  Also, notice that the original content is able to access data from
the source data and from the component's viewModel.

The following sections break this down more.


## Template insertion

The template specified by template is rendered directly withing the custom tag.

For example the following component:

    Component({
      tag: "my-greeting",
      template: stache("<h1>Hello There</h1>")
    });

With the following source html:

    <header>
      <my-greeting></my-greeting>
    </header>

Produces the following html:

    <header>
      <my-greeting><h1>Hello There</h1></my-greeting>
    </header>

However, if there was existing content within the source html, like:

    <header>
      <my-greeting>DO REMOVE ME!!!</my-greeting>
    </header>

that content is removed, and replaced by the component's template:

    <header>
      <my-greeting><h1>Hello There</h1></my-greeting>
    </header>

### The `<content>` element

Use the `<content>` element to place the source content in the
component's element within the component's
template. For example, if we change the component to look like:

    Component({
      tag: "my-greeting",
      template: stache("<h1><content/></h1>")
    });

and rendered with source html, like:

    <my-greeting>Hello World</my-greeting>

it produces:

    <my-greeting><h1>Hello World</h1></my-greeting>

### `<content>` element default content

If the user does not provide source content, the html
between the `<content>` tags will be used. For example, if we
change the component to look like:

    Component({
      tag: "my-greeting",
      template: stache("<h1><content>Hello World</content></h1>")
    });

and rendered with source html like:

    <my-greeting></my-greeting>

it produces:

    <my-greeting><h1>Hello World</h1></my-greeting>
