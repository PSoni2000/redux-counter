## Notes

### Redux

1. to use Redux in react install **redux & react-redux** npm modules
2. create "store" folder & js file which will be our central store.
3. create redux store by -
   `const store = createStore(counterReducer);`
4. create reduce function for our redux store -
   router function uses two arguments state & action
   where -
   **state** - previous state
   **action** - tell us what action we have to perform (action.type) and can also contain some extra data.

   ```
   const initialState = { counter: 0, showCounter: true };

   const counterReducer = (state = initialState, action) => {
   if (action.type === 'increment') {
       return {
       counter: state.counter + 1,
       };
   }

   if (action.type === 'increase') {
       return {
       counter: state.counter + action.amount,
       };
   }

   return state;
   };
   ```

   full file -

   ```
   import { createStore } from 'redux';

    const initialState = { counter: 0 };

    const counterReducer = (state = initialState, action) => {
    if (action.type === 'increment') {
        return {
        counter: state.counter + 1,
        };
    }

    if (action.type === 'increase') {
        return {
        counter: state.counter + action.amount,
        };
    }

    return state;
    };

    const store = createStore(counterReducer);

    export default store;

   ```

5. Add provider wrapper to your root component. (same as react context-api)

   ```
   import React from 'react';
   import ReactDOM from 'react-dom/client';
   import { Provider } from 'react-redux';

   import './index.css';
   import App from './App';
   import store from './store/index';

   const root = ReactDOM.createRoot(document.getElementById('root'));
   root.render(
        <Provider store={store}>
            <App />
        </Provider>
   );

   ```

6. To get the store value in any component use react-redux.useSelector hook.
   `const counter = useSelector((state) => state.counter);`
   **useSelector** - also update the page when value is change just like useState. and it also unsubscribe when component is not anymore used.

7. To update store value we have to call dispatch function -
   `const dispatch = useDispatch();`
   we don't give any argument in useDispatch but it gives as dispatch function which we can use to update redux store values.

   ```
   import { useSelector, useDispatch } from "react-redux";

   import classes from "./Counter.module.css";

   const Counter = () => {
       const dispatch = useDispatch();
       const counter = useSelector((state) => state.counter);

       const incrementHandler = () => {
           dispatch({ type: "increment" });
       };

       const increaseHandler = () => {
           dispatch({ type: "increase", amount: 10 });
       };

       return (
           <main>
               <h1>Redux Counter : {counter}</h1>
               <div>
                   <button onClick={incrementHandler}>Increment</button>
                   <button onClick={increaseHandler}>Increase by 10</button>
               </div>
           </main>
       );
   };

   export default Counter;
   ```

### Class based Component Redux

In class based component we cannot use hooks so we have to connect our component with our state this way

1. import connect function from "react-redux"
2. connect component with redux store in export
   `export default connect(mapStateToProps, mapDispatchToProps)(Counter);`
   here -
   **mapStateToProps** - provide redux store state in component props
   **mapDispatchToProps** - provide dispatch function in component props

   ```
   const mapStateToProps = state => {
       return {
           counter: state.counter
       };
   }

   const mapDispatchToProps = dispatch => {
       return {
           increment: () => dispatch({ type: 'increment' }),
       }
   };
   ```

   **full code -**

   ```
   import { Component } from "react";
   import { connect } from "react-redux";

   class Counter extends Component {

   incrementHandler() {
       this.props.increment();
   }

   render() {
       return (
       <main className={classes.counter}>
           <h1>Redux Counter : {this.props.counter}</h1>
           <button onClick={this.incrementHandler.bind(this)}>Increment</button>
       </main>
       );
   }
   }

   const mapStateToProps = state => {
       return {
           counter: state.counter
       };
   }

   const mapDispatchToProps = dispatch => {
       return {
           increment: () => dispatch({ type: 'increment' }),
       }
   };

   export default connect(mapStateToProps, mapDispatchToProps)(Counter);
   ```
