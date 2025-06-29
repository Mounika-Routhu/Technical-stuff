# Routing in React
1. Routing in React allows you to create a Single Page Application (SPA) with multiple views (pages), using URLs without reloading the whole page.
2. React by itself doesn't support routing — we use a library called:
   `react-router-dom (for web apps)`

## Key Concepts in React Router:
4. **BrowserRouter**: This is the component that wraps your entire app to enable routing.
2. **Routes**: This is used to group multiple routes. It ensures that only the first matching route is rendered.
1. **Route**: This is used to define a path and the component(element) that should render when the user navigates to that path.
3. **Link**: This is used to create links that navigate to different routes within your app, without refreshing the page.
4. **NavLink**: Same as Link but comes with isActive to determine current page. remember ({ isActive }) as an object

## Basic Example:
1. **Install React Router**:
   You need to install `react-router-dom` to add routing functionality in your React app.
   ```bash
   npm install react-router-dom
   ```
2. **Wrap your app in `<BrowserRouter>`**
```JS
import { BrowserRouter } from 'react-router-dom';

const App = () => (
  <BrowserRouter>
    {/* Your routes and links go here */}
  </BrowserRouter>
);
```
3. **Define Routes using `<Routes>` and `<Route>`**
Note: route match only pathName not query string(anything after ?)
```JS
import { BrowserRouter } from 'react-router-dom';

const App = () => (
  <BrowserRouter>
    <Routes>
     <Route path="/" element={<Home />} />
     <Route path="/about" element={<About />} />
   </Routes>
  </BrowserRouter>
);
```

4. **Add Navigation with `<Link>` or `<NavLink>`** in another file or include in APP.js only
Note: remember ({ isActive }) **as an object**
```JS
import { Link, NavLink } from 'react-router-dom';

<Link to="/about">Go to About</Link>
<NavLink to="/" style={({ isActive }) => ({ color: isActive ? 'red' : 'black' })}>
  Home
</NavLink>
```

## dynamic routes:
1. dynamic routes can be added using `:` & use useParams() hook to get route parameters
```JS
<Route path="/user/:id" element={<User />} />

// Inside User component
import { useParams } from 'react-router-dom';

const location = useLocation();

const { id } = useParams(); // Access dynamic segment
//fetch results based on ID & display
```

## Nested routes - `<Outlet />`
1. we can directed write nested routes inside App.js inside parent Route
2. Don't include / for nested routes, if so they will become absolute routes & don't render at <Outlet />
```JS
<BrowserRouter>
    <Routes>
     <Route path="/" element={<Home />} />
     <Route path="/about" element={<About />} />
     <Route path="/dashboard" element={<DashboardLayout />}>
        <Route path="profile" element={<Profile />} /> // relative path
     </Route>
   </Routes>

// DashboardLayout.jsx
<Outlet /> // Renders <Profile />
```

## Wildcard Routes (*)
```JS
<Route path="/dashboard/*" element={<Dashboard />} /> // matches anything that comes after /dashboard/settings/profile
<Route path="*" element={<NotFound />} /> // Catch-all
```

## useNavigate() – Programmatic navigation
1. Earlier in V5, useHistory is used, now in V6 useNavigate is being used
```JS
import { useNavigate } from 'react-router-dom';

const location = useLocation();

const navigate = useNavigate();
navigate('/home'); // navigates to home page
navigate(-1); // go 1 page back in history stack
navigate('/settings', {replace: true}) // navigates to settings page by replacing current page entry in history stack
```

## useLocation() - Read only - no editing
1. This hook lets you access the current URL info (location object) from anywhere inside your component.
2. like search query params, state passed between routes.
3. Note: route match only pathName not query string(anything after ?)
4. URLSearchParams - built in JS web API,
   - Handles decoding (?q=react%20router → react router)
   - has methods to extract/set/check - `.get(key)`, `set(name, value)`, `.has('key')` 

`URL : /search?q=react&page=2`
```JS
import { useLocation } from 'react-router-dom';

const location = useLocation();
const query = new URLSearchParams(location.search); //query params

const q = query.get('q');       // "react"
const page = query.get('page'); // "2"
```

## pass data btw routes to share data
1. Pass through dynamic routes `:` & access useParams
2. Pass through `Link` or `NavLink` include state prop as object & access useLocation
```JS
import { NavLink } from 'react-router-dom';

<NavLink to="/product/42" state={{ name: 'T-shirt', price: 299 }}>
  Go to Product 42
</NavLink>
```
3. Pass through `useNavigate` & access useLocation
```JS
import { useNavigate } from 'react-router-dom';

const ButtonNav = () => {
  const navigate = useNavigate();

  const goToProduct = () => {
    navigate('/product/42', { state: { name: 'Jeans', price: 999 } });
  };

  return <button onClick={goToProduct}>Buy Now</button>;
};
```
4. access by useLocation
```JS
//access it using useLocation from state inside component
import { useParams, useLocation } from 'react-router-dom';

const Product = () => {
  const { id } = useParams();
  const location = useLocation();
  const { name, price } = location.state || {};

  return (
    <div>
      <h2>Product ID: {id}</h2>
      <p>Name: {name}</p>
      <p>Price: ₹{price}</p>
    </div>
  );
};
```
