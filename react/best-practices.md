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

This way when using the hook, the order of variables would matter.

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
