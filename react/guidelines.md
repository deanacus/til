# React Guidelines

## General

* Prefer Function Components over Class Components
* Single Component per file
* Use PascalCase for both components and file names
* Take advantage of re-exporting from an index file
* Prefer short Fragment syntax

## State Management

* Prefer Context over Redux
* Keep Context as low in the tree as possible
* Follow the [Kent Dodds Context](https://kentcdodds.com/blog/how-to-use-react-context-effectively/) pattern
* Prefer `useReducer` over `useState` for anything more than a simple primitive value

## Props

* Avoid prop spreading
* Prefer children over props
* Prefer primitive values over Objects or Arrays
* Avoid passing
* Avoid render props
* Avoid shadowing HTML attributes
* Prefer optional props
* Props describe their purpose: `isDisabled`, `onClick`
* Expose `id`, `className`, `as` and `aria-*` whenever possible
* Document each components props - propTypes, TypeScript, JSDoc

## Styling

* Style using Styled Components or Emotion
* Put styles in a separate, sibling file
* Use a theme in the spirit of the [System UI Theme Spec](https://system-ui.com/theme/)

## Logic

* Keep components as logic free as possible
* Use Hooks
* Too many `useEffects` is a code smell
* Encapsulate stateful, reuseable logic in custom hooks
* Keep JSX as free of logic as possible.

## Data

* Don't try to roll your own data fetching. [React Query](https://react-query.tanstack.com/) is virtually perfect
* Handle empty, loading, and error states.
* Never use [array indices as keys](https://reactjs.org/docs/lists-and-keys.html#keys)

## Testing

* Prefer [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro) over Enzyme
* Avoid mocking anything but network requests where possible
* Snapshots are pointless
* Colocate tests with what they're testing
* Prefer many small, simple tests over fewer, larger, more complex tests
* Don't be afraid to [wrap and re-export](https://testing-library.com/docs/react-testing-library/setup/#custom-render) the render function for custom wrappers