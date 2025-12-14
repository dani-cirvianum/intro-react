# Axios amb React

### Ús d'Axios amb React

#### Exemple senzill amb `useEffect`

```javascript
import { useEffect, useState } from 'react';
import api from './api';

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    api.get('/users')
      .then(res => setUsers(res.data))
      .catch(console.error);
  }, []);

  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}

export default Users;
```

#### Ús amb async/await dins de React

```javascript
useEffect(() => {
  const fetchUsers = async () => {
    try {
      const res = await api.get('/users');
      setUsers(res.data);
    } catch (error) {
      console.error(error);
    }
  };
  fetchUsers();
}, []);
```

### Hooks personalitzats per Axios en React

#### `useAxios` hook

```javascript
import { useState, useEffect } from 'react';
import api from '../api';

export function useAxios(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let mounted = true;

    api.get(url)
      .then(res => {
        if (mounted) {
          setData(res.data);
        }
      })
      .catch(err => setError(err))
      .finally(() => setLoading(false));

    return () => { mounted = false; };
  }, [url]);

  return { data, loading, error };
}
```
