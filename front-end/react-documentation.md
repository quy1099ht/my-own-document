# React Knowledge Compilation

A compilation of my knowledge about React.

## Table of Contents

- [Introduction](#introduction)
- [Key Concepts](#key-concepts)
  - [Components](#components)
  - [Props](#props)
  - [State](#state)
  - [Lifecycle Methods](#lifecycle-methods)
  - [Hooks](#hooks)
- [Folder Structure](#folder-structure)
- [Routing](#routing)
- [State Management](#state-management)
- [HTTP Requests](#http-requests)
- [Best Practices](#best-practices)
- [Tools and Libraries](#tools-and-libraries)
- [Resources](#resources)

## Introduction

React is a JavaScript library for building user interfaces, particularly for creating single-page applications where the user interacts with the application without having to reload the entire page. Developed and maintained by Facebook, React has become one of the most popular front-end libraries for building modern, interactive web applications.

## Key Concepts

### Components

#### Definition

Components are the building blocks of a React application. They can be functional components or class components and are responsible for rendering UI elements.

#### Types

1. **Functional Components:**
   - Defined as functions.
   - Can use React hooks.
   - Example:

     ```jsx
     const MyComponent: React.FC = () => {
       return <div>Hello, React!</div>;
     };
     ```
    - This is the prefered practice

2. **Class Components:**
   - Defined as ES6 classes.
   - Can have lifecycle methods.
   - Example:

     ```jsx
     class MyClassComponent extends React.Component {
       render() {
         return <div>Hello, React!</div>;
       }
     }
     ```
### JSX (JavaScript XML)

#### Definition

JSX is a syntax extension for JavaScript that allows writing UI components using a syntax similar to XML or HTML.

#### Example

```jsx
const element = <h1>Hello, React!</h1>;
```

### Props

#### Definition

- Props are inputs that a React component can receive. They allow passing data from a parent component to a child component.

#### Example
Preferable way:
```tsx
    const MyComponentType = {
        message: string;
    }

    const MyComponent: React.FC<MyComponentType> = ({message}) => {
        return <div>{message}</div>;
    };

    const RenderMyComponent: React.FC = () => {
        return </MyComponent message={'something'}>
    }
```

Second way:
```tsx
    const MyComponent: React.FC = ({message}) => {
        return <div>{message}</div>
    }

    const RenderMyComponent: React.FC = () => {
        return </MyComponent message={'something'}>
    }
```

- The first way of implementing is more preferable because it force strict type passing

### State

#### Definition

- In React, component state is a crucial concept for managing and representing the internal data of a component. Unlike props, which are passed from parent to child components, state is managed within a component itself. This allows components to keep track of information that can change over time, such as user interactions, input values, or the result of asynchronous operations.

#### Example

```tsx
    import React, { useState } from 'react';

    const Counter = () => {
    // Declare a state variable named "count" with an initial value of 0
    const [count, setCount] = useState(0);

    // Event handler to increment the count
    const handleIncrement = () => {
        // Use the setCount function to update the state
        setCount(count + 1);
    };

    // Event handler to decrement the count
    const handleDecrement = () => {
        // Use the setCount function to update the state
        setCount(count - 1);
    };

    return (
        <div>
        <h1>Count: {count}</h1>
        <button onClick={handleIncrement}>Increment</button>
        <button onClick={handleDecrement}>Decrement</button>
        </div>
    );
    };

    export default Counter;
```

### Lifecycle Methods

### Mounting

```tsx
        import React, { useState, useEffect } from 'react';

        const Counter = () => {
            const [count, setCount] = useState(0);

            // On mounting
            // This will run one when web app render this component
            useEffect(() => {
                handleIncrement();

                // Handle unmount
                return () => {
                    setCount(0);
                };
            }, [])

            const handleIncrement = () => {
                setCount(count + 1);
            };

            const handleDecrement = () => {
                setCount(count - 1);
            };

            return (
                <div>{count}</div>
            );
        };

        export default Counter;
```

### Hooks

#### Common hooks

- React Hooks are functions that enable functional components to have state and lifecycle features, which were previously exclusive to class components. Hooks were introduced in React 16.8 to simplify and enhance the state management and side-effect handling in functional components. Two commonly used hooks are `useState` and `useEffect`.

`useState`
- The `useState` hook allows functional components to manage state. It takes an initial state as an argument and returns an array with two elements: the current state and a function to update the state. Here's a basic example:

```tsx
    import React, { useState } from 'react';

    const Counter = () => {
        const [count, setCount] = useState(0);

        const handleIncrement = () => {
            setCount(count + 1);
        };

        return (
            <div>
            <p>Count: {count}</p>
            <button onClick={handleIncrement}>Increment</button>
            </div>
        );
    };
```
- In this example, count is the state variable, and setCount is the function to update it. When the button is clicked, setCount is called with the new count, triggering a re-render with the updated state.

`useEffect`
- The `useEffect` hook is used for handling side effects in functional components. It takes a function as its first argument, which will be executed after the component renders. It is often used for tasks like data fetching, subscriptions, or manually changing the DOM. Here's an example:

```tsx
import React, { useState, useEffect } from 'react';

const DataFetchingComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    // Data fetching logic
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    fetchData();
  }, []); // Empty dependency array means this effect runs once after the initial render

  return (
    <div>
      <p>Data: {data ? JSON.stringify(data) : 'Loading...'}</p>
    </div>
  );
};
```

It can also be use for following certain states changes
```tsx
    import React, { useState } from 'react';

    const Counter = () => {
        const [count, setCount] = useState(0);

        const handleIncrement = () => {
            setCount(count + 1);
        };

        // It will alert everytime count value change
        useEffect(() => {
            alert(`Count change to ${count}`)
        }, [count])

        return (
            <div>
            <p>Count: {count}</p>
            <button onClick={handleIncrement}>Increment</button>
            </div>
        );
    };
```

#### Custom hook

Instead of using useState everywhere and codebase will be duplicated. Custom hooks are the way to go. Here is an example:
```tsx
import { useState } from 'react';

const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  return {
    count,
    increment,
    decrement,
  };
};

export default useCounter;
```
You can then use this custom hook in your components:
```tsx
import React from 'react';
import useCounter from './useCounter';

const CounterComponent = () => {
  const { count, increment, decrement } = useCounter(10);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

## Folder Structure

```
/public
|-- /images
| |-- image1.png
/src
|-- /context
| |-- AuthContext.tsx
| |-- ThemeContext.tsx
|
|-- /shared
| |-- /components
| | |-- Header.tsx
| | |-- Footer.tsx
|
|-- /components
| |-- /Feature1
| | |-- Feature1Component1.tsx
| | |-- Feature1Component2.tsx
|
|-- /containers
| |-- /Feature1Container.tsx
|
|-- /pages
| |-- HomePage.tsx
| |-- AboutPage.tsx
|
|-- /hooks
| |-- useLocalStorage.tsx
|
|-- /models
| |-- /entity1
| | |-- entityModel.ts
|
|-- /services
| |-- Feature1Services.ts
|
|-- /utils
| |-- helpers.ts
| |-- constants.ts
|
|-- App.tsx
|-- index.js
|-- .gitignore
|-- package.json
|-- .eslintrc.js
|-- .prettierrc
|-- webpack.config.js
|-- babel.config.js
|-- README.md
```

This is not the best structure but just a good baseline

## Routing

### Simple routing

```tsx
import React from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import Home from './Home';
import About from './About';
import Contact from './Contact';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/contact" element={<Contact />} />
    </Routes>
  );
};

export default App;
```

### Required Login kind of routing

#### `RequiredLogin` component:
```tsx
import React, { useEffect } from 'react';
import { Outlet, useNavigate } from 'react-router-dom';

// A component for routes that require authentication
const RequiredLogin: React.FC = () => {
  const navigate = useNavigate();
  const hasAccessToken = localStorage.getItem('accessToken'); // Replace with your auth logic

  useEffect(() => {
    if (!hasAccessToken) {
      navigate('/login');
    }
  }, [hasAccessToken, navigate]);

  return <Outlet />;
};

export default RequiredLogin;
```
#### `RequiredAdmin` component
```tsx
import React, { useEffect } from 'react';
import { Outlet, useNavigate } from 'react-router-dom';

// A component for routes that require admin access
const RequiredAdmin: React.FC = () => {
  const navigate = useNavigate();
  const isAdmin = localStorage.getItem('isAdmin') === 'true'; // Replace with your logic for checking admin status

  useEffect(() => {
    if (!isAdmin) {
      // Redirect to login or handle unauthorized access
      navigate('/unauthorized');
    }
  }, [isAdmin, navigate]);

  return <Outlet />;
};

export default RequiredAdmin;
```
#### Implementation:
```tsx
import React from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import RequiredLogin from './RequiredLogin'
import RequiredAdmin from './RequiredAdmin'
import Home from './Home';
import About from './About';
import Contact from './Contact';
import Admin from './Admin';
import Login from './Login';
import Unauthorized from './Unauthorized';


const App = () => {
  return (
    <Routes>
      <Route element={<RequiredLogin />} >
        <Route path="/" element={<Home />} />
        <Route path="/contact" element={<Contact />} />

        <Route element={<RequiredAdmin />} > 
          <Route path="/admin" element={<Admin />} />
        </Route>

        <Route path="/unauthorized" element={<Unauthorized />} />
      </Route>

      {/* This route doesn't require login */}
      <Route path="/about" element={<About />} />
      <Route path="/login" element={<Login />} />
    </Routes>
  );
};

export default App;
```

## State Management

State management in React involves handling and controlling data within an application. Here are three popular options:

### 1. Context API
- Use Case: Good for managing shared state across components.
#### Example:
```tsx
const MyContext = React.createContext();

const MyApp = () => {
  const sharedState = { data: 'Shared Data' };

  return (
    <MyContext.Provider value={sharedState}>
      <ChildComponent />
    </MyContext.Provider>
  );
};

const ChildComponent = () => {
  const { data } = React.useContext(MyContext);
  return <div>{data}</div>;
};
```

### 2.Redux/Redux toolkit
- Use Case: Ideal for managing the state of the entire application in a centralized way.

#### Example:

```tsx
const initialState = { data: 'Initial Data' };

const dataReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'UPDATE_DATA':
      return { ...state, data: action.payload };
    default:
      return state;
  }
};

const store = Redux.createStore(dataReducer);

const ConnectedComponent = () => {
  const data = store.getState().data;
  return <div>{data}</div>;
};
```

Read more on [Redux Toolkit](https://redux-toolkit.js.org/tutorials/typescript)

## HTTP Requests

Use [Axios](https://axios-http.com/docs/intro)

### Implementing HTTP Service

```tsx
// httpService.ts
import axios from 'axios';

// Set up the base URL for your API
const baseURL = 'https://api.example.com';

// Create an instance of Axios with a custom config
const httpService = axios.create({
  baseURL,
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${your-token}`,
  },
});

// Interceptors are optional

// Adding a request interceptor 
// This part can already be done above
httpService.interceptors.request.use(
  config => {
    // Add headers, authentication, etc.
    return config;
  },
  error => {
    return Promise.reject(error);
  }
);

// Adding a response interceptor
httpService.interceptors.response.use(
  response => {
    // Handle successful responses
    return response;
  },
  error => {
    // Handle errors
    // Should not handle popup of errors in here
    return Promise.reject(error);
  }
);


export default httpService;
```

### Usage

#### Create a feature's services

```tsx
// userService.js
import httpService from './httpService';

const userService = {
  getUsers: () => {
    return httpService.get('/users');
  },

  getUserById: (userId) => {
    return httpService.get(`/users/${userId}`);
  },

  createUser: (userData) => {
    return httpService.post('/users', userData);
  },

  updateUser: (userId, userData) => {
    return httpService.put(`/users/${userId}`, userData);
  },

  deleteUser: (userId) => {
    return httpService.delete(`/users/${userId}`);
  },
};

export default userService;
```

#### Example

```tsx
// UserListComponent.js
import React, { useEffect, useState } from 'react';
import userService from './userService';

const UserListComponent = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    // Fetch the list of users when the component mounts
    userService.getUsers()
      .then(response => {
        setUsers(response.data);
      })
      .catch(error => {
        console.error('Error fetching users:', error);
      });
  }, []);

  return (
    <div>
      <h2>User List</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default UserListComponent;
```

## Best Practices

- Read [AirBnB best practices](https://github.com/airbnb/javascript)

## Tools and Libraries

- `react-toastify`
- `docxjs`
- `tailwindcss`
- `axios`
- `redux-toolkit`
- `ant design`
- `material-ui`
- `mdn websocket`

## Resources

- Leave it here for now
