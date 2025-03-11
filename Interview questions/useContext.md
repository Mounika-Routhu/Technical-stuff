**ThemeContext.js**
```js
import React, { createContext, useState } from 'react';

// Create the context with default value
const ThemeContext = createContext();

// Create a provider component to manage the theme
export const ThemeProvider = ({ children }) => {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const toggleTheme = () => {
    setIsDarkMode(prevMode => !prevMode);
  };

  return (
    <ThemeContext.Provider value={{ isDarkMode, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export default ThemeContext;
```

**App.js**
```
import React from 'react';
import { ThemeProvider } from './ThemeContext';
import Header from './Header';
import Content from './Content';
import './App.css'

const App = () => {
  return (
    <ThemeProvider>
      <div className="app">
        <Header />
        <Content />
      </div>
    </ThemeProvider>
  );
};

export default App;
```

**App.css**
```css
/* Default light mode styles */
.light-mode {
  background-color: #fff;
  color: #000;
}

.dark-mode {
  background-color: #333;
  color: #fff;
}

/* Additional styles for header and content */
.header {
  padding: 20px;
  text-align: center;
}

.content {
  padding: 20px;
}

```

**Header.js**
```js
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

const Header = () => {
  const { isDarkMode, toggleTheme } = useContext(ThemeContext);

  return (
    <header className={`header ${isDarkMode ? 'dark-mode' : 'light-mode'}`}>
      <h1>Theme Switcher</h1>
      <button onClick={toggleTheme}>
        Switch to {isDarkMode ? 'Light' : 'Dark'} Mode
      </button>
    </header>
  );
};

export default Header;
```

**Content.js**
```js
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

const Content = () => {
  const { isDarkMode } = useContext(ThemeContext);

  return (
    <div className={['content', [isDarkMode ? 'dark-mode' : 'light-mode']].join(' ')}>
      <p>This is some content.</p>
      <p>The current theme is {isDarkMode ? 'Dark' : 'Light'} Mode.</p>
    </div>
  );
};

export default Content;
```

**Summary:**
1. `ThemeContext`: context object created using `createContext`.
2. `ThemeProvider`:
   - component provides the theme and a toggle function & uses <ThemContext.Provider value={{ isDarkMode, toggleTheme }}> tag to wrap children in return
   - Component wraps the app to provide the theme context to all child components.
3. `useContext(ThemeContext)`: Consumed in components to apply theme-specific styles.
    - Before `useContext <ThemeContext.Consumer>` is used to consume values

  ```js
  <ThemeContext.Consumer>
      {({ isDarkMode }) => (
        <div className={`content ${isDarkMode ? 'dark-mode' : 'light-mode'}`}>
          <p>This is some content.</p>
          <p>The current theme is {isDarkMode ? 'Dark' : 'Light'} Mode.</p>
        </div>
      )}
  ```

**Note**: **createContext is not a hook**. It is a standard function to create context obj hence it can be called outside functional component unlike hooks. 

