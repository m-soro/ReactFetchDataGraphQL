# React Fetch Data with GraphQL

```jsx
/**
 * ---------------------------
 * FETCHING DATA FROM GRAPHQL
 * ---------------------------
 * Example of GraphQL
 * Snowtooth - A real GraphQL API for a fake ski resort
 * @link https://snowtooth.moonhighway.com/
 */

// Another component
function Lift({ name, elevationGain, status }) {
  return (
    <div>
      <h1>{name}</h1>
      <p>{elevationGain}</p>
      <p>{status}</p>
    </div>
  );
}

import "./App.css";
import { useState, useEffect } from "react";

// The custom GraphQL query this is a string
const query = `
query {
  allLifts {
    name
    elevationGain
    status
    
  }
} 
`;

// specify some options

const opts = {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ query }),
};

// useState to handle the data
// useEffect to make the call for the external data
function App() {
  let url = "https://snowtooth.moonhighway.com/";

  // CAN KEEP THE SAME DATA FETCHING LOGIC
  // JUST CHANGED THE RETURN

  // create container for the data
  const [data, setData] = useState(null);
  // adding these two states
  // create a value for error
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  // time to represent the above three to the component

  // call the api
  useEffect(() => {
    setLoading(true);
    // fetch is built into the browser. A way of making http request to get data from a source
    // once we get the data take that response and call .json() to turn it into json
    // fetch will take in the url and options
    fetch(url, opts)
      .then((response) => response.json())
      // then passs this json data to our data container
      .then(setData)
      // set the setLoading to false since we have the data already
      .then(() => setLoading(false))
      // if there si some sort of error set an Error
      .catch(setError);
    // pass an empty array to tell useEffect to only get data once
  }, []);

  // show a message if loading
  if (loading) return <h1>Loading...</h1>;
  // if error of any kind, return a pre-formatted json string to display
  if (error) return <pre>{JSON.stringify(error)}</pre>;
  // if there is no data return null
  if (!data) return null;

  // display the github user component if everything goes as expected
  // iterate through the data
  return (
    <div>
      <div>
        <h1>Fetch Data with GraphQL</h1>
      </div>
      {/* // for every iteration return a Lift component*/}
      {data.data.allLifts.map((lift) => (
        <Lift
          name={lift.name}
          elevationGain={lift.elevationGain}
          status={lift.status}
        />
      ))}
    </div>
  );
}

export default App;
```
