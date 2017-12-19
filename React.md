## React

#### Error boundaries

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed.

A class component becomes an error boundary if it defines a new lifecycle method called `componentDidCatch(error, info)`:

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

Then you can use it as a regular component:

```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

#### Using CSS Module in create-react-app project

First run `npm run eject` to get access to webpack config file.

Go to `webpack.config.dev.js` and `webpack.config.prod.js` and add CSS modules feature in css-loader.

```
{
  loader: require.resolve('css-loader'),
  options: {
    modules: true,
    localIdentName: '[path][name]__[local]--[hash:base64:5]'
  }
}
```

Now you can import the CSS file `import styles from './App.css'` and use .title style in your code as if you are dealing with an object `<div className={styles.title}>test</div>`.

To create a global CSS style, you can add a `:global` prefix such as `:global .post { ... } `, then you can use `className="post"` anywhere in your app and receive that styling.

#### Avoid mutating states directly

```
// copy an array in the state using slice() or spread operator
const persons = this.state.persons.splice();
const persons = [...this.state.persons];

// copy an object in the state using spread operator or Object.assign()
const person = {...this.state.persons[personIndex]};
const person = Object.assign({}, this.state.persons[personIndex]);
```

#### Passing method reference with pre-defined parameters to child component

We have a handler method with one expected parameter that need to pass to a child component

```
deletePersonHandler = (personIndex) => {
  // copy the current state using slice() or spread operator
  const persons = this.state.persons.splice();
  const persons = [...this.state.persons];
  
  // remove a person from the array based on its index
  persons.splice(personIndex, 1);
  this.setState({ persons: persons })
}
```

We can use the `bind()` method to pass a pre-defined parameter to child component

```
// use bind() to bind the context as well as the index
{this.state.persons.map((person, index) => {
  return <Person
    deletePerson={this.deletePersonHandler.bind(this, index)}
    key={person.id}
    name={person.name} />
})}
```