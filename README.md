# React-Router

### Installation

``` 
npm install react-router-dom 
```

### Setup in React Project

-  Wrap the App with browser Router has shown below.

```
ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);

```

- return like this App.js

```
return (
    <Routes>
      <Route path="/login" Component={LoginPage}></Route>
      <Route path="/" Component={Home}></Route>
    </Routes>
  );

```

### Difference between Element and Component

- Element
```
<Route path="/login" element={<LoginPage></LoginPage>}></Route>
```

- Component

```
<Route path="/" Component={Home}></Route>
 ```


### Conditional Rendering in same url
- Element

``` 
<Route path="/login" element={loggedInStatus?<LoginPage></LoginPage>:"Hello"}></Route> 
```

- Component- Should only provide/render component neither JSX nor <></>

``` 
<Route path="/login" Component={loggedInStatus?LoginPage:Home}></Route>
```


### Redirection conditionally

- Element

``` 
const [loggedInStatus, setLoggedInStatus] = useState(false);

 useEffect(() => {
    const checkIsLoggedIn = async () => {
      const isLoggedIn = getCookiesDetails();
      console.info(isLoggedIn)
      if (!isLoggedIn) return setLoggedInStatus(false);
      try {
        const result = await loggedInCheckApi();
        setLoggedInStatus(result);
      } catch (error) {
        console.error("Error while checking loggedin status:", error);
        alert("Something went wrong while checking loggedin status");
      }
    };

    checkIsLoggedIn();
  }, []);

  return (
    <Routes>
      <Route path="/login" element={<LoginPage />} />
      <Route
        path="/"
        element={loggedInStatus ? <Home /> : <Navigate to="/login" />}
      />
    </Routes>
  );
```


- Component

``` 
<Routes>
      <Route path="/login" element={<LoginPage />} />
      <Route
        path="/"
        component={() =>
          loggedInStatus ? <Home /> : <Navigate to="/login" />
        }
      />
    </Routes>
```


