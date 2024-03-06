# React-Router

### Installation

```
npm install react-router-dom 
```

### Setup in React Project

- Wrap the App with browser Router has shown below.

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

- Element to render JSX

```
<Route path="/login" element={loggedInStatus?<LoginPage></LoginPage>:"Hello"}></Route> 
```

- Component- Should only provide/render component neither JSX nor <></>

```
<Route path="/login" Component={loggedInStatus?LoginPage:Home}></Route>
```

### Redirection conditionally

```
function App() {
  return (
    <Routes>
      <Route path="/login" element={<LoginPage />} />
      <Route path="/" element={<PrivateRoute component={Home} />} />
    </Routes>
  );
}
```

#### PrivateRoute Implementation

```
const PrivateRoute = ({ component: Component, ...rest }) => {
  const [isAuthenticated, setIsAuthenticated] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      // Do some condition check
      setIsAuthenticated(.....);
    };

    fetchData();
  }, []);

  if (isAuthenticated === null) {
    // You can optionally render a loading spinner or message while waiting for authentication check
    return <div>Loading...</div>;
  }

  return isAuthenticated ? <Component {...rest} /> : <Navigate to="/login" />;
};

export default PrivateRoute;
```

### Link

- Similar To anchor Tag but it will not refresh the page
- Use this to navigate through URL without page Refresh.

```
<Link to="/home"> <li>Home</li> </Link> 
```

### Router Params

``` 
<Route path="/vans/:id/:type" element={<VansDetailedPage/>}></Route>
```
To Access the Id in the VanDetailedPage component:
``` 
import {useParams} from 'react-router-dom';

// inside component
const params= useParams();
console.log(params.id) /// will give you the id
console.log(params.type) /// will give you the type
```

### Nested Routes
- Nested Urls: 
     - /vans
     - /vans/types

- Shared Ui:
    - When you want to share the common Ui between the same routes.

#### Layout Route:

- Used in senarios where you want the certain component through out the Routes something like NavBar or Footer.

-  Create a NavBar.jsx file and have your NavBar code there and also create a Layout.jsx file.
- Wrap the all the Routes with Single Parent Route to create a parent child relationship.

``` 
<Route element={<Layout/>}>
<Route path="/" element={<Home/>}></Route>
<Route path="/vans" element={<Vans/>}></Route>
<Route path="/about" element={<About/>}></Route>
</Route>
```

#### Outlet:


- This will take the child route under the parent route and render on the screen when url matches.

``` 
import {Outlet} from 'react-router-dom'

export const Layout=()=>{
  return(
    <div>
      <NavBar/>
      <Outlet/>
    </div>
  )
}
```

#### Deep Nested Route:

![Alt text](image.png)

- Something like above.

``` 
<Route element={<Layout/>}>
<Route path="/" element={<Home/>}></Route>
<Route path="/vans" element={<Vans/>}></Route>
<Route path="/about" element={<About/>}></Route>

<Route path="/host" element={<HostLayout/>}>

<Route path="/host" element={<Dashboard/>}></Route> (donot use this,use Index Route to Fix)
<Route path="/host/income" element={<Income/>}></Route>
<Route path="/reviews" element={<Reviews/>}></Route>

</Route>
</Route>
```

#### Relative Routes:

- You donot need actually need to have that / at each level of path.
- Here if you remove the / for the path it would look for the parent Route if it exist if not it would consider it as the root path.

``` 
<Route element={<Layout/>}>
        <Route path="/" element={<Home/>}></Route>
        <Route path="vans" element={<Vans/>}></Route>
        <Route path="about" element={<About/>}></Route>

        <Route path="host" element={<HostLayout/>}>
            <Route path="host" element={<Dashboard/>}></Route> (donot use this,use Index Route to Fix)
            <Route path="income" element={<Income/>}></Route>
            <Route path="reviews" element={<Reviews/>}></Route>
         </Route>
</Route>
```

#### Index:
- putting index will say that it should also be loaded with the root level path intial
``` 
<Route path="/" element={<Layout/>}>
      <Route index element={<Home/>}></Route>
      <Route path="vans" element={<Vans/>}></Route>
      <Route path="about" element={<About/>}></Route>

      <Route path="host" element={<HostLayout/>}>
              <Route index element={<Dashboard/>}></Route> 
              <Route path="income" element={<Income/>}></Route>
              <Route path="reviews" element={<Reviews/>}></Route>
      </Route>
</Route>
```

### NavLink

- Used for visual feedback the you opted for the path.
- Similar to Link but with extra features.


![Alt text](image-1.png)

``` 
import {NavLink} from 'react-router-dom';

<NavLink to='/' className={({isActive})=>isActive?"activeLinkCSS":notActiveCSS}>Home</NavLink>
<NavLink to='/about'>About</NavLink>
<NavLink to='/contact'>Contact</NavLink>
```

OR

``` 
const activeStyle={
  fontWeight:"bold",
  textDecoration:"underline",
  color:red;
}

<NavLink to='/' style={({isActive})=>isActive?"activeStyle":null}>Home</NavLink>

```

#### End
- let say you have route something like this when you apply navLink isActive property it will apply style for Dashboard and income if url as income. this is because of the common path.
``` 
<Route path="host" element={<HostLayout/>}>
              <Route index element={<Dashboard/>}></Route> 
              <Route path="income" element={<Income/>}></Route>
              <Route path="reviews" element={<Reviews/>}></Route>
      </Route>
```
- To stop this apply this to your Navlink indexed route.

``` 
<NavLink end to='/host' style={({isActive})=>isActive?"activeStyle":null}>Home</NavLink>
```

### Relative Links:

- when you have routes something like this.

