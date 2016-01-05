# Sharing behaviour between React components

React 0.13 introduced using plain Javascript classes to create components, 0.14 introduced "stateless functional components" as another method of defining them. Neither of these have an in-built method for behaviour sharing such as we had with the `mixins` option passed to `createClass`. So now we get to review mixins as a pattern for behaviour sharing and, if necessary, come up with something better. If they're not all bad, we'll need to figure out how to, or even if we can, add them to the two new component definition options.

## The problem

[Dan Abromov's medium article from Mar 2015](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.l0lztnsxd) makes the case for avoiding mixins.

For me (paraphrasing in the extreme), the key problems exposed are:
* mixins can unintentionally hide / distribute the source of behaviour
* mixins can unintentionally hide / distribute the source of some component state
* mixins look a lot like stand-alone component definitions but they don't act like them. See: the chaining of _some_ life-cycle methods as well as the merging of prop-types and initial-state objects.

Mixins make it easier to abstract the behaviour and state management of our components. However, this kind of abstraction is super hard and we can accidentally force readers of our code to play a frustrating game of hide-and-go-seek. Components can become a patch-work quilt of different methods, default props and initial state. Sure, this is a problem which can be avoided with **Discipline &#8482;** but maybe there's a way which encourages keeping related behaviours together a little more... obviously?

That's what this is going to be about: digging into alternative ways to share behaviour between React components. Let's see if we find something useful.

## Currently proposed solutions: nesting and higher order components

Higher order _functions_ are: "functions which take in other functions as input, or return a function as output". So higher order components are functions which take in React components as input and / or return a React component as output. With that in mind we can write a function which takes a component and returns a new component which renders the original one with some added behaviour.

It can look a little like this:

```js
const Loadable = (Component) => (props) =>
  props.loading ? <div>... loading</div> : <Component { ...props } />

const SomeComponent = (props) =>
  <div>{ props.data }</div>

const SomeOtherComponent = (props) =>
  <div>{ props.otherData }</div>

const LoadableComponent = Loadable(SomeComponent)
const AnotherLoadableComponent = Loadable(SomeOtherComponent)
```

Or, you know, more tersely:

```js
const Loadable = (Component) => (props) =>
  props.loading ? <div>... loading</div> : <Component { ...props } />

const SomeComponent = Loadable(
  (props) => <div>{ props.data }</div>
)

const SomeOtherComponent = Loadable(
  (props) => <div>{ props.otherData }</div>
)
```

This same result can be achieved with nesting:

```js
const Loadable = (props) =>
  props.loading ? <div>... loading</div> : props.children

const SomeComponent = (props) =>
  <Loadable>
    <div>{ props.data }</div>
  </Loadable>

const SomeOtherComponent = (props) =>
  <Loadable>
    <div>{ props.otherData }</div>
  </Loadable>
```

The last example might suggest to you that this isn't a totally new idea. You've probably seen examples of this before, say from React Router and `ReactCSSTransitionGroup`.

The first two examples above aren't as common and might look a little strange. Why on earth would we do that? Let's investigate.

What might happen to the last example if we wanted to pass a component responsible for rendering the loading state, you know, rather than just printing `... loading`? We could pass the component we want to render as a prop to `Loadable`:

```js
const Loadable = ({ loading, loadingState, children }) =>
  loading ? loadingState : children

const SomeComponent = (props) =>
  <Loadable loadingState={ <div>... loading (so much has changed!)</div> }>
    <div>{ props.data }</div>
  </Loadable>
```

What about this:

```js
const Loadable = (LoadingComponent, ContentComponent) => (props) =>
  props.loading ?
    <LoadingComponent { ...props } /> :
    <ContentComponent { ...props } />

const SomeComponent = Loadable(
  (props) => <div>... loading { props.resourceName }</div>,
  (props) => <div>{ props.data }</div>
)
```

What if multiple components use the same loading state?

```js
const Loadable = ({ loading, loadingState, children }) =>
  loading ? loadingState : children

const ResourceNameLoadingState = ({ resourceName }) =>
  <div>... loading { resourceName }</div>

const SomeComponent = (props) =>
  <Loadable loadingState={ <ResourceNameLoadingState /> }>
    <div>{ props.data }</div>
  </Loadable>

const AnotherComponent = (props) =>
  <Loadable loadingState={ <ResourceNameLoadingState /> }>
    <div>
      <h1>I'm special</h1>
      { props.differentData }
    </div>
  </Loadable>
```

```js
const Loadable = (LoadingComponent, ContentComponent) => (props) =>
  props.loading ?
    <LoadingComponent { ...props } /> :
    <ContentComponent { ...props } />

const ResourceNameLoadingState = ({ resourceName }) =>
  <div>... loading { resourceName }</div>

const SomeComponent = Loadable(
  ResourceNameLoadingState,
  (props) => <div>{ props.data }</div>
)

const AnotherComponent = Loadable(
  ResourceNameLoadingState,
  (props) =>
    <div>
      <h1>I'm special</h1>
      { props.differentData }
    </div>
)
```

The first of the last two examples is more readable to me. Something to do with loading is wrapping the rendering of my component and it takes a loading state. I get it, it's probably going to render the `loadingState` while the component is loading.

The second example is more obscure. This might have to do with the fact that we get more information from the name of the `loadingState` prop. "The first argument to the Loadable function" is less informative.

In this case we might prefer the `<WrappingComponent><my-component-markup /></WrappingComponent>` version. We need a name for these... something to do with "nesting". Matryoshkas?

### Another example: prop transforming

Let's think about another example. What if we need to consistently transform the props passed to a component. Say we want to filter the `data` props which could be passed to multiple list components.

```js
const filterBySearchTerm = (searchTermString, collection) => {
  const searchTermRgx = new RegExp(queryString.split('').join('.*'), 'gi')
  return collection.filter(
    (item) => item.match(searchTermRgx)
  )
}

const SearchFilterable  = (Component) => (props) => {
  const filteredData = filterBySearchTerm(props.searchTerm, props.data)

  return <Component { ...props } data={ filteredData } />
}

const SomeListComponent = SearchFilterable(
  (props = { data: [] }) =>
    <ul>{ data.map((datum) => <li>{ datum }</i>) }</ul>
)
```

This is a problem we encountered recently building the Great Southern Crossing Tour Tracker. We had three list components which could fetch their data independently but also needed to be search-filtered client side.

Let's take it a little further, what if on top of a search-filter we wanted a category filter as well?

```js
// imagine that SearchFilterable has been refactored to match the Regex against say a `name` key.

const fitlerByCategory = (category, collection) =>
  collection.filter((item) => item.category === category)

const CategoryFilterable = (Component) => (props) {
  const filteredData = fitlerByCategory(props.categoryFilter, props.data)

  return <Component { ...props } data={ filteredData } />
}

const SomeListComponent = CategoryFilterable(SearchFilterable(
  (props = { data: [] }) =>
    <ul>{ data.map((datum) => <li>{ datum }</i>) }</ul>
))
```

Hrm, that's pretty wonky. This is starting to look like a common problem though: `funcA(funcB(params))`. That's function composition. Let's see if it helps.

```js
const compose = (firstFunc, ...remainingFuncs) =>
  remainingFuncs.reduce((a, b) => (arg) => a(b(arg)), firstFunc)
```

We need all the functions we pass to compose to accept only one argument, also the return value of one function will be used as the input to the next. Therefore our functions need to accept a Component as their only argument and return a new Component. Looks like that's exactly what these already do :).

With `compose` it might look like this:

```js
const SomeListComponent = compose(
  CategoryFilterable,
  SearchFilterable
)(
  (props = { data: [] }) =>
    <ul>{ data.map((datum) => <li>{ datum }</i>) }</ul>
)

const SomeOtherListComponent = compose(
  CategoryFilterable,
  SearchFilterable
)(
  (props = { data: [] }) =>
    <ol>{ data.map((datum) => <li>{ datum }</i>) }</ol>
)
```

Because of how function composition works, we can make this even a little more generic:

```js
const SearchAndCatFilterable = compose(
  CategoryFilterable,
  SearchFilterable
)

const SomeListComponent = SearchAndCatFilterable(
  (props = { data: [] }) =>
    <ul>{ data.map((datum) => <li>{ datum }</i>) }</ul>
)

const SomeOtherListComponent = SearchAndCatFilterable(
  (props = { data: [] }) =>
    <ol>{ data.map((datum) => <li>{ datum }</i>) }</ol>
)
```

Bleh, it's on it's way I think. But it's still not there... Also, it's probably time to check back in on the problem.

Let's look at the first two issues. Does this address behaviour bleeding between mixins and the original component? Does this address state management bleeding?

In the above examples it's impossible for one wrapping component to call methods on any other component, they can only communicate via props. The same then goes for state, no component can directly update the state of any other component. They _can_ however mess about with props as they're being passed down. Is that easier or harder to reason about?

The last issue is about mixins being "a lot like" component definitions. In all of the above examples they _are_ component definitions. They can be made with `createClass`, as ES6 classes, as functions, it makes no difference.
