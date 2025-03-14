# React / Best practices

### Hooks

#### Return Object from custom hooks instead of Array

GOOD

```js
const useData = () => {
  const [someData, setSomeData] = useState();
  const [someOtherData, setSomeOtherData] = useState();

  return {
    someData,
    someOtherData,
  };
};

// Usage
const { someData, someOtherData } = useData();
const { someOtherData, someData } = useData();
const { someOtherData } = useData();
const { someData: foo, someOtherData: bar } = useData();
```

BAD

This way when using the hook, the order and number of variables would matter.

```js
export const useData = () => {
  const [someData, setSomeData] = useState();
  const [someOtherData, setSomeOtherData] = useState();

  return [
    someData,
    someOtherData,
  ];
};
```

### Contexts

#### Use contexts for..

- centralized state or data storage, similar to Redux (but simpler)
- sharing data across multiple components without props-drilling
- keeping data between component re-renders

See also:
- https://kentcdodds.com/blog/prop-drilling
- https://kentcdodds.com/blog/application-state-management-with-react/
- https://kentcdodds.com/blog/how-to-use-react-context-effectively/
- https://github.com/c-hive/basics/tree/master/react-hook-context-memo