``` 
<Route path="/" element={<Layout/>}>
      <Route index element={<Home/>}></Route>
      <Route path="vans" element={<Vans/>}></Route>
      <Route path="about" element={<About/>}></Route>

      <Route path="host" element={<HostLayout/>}>
              <Route index element={<Dashboard/>}></Route> 
              <Route path="income" element={<Income/>}></Route>
              <Route path="reviews" element={<Reviews/>}></Route>
      </Route>
</Route>
```

- in the navlinks Area you donot need to do like this.

``` 
<NavLink to='/host' className={({isActive})=>isActive?"activeLinkCSS":notActiveCSS}>Home</NavLink>
<NavLink to='/host/income'>About</NavLink>
<NavLink to='/host/about'>Contact</NavLink>
```

INTO

``` 
<NavLink to='/host' className={({isActive})=>isActive?"activeLinkCSS":notActiveCSS}>Home</NavLink>
<NavLink to='income'>About</NavLink>
<NavLink to='about'>Contact</NavLink>

```

- this working becoz if you see.

``` 
<Route path="host" element={<HostLayout/>}></Route>
```
- is the parent of all the path so react router will consider this as path before each child path.

#### Current Route ( . ):
- It is suggested to use this approach for the indexed child route under the parent which shares same URL's.


FROM THIS
``` 
<NavLink to='/host' className={({isActive})=>isActive?"activeLinkCSS":notActiveCSS}>Home</NavLink>
```

TO THIS

``` 
<NavLink to='.' className={({isActive})=>isActive?"activeLinkCSS":notActiveCSS}>Home</NavLink>
```

### Relative Path

- This is used for the going back one level backward the path.

- imagine you have route something like this.
``` 
<Route path="host" element={<HostLayout/>}>
              <Route index element={<Dashboard/>}></Route> 
              <Route path="income" element={<Income/>}></Route>
              <Route path="reviews" element={<Reviews/>}></Route>
              <Route path='/vans' element={<HostVans/>}></Route>
              <Route path="/vans/:id" element={<HostVanDetails/>}></Route>
      </Route>
``` 

- HostVanDetails is component that would show you the details of one van and now in that component you need to have button that will make go back to /van where it has list of vans.

- if we have used this.

``` 
// Inside HostVanDetails

<Link to='..'>Go Back</Link>
```
- This would go back to the relative parent route and will render the Dashboard Url screen.

- But we just want to go back as per the path i.e from /vans/:id ---> /vans.

- we can do this by:

```
<Link to='..' relative="path" >Go Back</Link> 
```

###  Outlet Context:

- Use this when you want pass data from the Parent route to child Routes.

- Lets imagine you have routes like this.

``` 
<Route path='vans/:id' element={<VansDetails/>}>
    <Route index element={<Details/>}></Route>
    <Route path='pricing' element={<Pricing/>}></Route>
    <Route path='photos' element={<Photos/>}></Route>
</Route>
```
- At VansDetails component you also have api to fetch the details of the van where it return the details like 
  - Basic Details
  - Pricing
  - Photos
- Now as the VanDetails component being a parent where it renders the its child using **Outlet** but how can we pass data to child routes.

- We can do this with the help of **useOutletContext**.

- Inside the VanDetails component which is parent Route component.
- Add context prop to the Outlet and assign the data that you want to pass.

``` 
<Outlet context={{currentVanApiData}}/>
```
#### useOutletContext:

- Use this in child route component to get the data that is passed in the parent route context.

``` 
import {useOutletContext} from 'react-router-dom';

const {currentVanApiData}=useOutletContext();

/// at code level....
<p>{currrentVanApiData.name}</p>
```

![Alt text](image-2.png)


### Search / Query Parameters:
- These are used for senarios like sorting, filtering,pagination.
- where the url changes and that would effect the ui and when shared or opened again your page will open extactly at the path where you were.
- These are lie key value pairs.
- Begin with ? Example: `/vans?type=rugged`
- you can also have multiple query parameters by adding & between then. Example: `/vans?type=rugged&filterBy=price`.
- Data inside the State lives inside the component but where as serach paramerts lives in URL.

#### URLSearchParams (Not Related to React Router):
- It is an built in JS Object that has methods to work with query strings of URL.
- The query string is the part of a URL that comes after the "?" character and contains key-value pairs separated by "&" characters. URLSearchParams allows you to manipulate these parameters easily.

``` 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>URLSearchParams Example</title>
</head>
<body>
<h1>URLSearchParams Example</h1>
<p>Current URL: <span id="currentURL"></span></p>
<p>Modified URL: <span id="modifiedURL"></span></p>

<script>
// Get the current URL
const currentURL = window.location.href;

// Display the current URL
document.getElementById('currentURL').textContent = currentURL;

// Create a new URLSearchParams object with the query parameters of the current URL
const params = new URLSearchParams(currentURL.split('?')[1]);

// Check if a query parameter exists and get its value
const nameParam = params.get('name');
if (nameParam) {
    console.log(`Name parameter value: ${nameParam}`);
}

// Set a new query parameter
params.set('newParam', 'newValue');

// Convert the modified URLSearchParams back to a string and update the current URL
const updatedURL = `${window.location.origin}${window.location.pathname}?${params.toString()}`;

// Display the modified URL
document.getElementById('modifiedURL').textContent = updatedURL;

// Update the current URL
window.history.replaceState({}, '', updatedURL);
</script>
</body>
</html>

```


#### useSearchParams():

``` 
import {useSearchParams} from 'react-router-dom';

// inside component code..
const [searchParams,setSearchParams]=useSearchParams();
console.log(searchParams.get('type'));
```
- if your URL look like this: http://localhost:3000/vans then here output will be `null` because there is no type query params here.

- But if your URL: http://localhost:3000/vans?type=rugged then output will be `rugged`










