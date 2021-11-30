### 1. Map out component heirarcy
  -  App -> BoxList -> (Box, NewBoxForm)
  -  What components can be generalized?
  -  What state/props go where?


### 2. Create App/Install
    
    `npx create-react-app`
    `npm install react-router-dom`
    `npm start`



### 3. App Component (for Routing)
```JSX
import React from "react";
import Nav from "./Nav";
import Routes from "./Routes";
import { BrowserRouter } from 'react-router-dom';

/** Order entering system before it ships.
 *
 * Props:
 * - orderId
 * - price (before tax)
 * - salespersonId
 *
 * State:
 * - isConfirmed: true/false
 *
 * Customer -> Order -> OrderItem
 */
function App() {
  return (
    <div>
      <BrowserRouter>
        <Nav />
        <Routes />
      </BrowserRouter>
    </div>
  );
}

export default App;

```

### 4. Route Component

    ```JSX
    import React from "react";
    import { Route, Switch, Redirect } from "react-router-dom";
    import Home from "./Home";
    import About from "./About";
    import ContactRedirect from "./ContactRedirect";
    import BlogHome from "./BlogHome";
    import Post from "./Post";

    /** Order entering system before it ships.
   *
   * Props:
   * - orderId
   * - price (before tax)
   * - salespersonId
   *
   * State:
   * - isConfirmed: true/false
   *
   * Customer -> Order -> OrderItem
   */
    function Routes() {
    return (
        <Switch>
        <Route exact path="/about"><About /></Route>
        <Route exact path="/contact"><ContactRedirect /></Route>
        <Route exact path="/blog/:slug"><Post /></Route>
        <Route exact path="/blog"><BlogHome /></Route>
        <Route exact path="/"><Home /></Route>
        <Redirect to="/" />{/*include push if successful*/}
        {/*Or use a NotFound Component*/ }
        </Switch>
    );
    }

    export default Routes;

    ```

### 5. Nav Bar
    ```JSX
    import React from "react";
    import { NavLink } from "react-router-dom";
    import "./NavBar.css";

    function NavBar() {
    return (
        <nav className="NavBar">
        <NavLink exact to="/">
            Home
        </NavLink>
        <NavLink exact to="/eat">
            Eat
        </NavLink>
        <NavLink exact to="/drink">
            Drink
        </NavLink>
        </nav>
    );
    }

    export default NavBar;

    ```

### 6. Active Nav CSS

```CSS
.NavBar {
  background-color: tomato;
  text-align: center;
  font-weight: bold;
}

.NavBar a {
  padding: 0.5em 0;
  width: 10em;
  display: inline-block;
  color: white;
  text-decoration: none;
}

.NavBar a:hover {
  background-color: black;
  color: white !important;
}

.NavBar a.active {
  font-weight: bold;
  color: black;
}
```

### 7. Pulling Params in Page
```JSX
import { useParams } from "react-router-dom";

function Food() {
  const { name } = useParams();
  return (
    <div>
      <h1>You must like {name}.</h1>
    </div>
  );
}
export default Food;
```



### Form
```JSX
import React, { useState } from "react";

/** Form for editing names.
 *
 * State:
 * - formData: { firstName, lastName }
 */

//Note: Form
function NameForm() {
  //1a. Handle Forms with one or two inputs-----------------------------------
  const INITIAL_DATA = ""
  const [fullName, setFullName] = useState(INITIAL_DATA);

  function handleChange(evt) {
    setFullName(evt.target.value);
  }
  
  //1b. Handle Form with multiple inputs--------------------------------------
  const INITIAL_DATA = {
    firstName: "",
    lastName: "",
  }
  const [formData, setFormData] = useState(INITIAL_DATA);
  
  function handleChange(evt) {
    const { name, value } = evt.target;
    setFormData(fData => ({
      ...fData,
      [name]: value,
    }));
  }

  //2. Needed for both-------------------------------------------------------
  function handleSubmit(evt) {
    evt.preventDefault();
    setFormData(INITIAL_DATA);
    console.log("Check out state ->", formData);
    // do something with the data submitted
  }

  return (
      <form onSubmit={handleSubmit}>
        <label htmlFor="firstName">First:</label>
        <input
            id="firstName"
            name="firstName"
            value={formData.firstName}
            onChange={handleChange}
        />

        <label htmlFor="lastName">Last:</label>
        <input
            id="lastName"
            name="lastName"
            value={formData.lastName}
            onChange={handleChange}
        />
        <button>Add a new person!</button>
      </form>
  );
}
// end

export default NameForm;

```

### useState Examples
```JSX
//Dogs examples from 10/18
import React, { useState } from "react";
import axios from "axios";
import { BrowserRouter } from "react-router-dom";

import Routes from "./Routes";
import NavBar from "./NavBar";

/**
 * App
 *
 * state:
  * dogs: [{name...}]
  * isLoading: bool
 *
 * props: none
 *
 * App -> Routes
 *
 */

function App() {
  const [dogs, setDogs] = useState([]);
  const [isLoading, setIsLoading] = useState(true)

  async function loadDogs(){
    const response = await axios.get("http://localhost:5000/dogs")
    setDogs(response.data)
    setIsLoading(false)
  }

  if (isLoading) {
    loadDogs();
    return <h1>Loading...</h1>
  }

  return (
    <div>
      <h1>Welcome!</h1>
      <BrowserRouter>
        <NavBar dogs={dogs} />
        <div className="container">
          <Routes dogs={dogs} />
        </div>
      </BrowserRouter>
    </div>
  );
}

export default App;


//---------------------------------
import { useState } from "react";

//Note: useState using map (1)
//old state depends on prev
function zeroFirst() {
    setDice(curr => curr.map(
        (val, i) => i === 0 ? 0 : val));
}

//Note: Nested State (2)
function reroll(pos) {
    setDice(curr =>
        curr.map((die, i) =>
            i === pos
                ? { ...die, val: d6() } //spread and specify what needs to change
                : die
        ));
}
```

### Conditional DOM Element
```JSX
  {final===21 && <p>BlackJack</p>}
  //or
  {boolean ? <TruthyComponent/> : <FalseyElement/>}
```

## Raw Notes-------------------------------------

```JSX
```