# Create Context Template

```
import React, { createContext, useReducer } from 'react';

const StateContext = createContext({});
const DispatchContext = createContext({});

interface State {
  [key: string]: any
}

interface Action {
  type: string;
  payload: {
    [key: string]: any;
  }
}

const reducer: React.Reducer<State, Action> = (state, action) => {
  switch (action.type) {
    default: {
      throw new Error(`Unhandled action type: ${action.type}`);
    }
  }
}

const Provider: React.FC = ({ children }): JSX.Element => {
  const [state, dispatch] = useReducer(Reducer, {});

  return (
    <StateContext.Provider value={state}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}

const useSharedState = (): State => {
  const context = React.useContext(StateContext);
  if (context === undefined) {
    throw new Error('useSharedState must be used within a StateContext Provider');
  }
  return context;
}

const useDispatch = (): React.Dispatch<Action> => {
  const context = React.useContext(DispatchContext);
  if (context === undefined) {
    throw new Error('useDispatch must be used within a DispatchContext Provider');
  }
  return context;
}

export { Provider as default, useState, useDispatch};
```
