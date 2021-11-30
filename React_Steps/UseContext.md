### 1. Creating a new Context
```JSX
import React from "react";

const CountContext = React.createContext();

export default CountContext;

```

### 2. Providing context
```JSX
import React, { useState } from "react";
import Child from "./Child";
import CountContext from "./countContext";

function CounterReadOnly() {
  const [num, setNum] = useState(0);
  function up(evt) {
    setNum(oldNum => oldNum + 1);
  }

  return (
    <CountContext.Provider value={num}>
      <button onClick={up}>+1 (from parent)</button>
      <Child />
    </CountContext.Provider>
  );
}

export default CounterReadOnly;


```

### 3. Consuming Context
```JSX
import React, { useContext } from "react";
import CountContext from "./countContext";

function GreatGrandReadOnly() {
  const num = useContext(CountContext);

  return (
    <div>
      <p>I'm a great-grandchild and here's the count: {num}.</p>
    </div>
  );
}

export default GreatGrandReadOnly;


```


