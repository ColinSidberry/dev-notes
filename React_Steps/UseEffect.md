### 1. Changing Non-React DOM After State Change
```JSX
import React, { useState, useEffect } from "react";

/** Demo of a simple effect: changes doc title */

function EffectExample() {
  const [num, setNum] = useState(0);

  useEffect(function setTitleOnRerender() {
    document.title = `WOW${"!".repeat(num)}`;
  }, [num]);

  function increment(evt) {
    setNum(n => n + 1);
  }

  return (
      <button onClick={increment}>Get More Excited!</button>
  );
}

```

### 2. Handling Errors in Ajax Request
```JSX
function ProfileViewer() {
  const [profile, setProfile] = useState(null);
  const [error, setError] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(function fetchUserWhenMounted() {
    async function fetchUser() {
      try {
        const userResult = await axios.get(URL);
        setProfile(userResult.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    }
    fetchUser();
  }, []);

  if (isLoading) return <i>Loading...</i>
  else if (error) return <b>Oh no! {error}</b>
  else return ( <div>... stuff using data ...</div> )
}

```

### 3. Updating after dependency changes
```JSX
import React, { useState, useEffect } from "react";
import axios from "axios";
import ProfileSearchForm from "./ProfileSearchForm";

const BASE_URL = "https://api.github.com/users";

/** GitHub Profile Component --- shows info from GH API */

function ProfileViewerWithSearch() {
  const [username, setUsername] = useState("elie");
  const [profile, setProfile] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(function fetchUserOnUsernameChange() {
    async function fetchUser() {
      const userResult = await axios.get(`${BASE_URL}/${username}`);
      setProfile(userResult.data);
      setIsLoading(false);
    }
    fetchUser();
  }, [username]);

  function search(username) {
    setUsername(username);
  }

  if (isLoading) return <i>Loading...</i>

  return (
      <div>
        <ProfileSearchForm search={search} />
        <b>{profile.name}</b>
      </div>
  );
}

export default ProfileViewerWithSearch;

```

