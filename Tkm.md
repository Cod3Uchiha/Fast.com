## README.md

**Project Description**

This project is a clone of [Fast.com](https://fast.com/), a popular internet speed test website. It is built using React and provides a simple, user-friendly interface for testing your internet connection.

**Installation Instructions**

```
npx create-react-app fast-clone
cd fast-clone
npm start
```

**Usage Examples**

Once the project is running, you can access the speed test at [http://localhost:3000/](http://localhost:3000/).

The test will automatically start and display the results on the screen. You can also click the "Restart Test" button to run the test again.

**Project Structure**

The project is structured as follows:

```
├── README.md
├── package.json
├── src
│   ├── App.js
│   ├── App.test.js
│   ├── components
│   │   ├── Button.js
│   │   ├── Card.js
│   │   ├── Form.js
│   │   ├── Input.js
│   │   ├── Logo.js
│   │   ├── Progress.js
│   │   ├── Results.js
│   │   ├── Spinner.js
│   │   ├── Test.js
│   ├── config.json
│   ├── index.css
│   ├── index.js
│   └──reportWebVitals.js
└── .gitignore
```

**Additional Features**

In addition to the basic speed test, this project also includes the following features:

- **Customizable color theme**
- **Dark mode**
- **Reset button**

**Contributions**

Contributions are welcome! Please read the [contributing guidelines](CONTRIBUTING.md) before submitting a pull request.

## .gitignore

```
node_modules/
build/
.DS_Store
```

## package.json

```json
{
  "name": "fast-clone",
  "version": "1.0.0",
  "description": "A clone of Fast.com built using React.",
  "main": "index.js",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "author": "Your Name",
  "license": "ISC",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "^5.0.1"
  },
  "devDependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "jest": "^29.4.1"
  }
}
```

## config.json

```json
{
  "primaryColor": "#06b6d4",
  "secondaryColor": "#343a40",
  "accentColor": "#e07a5f",
  "darkMode": false
}
```

## index.js

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { App } from "./App";
import "./index.css";
import { loadConfig } from "./utils/config";

const config = loadConfig();

ReactDOM.render(
  <React.StrictMode>
    <App config={config} />
  </React.StrictMode>,
  document.getElementById("root")
);
```

## src/App.js

```jsx
import React, { useState, useEffect } from "react";
import { Logo, Form, Results, Test, Spinner } from "./components";
import { loadConfig } from "../utils/config";

const App = ({ config }) => {
  const [isTesting, setIsTesting] = useState(false);
  const [results, setResults] = useState(null);
  const [configState, setConfigState] = useState(config);

  useEffect(() => {
    setConfigState(config);
  }, [config]);

  const startTest = () => {
    setIsTesting(true);
    Test(configState)
      .then((results) => setResults(results))
      .finally(() => setIsTesting(false));
  };

  const resetTest = () => {
    setResults(null);
  };

  return (
    <div className="container">
      <div className="header">
        <Logo />
        <h1>Internet Speed Test</h1>
        <p>
          Check your internet speed with just one click. No ads, no hassle.
        </p>
      </div>
      <div className="content">
        <Form config={configState} onStartTest={startTest} />
        {results ? (
          <Results results={results} onResetTest={resetTest} />
        ) : (
          <div className="test-area">
            {isTesting ? <Spinner /> : <button onClick={startTest}>Test Now</button>}
          </div>
        )}
      </div>
    </div>
  );
};

export default App;
```

## src/components/Button.js

```jsx
import React from "react";

const Button = ({ children, onClick, ...props }) => {
  return (
    <button onClick={onClick} {...props}>
      {children}
    </button>
  );
};

export default Button;
```

## src/components/Card.js

```jsx
import React from "react";

const Card = ({ children, ...props }) => {
  return <div className="card" {...props}>{children}</div>;
};

export default Card;
```

## src/components/Form.js

```jsx
import React, { useState } from "react";
import { Input, Button } from "./";

const Form = ({ config, onStartTest }) => {
  const [server, setServer] = useState(config.defaultServer);

  const handleChange = (e) => {
    setServer(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onStartTest({ server });
  };

  return (
    <form onSubmit={handleSubmit}>
      <Input
        label="Server"
        value={server}
        onChange={handleChange}
        placeholder="Enter a server URL or IP address"
      />
      <Button type="submit">Start Test</Button>
    </form>
  );
};

export default Form;
```

## src/components/Input.js

```jsx
import React from "react";

const Input = ({ label, value, onChange, ...props }) => {
  return (
    <div className="input-container">
      <label htmlFor={label}>{label}</label>
      <input type="text" id={label} value={value} onChange={onChange} {...props} />
    </div>
  );
};

export default Input;
```

## src/components/Logo.js

```jsx
import React from "react";

const Logo = () => {
  return (
    <svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path
        d="M44 28.9971L37.1529 20H26.2187L19.3716 29H12L6 36H0V48H48V35.9971L41.8471 28.9971H44ZM19.3716 22H37.1529L44 30V18L37.1529 22H19.3716Z"
        fill="currentColor"
      />
    </svg>
  );
};

export default Logo;
```

## src/components/Progress.js

```jsx
import React from "react";

const Progress = ({ progress }) => {
  return (
    <div className="progress">
      <div className="progress-bar" style={{ width: `${progress}%` }}></div>
    </div>
  );
};

export default Progress;
```

## src/components/Results.js

```jsx
import React from "react";
import { Card, Button } from "./";

const Results = ({ results, onResetTest }) => {
  return (
    <Card>
      <div className="results">
        <div className="result-item">
          <span>Download Speed</span>
          <span>{results.download} Mbps</span>
        </div>
        <div className="result-item">
          <span>Upload Speed</span>
          <