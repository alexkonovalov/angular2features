# Angular 2 features

* [Annotations](#annotations)
  * [Directives](#directives)
  * [Components](#components)
* [Templates](#templates)
* [Change detection](#changedetection)
* [Core](#core)
* [DI](#di)
* [Forms](#forms)
* [HTTP](#http)
* [Pipes](#pipes)
* [Router](#router)
* [Test](#test)

## Annotations
### Directives
Directives allow you to attach behavior to elements in the DOM.

Directives are classes which get instantiated as a response to a particular DOM structure. By controlling the DOM structure, what directives are imported, and their selectors, the developer can use the [composition pattern](http://en.wikipedia.org/wiki/Object_composition) to get a desirable application behavior.

Directives are the cornerstone of an Angular application. We use Directives to break complex problems into smaller more reusable components. Directives allow the developer to turn HTML into a DSL and then control the application assembly process.

A directive consists of a single directive annotation and a controller class. When the directive's `selector` matches elements in the DOM, the following steps occur:

* For each directive, the *ElementInjector* attempts to resolve the directive's constructor arguments.
* Angular instantiates directives for each matched element using *ElementInjector* in a depth-first order, as declared in the HTML.

Here is a trivial example of a tooltip decorator. The directive will log a tooltip into the console on every time mouse enters a region:

``` javascript
@Directive({
  selector: '[tooltip]',     | CSS Selector which triggers the decorator
  properties: [              | List which properties need to be bound
    'text: tooltip'          |  - DOM element tooltip property should be
  ],                         |    mapped to the directive text property.
  host: {                    | List which events need to be mapped.
    (mouseover): 'show()'    |  - Invoke the show() method every time
  }                          |    the mouseover event is fired.
})                           |
class Form {                 | Directive controller class, instantiated
                             | when CSS matches.
  text:string;               | text property on the Directive Controller.
                             |
  show(event) {              | Show method which implements the show action.
    console.log(this.text);  |
  }
}
```

Example of usage:

``` html
<span tooltip="Tooltip text goes here.">Some text here.</span>
```

The developer of an application can now freely use the `tooltip` attribute wherever the behavior is needed. The code above has taught the browser a new reusable and declarative behavior.

Notice that data binding will work with this decorator with no further effort as shown below.

``` html
<span tooltip="Greetings {{user}}!">Some text here.</span>
```

**NOTE:** Angular applications do not have a main method. Instead they have a root Component. Dependency Injection then assembles the directives into a working Angular application.

###Components
A component is a directive which uses shadow DOM to create encapsulate visual behavior. Components are typically used to create UI widgets or to break up the application into smaller components.

* Only one component can be present per DOM element.
* Component's CSS selectors usually trigger on element names. (Best practice)
* Component has its own shadow view which is attached to the element as a Shadow DOM.
* Shadow view context is the component instance. (i.e. template expressions are evaluated against the component instance.)

Each Angular component requires a single `@Component` and at least one `@View` annotation. The `@Component` annotation specifies when a component is instantiated, and which properties and hostListeners it binds to.

When a component is instantiated, Angular

* Creates a shadow DOM for the component.
* Loads the selected template into the shadow DOM.
* Creates a child Injector which is configured with the appInjector for the Component.

Example of a component:

``` javascript
@Component({                      | Component annotation
  selector: 'pane',               | CSS selector on <pane> element
  properties: [                   | List which property need to be bound
    'title',                      |  - title mapped to component title
    'open'                        |  - open attribute mapped to component open property
  ],                              |
})                                |
@View({                           | View annotation
  templateUrl: 'pane.html'        |  - URL of template HTML
})                                |
class Pane {                      | Component controller class
  title:string;                   |  - title property
  open:boolean;

  constructor() {
    this.title = '';
    this.open = true;
  }

  // Public API
  toggle() => this.open = !this.open;
  open() => this.open = true;
  close() => this.open = false;
}
```

`pane.html`:
``` html
<div class="outer">
  <h1>{{title}}</h1>
  <div class="inner" [hidden]="!open">
    <content></content>
  </div>
</div>
```

`pane.css`:
``` css
.outer, .inner { border: 1px solid blue;}
.h1 {background-color: blue;}
```

Example of usage:
``` html
<pane #pane title="Example Title" open="true">
  Some text to wrap.
</pane>
<button (click)="pane.toggle()">toggle</button>

```
<!-- -------------------------- -->
### Templates
-----------------------------------
Templates are markup which is added to HTML to declaratively describe how the application model should be
projected to DOM as well as which DOM events should invoke which methods on the controller. Templates contain
syntax which is core to Angular and allows for data-binding, event-binding, template-instantiation.

The design of the template syntax has these properties:


* All data-binding expressions are easily identifiable. (i.e. there is never an ambiguity whether the value should be
  interpreted as string literal or as an expression.)
* All events and their statements are easily identifiable.
* All places of DOM instantiation are easily identifiable.
* All places of variable declaration are easily identifiable.

The above properties guarantee that the templates are easy to parse by tools (such as IDEs) and reason about by people.
At no point is it necessary to understand which directives are active or what their semantics are in order to reason
about the template runtime characteristics.

<!-- -------------------------- -->
### Change detection
-----------------------------------
<!-- -------------------------- -->
### Core
-----------------------------------
<!-- -------------------------- -->
### DI
-----------------------------------
<!-- -------------------------- -->
### Forms
-----------------------------------
<!-- -------------------------- -->
### HTTP
-----------------------------------
<!-- -------------------------- -->
### Pipes
-----------------------------------
<!-- -------------------------- -->

Pipes can be appended on the end of the expressions to translate the value to a different format. Typically used
to control the stringification of numbers, dates, and other data, but can also be used for ordering, mapping, and
reducing arrays. Pipes can be chained.

**NOTE:** Pipes are known as filters in Angular v1.

Pipes syntax is:

``` javascript
<div class="movie-copy">
  <p>
    {{ model.getValue('copy') | async | uppercase }}
  </p>
  <ul>
    <li>
      <b>Starring</b>: {{ model.getValue('starring') | async }}
    </li>
    <li>
      <b>Genres</b>: {{ model.getValue('genres') | async }}
    </li>
  <ul>
</div>
```

<!-- -------------------------- -->
### Router
-----------------------------------
<!-- -------------------------- -->
### Test
-----------------------------------
