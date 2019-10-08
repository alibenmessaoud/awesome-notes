### Functional techniques

##### Currying and Partial Application

The more significant reason to use currying is that we can make use of partial application. The idea behind it is to supply only some of the required arguments to a function. This returns a new function requiring only the remaining arguments.

```javascript
const createAnimal = animal => event => {
    setAnimals([...new Set([...animals, animal])]); // Set() ensures uniqueness
    console.log("Mouse click at: " + event.clientX + ", " + event.clientY);
};
```

##### Lazy evaluation

Lazy evaluation is a concept in functional programming that means that code is only evaluated when necessary. 

##### Short-circuit Evaluation

Expressions using boolean operators can “short-circuit” if the first argument determines the value of the whole expression. Technically, JavaScript is a strict (not lazy) language but it does have short-circuit evaluation for boolean operators. 

```javascript
{props.animals.includes("Cat") ? <span>😸</span> : null}
// vs
{props.animals.includes("Dog") && <span>🐶</span>}
```

##### Principales : Refactor-driven development

Make it work, then make it better. Start small!

##### Single element pattern

Set of rules/best practices to create consistent, reliable and maintainable components.

Rule #1: Render only one element

Rule #2: Never break the app

Rule #3: Render all HTML attributes passed as props

```javascript
const Avatar = props => <img className="avatar" {...props} />;

Avatar.propTypes = {
  src: PropTypes.string.isRequired,
  alt: PropTypes.string.isRequired
};

<Avatar src={profile.photoUrl} alt={profile.photoAlt} />
```

Rule #4: Always merge the styles passed as props

```javascript
const Avatar = ({ className, style, ...props }) => (
  <img 
    className={'avatar ${className}'}
    style={{ borderRadius: "50%", ...style }}
    {...props} 
  />
);

Avatar.propTypes = {
  src: PropTypes.string.isRequired,
  alt: PropTypes.string.isRequired,
  className: PropTypes.string,
  style: PropTypes.object
};

<Avatar
  className="my-avatar"
  style={{ borderWidth: 1 }}
/>
```

Rule #5: Add all the event handlers passed as props

```javascript
const callAll = (...fns) => (...args) => fns.forEach(fn => fn && fn(...args));

const internalOnLoad = () => console.log("loaded");

const Avatar = ({ className, style, onLoad, ...props }) => (
  <img 
    className={'avatar ${className}'}
    style={{ borderRadius: "50%", ...style }}
    onLoad={callAll(internalOnLoad, onLoad)}
    {...props} 
  />
);

Avatar.propTypes = {
  src: PropTypes.string.isRequired,
  alt: PropTypes.string.isRequired,
  className: PropTypes.string,
  style: PropTypes.object,
  onLoad: PropTypes.func
};
```

Suggestion #1: Avoid adding custom props

> Always try to create a new single element — such as AvatarRounded — which renders Avatar and modifies it, rather than adding a custom prop.

Suggestion #2: Receive the underlying HTML element as a prop

> 

Validate by using `npm install --global singel`

##### The Iceberg of React Hooks

```javascript
 const [count, setCount] = useState(0);
```

Most side-effects happen inside useEffect. The default behavior of useEffect is to run after every render, so new interval will be created every time count changes. 

Giving [] as second argument to useEffect will call function once, after mount. 

To prevent resource leaks, everything must be disposed when lifecycle of a hook ends. In this case returned function will be called after component unmounts.

Giving array of dependencies to useEffect will change its lifecycle. useEffect will be called once after mount and every time the dependant variables change. Cleanup function will be called every time changes to dispose previous resource.

##### React Hooks Radar : level of usage with caution

Green: safe to use almost everywhere without much thinking.

- useReducer
- useState
- useContext

Yellow:  provide useful performance optimizations.

- useCallback
- useMemo

Red: interact with mutable world using side effects.

- useRef
- useEffect
- useLayoutEffect

##### React Hooks Checklist

1. Obey Rules of Hooks : https://reactjs.org/docs/hooks-rules.html 
2. Don’t do any side-effects in main render function.
3. Unsubscribe/dispose/destroy all used resources.
4. Prefer useReducer or functional updates for useState to prevent reading and writing same value in a hook.
5. Don’t use mutable variables inside render function, use useRef instead.
6. If what you save in useRef has smaller lifecycle than the component itself, don’t forget to unset the value when disposing the resource.
7. Be cautions with infinite recursion and resource starvation.
8. Memoize functions and objects when needed to improve performance.
9. Correctly capture input dependencies (undefined => every render, [a, b] => when a or b change, [] => only once).
10. Use customs hooks for non-trivial use-cases.