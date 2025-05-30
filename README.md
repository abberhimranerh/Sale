
## 1. App.js (Main Component)

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import ListComponent from './ListComponent';
import './App.css';

function App() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        // Using JSONPlaceholder API as suggested
        const response = await axios.get('https://jsonplaceholder.typicode.com/users');
        setData(response.data);
        setLoading(false);
      } catch (err) {
        setError(err.message);
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  // Custom render function for list items
  const renderUserItem = (user) => (
    <div className="user-item">
      <h3>{user.name}</h3>
      <p>Email: {user.email}</p>
      <p>Phone: {user.phone}</p>
    </div>
  );

  if (loading) {
    return <div className="loading">Loading data...</div>;
  }

  if (error) {
    return <div className="error">Error: {error}</div>;
  }

  return (
    <div className="app">
      <h1>User List</h1>
      <ListComponent 
        items={data} 
        renderItem={renderUserItem} 
        listClassName="user-list"
        itemClassName="user-list-item"
      />
    </div>
  );
}

export default App;
```

## 2. ListComponent.js (Reusable Component)

```jsx
import React from 'react';

const ListComponent = ({ items, renderItem, listClassName, itemClassName }) => {
  if (!items || items.length === 0) {
    return <div className="empty-list">No items to display</div>;
  }

  return (
    <ul className={listClassName || 'default-list'}>
      {items.map((item, index) => (
        <li key={index} className={itemClassName || 'default-list-item'}>
          {renderItem ? renderItem(item) : item.toString()}
        </li>
      ))}
    </ul>
  );
};

export default ListComponent;
```

## 3. App.css (Styling)

```css
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.loading, .error, .empty-list {
  text-align: center;
  padding: 20px;
  font-size: 18px;
}

.error {
  color: #d32f2f;
}

.user-list {
  list-style: none;
  padding: 0;
}

.user-list-item {
  background: #f5f5f5;
  margin-bottom: 10px;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.user-item h3 {
  margin-top: 0;
  color: #1976d2;
}

.user-item p {
  margin: 5px 0;
}
```

## 4. Index.js (Entry Point)

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

## 5. package.json Dependencies

```json
{
  "dependencies": {
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "axios": "^0.21.1",
    "@testing-library/react": "^12.0.0",
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/user-event": "^13.1.9"
  }
}
```
