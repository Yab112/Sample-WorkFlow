<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React Application with Redux - README</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            padding: 20px;
            max-width: 800px;
            margin: auto;
        }
        code {
            background: #f4f4f4;
            padding: 2px 4px;
            border-radius: 4px;
        }
        pre {
            background: #f4f4f4;
            padding: 10px;
            border-radius: 4px;
            overflow-x: auto;
        }
        h1, h2, h3 {
            color: #333;
        }
        ul {
            list-style-type: disc;
            margin-left: 20px;
        }
    </style>
</head>
<body>
    <h1>React Application with Redux</h1>
    <p>This documentation outlines the setup and configuration of a React application using traditional Redux methods with TypeScript and Tailwind CSS.</p>

    <h2>1. Set Up the Project</h2>
    <ul>
        <li><strong>Create a React App:</strong>
            <pre><code>npx create-react-app react-redux-app --template typescript</code></pre>
        </li>
        <li><strong>Install Dependencies:</strong>
            <pre><code>npm install @reduxjs/toolkit react-redux</code></pre>
        </li>
        <li><strong>Install Tailwind CSS:</strong>
            <pre><code>
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
            </code></pre>
            <p>Update <code>tailwind.config.js</code> and <code>src/index.css</code> as described in the Tailwind CSS documentation.</p>
        </li>
    </ul>

    <h2>2. Set Up Redux</h2>
    <ul>
        <li><strong>Create a Redux Slice:</strong>
            <p>Create a file called <code>counterSlice.ts</code> and add the following code:</p>
            <pre><code>
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
    name: 'counter',
    initialState: { value: 0 },
    reducers: {
        increment: state => { state.value += 1; },
        decrement: state => { state.value -= 1; },
        incrementByAmount: (state, action) => { state.value += action.payload; }
    }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
            </code></pre>
        </li>
        <li><strong>Configure the Store:</strong>
            <p>Create a file called <code>store.ts</code> and add the following code:</p>
            <pre><code>
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './features/counter/counterSlice';

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
            </code></pre>
        </li>
    </ul>

    <h2>3. Create React Components</h2>
    <ul>
        <li><strong>Counter Component:</strong>
            <p>Create a file called <code>Counter.tsx</code> and add the following code:</p>
            <pre><code>
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { RootState, AppDispatch } from './app/store';
import { increment, decrement, incrementByAmount } from './features/counter/counterSlice';

const Counter = () => {
    const count = useSelector((state: RootState) => state.counter.value);
    const dispatch = useDispatch<AppDispatch>();

    return (
        <div>
            <h1>{count}</h1>
            <button onClick={() => dispatch(increment())}>Increment</button>
            <button onClick={() => dispatch(decrement())}>Decrement</button>
            <button onClick={() => dispatch(incrementByAmount(5))}>Increment by 5</button>
        </div>
    );
};

export default Counter;
            </code></pre>
        </li>
    </ul>

    <h2>4. Integrate Redux with React</h2>
    <ul>
        <li><strong>Update App Component:</strong>
            <p>Modify <code>App.tsx</code> to include the Redux Provider:</p>
            <pre><code>
import React from 'react';
import { Provider } from 'react-redux';
import { store } from './app/store';
import Counter from './features/counter/Counter';

const App = () => {
    return (
        <Provider store={store}>
            <Counter />
        </Provider>
    );
};

export default App;
            </code></pre>
        </li>
    </ul>
</body>
</html>
