# React / Best practices

## Contexts
- Use to share data across multiple components without props-drilling
- Use to keep data between component re-renders
- Use with state-management hooks to create a dynamically updatable data-storage system
- Fail fast in case the context is used outside the provider

#### Resources
- https://kentcdodds.com/blog/prop-drilling
- https://kentcdodds.com/blog/application-state-management-with-react/
- https://kentcdodds.com/blog/how-to-use-react-context-effectively/