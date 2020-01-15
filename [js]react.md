## React Context API

Prop Drilling In React

### Example 1

#### Provide The Context

To create a context, we need to initialize it:

```javascript
export const MyContext = React.createContext(defaultValue);

<MyContext.Provider value={{ user: 'Guest' }}>
  <View>
    // Deep nested
    <ChildComponent />
  </View>
</MyContext.Provider>
```

#### Consume The Context

```javascript
// ChildComponent.js
import { MyContext } from './MyContext.js'

function ChildComponent() {
  return (
    <MyContext.Consumer>
      {({ user }) => {
        // user is equal to 'Guest' declared above
        return <p>Hello, {user}</p>
      }}
    </MyContext.Consumer>
  );
}

//////

function ChildComponent() {
  const context = React.useContext(MyContext);
  return <p>Hello, {context.user}</p>;
}

//////

class ChildComponent extends React.Component {
  render() {
    return <p>Hello, {this.context.user}</p>;
  }
}

ChildComponent.contextType = MyContext;
```

### Example 2

```javascript
import { setLabelAction, setIsCheckedAction } from './actions';

export const ActionContext = React.createContext();

function Controller() {
  const actions = {
    setLabel: (label) => setLabelAction(label),
    setIsChecked: (isChecked) => setIsCheckedAction(isChecked),
  };

  return (
    <ActionContext.Provider value={actions}>
      <DataContainer>
        <View>
          <MoreView />
          ...
    </ActionContext.Provider>
  );
}
```

```javascript
import { ActionContext } from './Controller.js'

export function MoreView() {
  const actions = React.useContext(ActionContext);

  return <button onClick={() => actions.setIsChecked(true)}>Check</button>;
}
```

## General

```javascript
//	Title.jsx 
function Title(props) {
	return <h1> {props.text} </h1>;
}

Title.propTypes	= {
	text: PropTypes.string
}; 

Title.defaultProps = { 
	text: 'Hello world'
};

//	App.jsx 
function App() {
	return <Title text = 'Hello	React' / > ;
}
```

```javascript
function SomethingElse({ answer }) { 
	return <div>The	answer is { answer }</div>; 
}
function Answer() { 
	return <span>42</span>; 
}
//	later	somewhere	in	our	application 
<SomethingElse answer={ <Answer /> } />
```

```javascript
// There is also a props.children property that gives us access	to	the	child components passed	by the owner of the	component

function Title({ text, children }) {
    return (
        <h1>
            {text}
            {children}
        </h1>
    );
}

function App() {
    return (
        <Title text="Hello	React">
            <span>community</span>{" "}
        </Title>
    );
}
```

```javascript
// Function	as	a	children, render	prop

function UserName({ children }) {
  return (
    <div>
      <b>{children.lastName}</b>, {children.firstName}{" "}
    </div>
  );
}

function App() {
  const user = { firstName: "Krasimir", lastName: "Tsonev" };
  return <UserName>{user}</UserName>;
}
```

```javascript
function TodoList({ todos, children }) {
  return (
    <section className="main-section">
      <ul className="todo-list">
        {todos.map((todo, i) => (
          <li key={i}>{children(todo)}</li>
        ))}
      </ul>
    </section>
  );
}

function App() {
  const todos = [
    { label: "Write	tests", status: "done" },
    { label: "Sent	report", status: "progress" },
    { label: "Answer	emails", status: "done" },
  ];
  const isCompleted = (todo) => todo.status === "done";
  return (
    <TodoList todos={todos}>
      {(todo) => (isCompleted(todo) ? <b>{todo.label}</b> : todo.label)}
    </TodoList>
  );
}
```

Presentational components

-  Mainly concerned with how things look.
-  Have no major dependencies on the rest of the app.
-  No connection with the specification of the data that is outside the component.
-  Mainly receives data and callbacks exclusively via props.

```javascript
const Users = (props) => (
  <ul>
    {props.users.map((user) => (
      <li>{itr}</li>
    ))}
  </ul>
);
```

Container components

-  Mainly concerned with how things work.
-  May contain both presentational and container component but does not have DOM and markup of their own.
-  Provide the data and behavior to presentational or other container components.
-  Call flux actions and provides these as callbacks to the presentational component.

```javascript
class UsersContainer extends React.Component {
  constructor() {
    this.state = {
      itr: [],
    };
  }

  componentDidMount() {
    axios.get("/users").then((itr) => this.setState({ users: itr }));
  }
  render() {
    return <Users users={this.state.itr} />;
  }
}
```

Dependency	injection

```javascript
//	Title.jsx
export default function Title(props) {
  return <h1>{props.title}</h1>;
}
//	Header.jsx
import Title from "./Title.jsx";
export default function Header() {
  return (
    <header>
      <Title />
    </header>
  );
}
//	App.jsx
import Header from "./Header.jsx";
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { title: "React	in	patterns" };
  }
  render() {
    return <Header />;
  }
}

// However,	this	may	work	for	these	three components	but	what happens	if	there	are	multiple properties	and	deeper	nesting.	Lots	of	components	will act	as	proxy	passing	properties	to	their	children.

//	inject.jsx
const title = "React	in	patterns";
export default function inject(Component) {
  return class Injector extends React.Component {
    render() {
      return <Component {...this.props} title={title} />;
    }
  };
}
//	----------------------------------//	Header.jsx
import inject from "./inject.jsx";
import Title from "./Title.jsx";
var EnhancedTitle = inject(Title);
export default function Header() {
  return (
    <header>
      <EnhancedTitle />
    </header>
  );
}
```

