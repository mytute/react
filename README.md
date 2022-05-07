# react


## init project

create react app with npx

```bash
# to create project with new folder
$ npx create-react-app <your-project-name>
# to create project inside current folder
$ npx create-react-app  
```

add react-boostrap to project (https://react-bootstrap.github.io/getting-started/introduction)
```bash
$ npm install react-bootstrap bootstrap@5.1.3
```


to keep nessory files (delect escept App.js and index.js inside src folder)

>index.js

```javascript
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

> App.js

```javascript
function App() {
  return (
    <div></div>
  );
}

export default App;
```

we have to use fragment when more than one return element

```javascript
import TodoList from './TodoList';

function App() {
  return (
    <>
      <TodoList/>
      <input type="text" />
      <button> Add Todo</button>
      <button>Clear Completed Todos</button>
      <div>0 left to do</div>
    </>
  );
}

export default App;
```

### manage state
this is kind of data store that we set update with dom.



### unique id lib

```bash
$ npm i uuid
```







### use with typescript

create react app with typescript/scss/mui
```bash
# add typescript
$ npx create-react-app my-app --template typescript
#add sass
$ npm i node-sass --save
$ npm i @types/node-sass --save-dev # This package contains type definitions for node-sass
$ npm i npm-run-all --save-dev # A CLI tool to run multiple npm-scripts in parallel or sequential.
```

make direction to sass files in package.json file.     
```json
  "scripts": {
    "build:css":"node-sass src/styles/sass/ -o src/styles/css/",
    "watch:css":"npm run build:css && node-sass src/styles/sass/ -o src/styles/css/ --watch --recursive",
    "start:js":"react-scripts start",
    "start":"npm-run-all -p watch:css start:js",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
```



to start created react app
```bash
$ npm run start
```

basic function component     
```typescript
import React from 'react';

const App:React.FC =()=> {
  return (
    <div>yo</div>
  );
}

export default App;
```

basic type with probs

```typescript
import React from 'react';

const TextField: React.FC<{text:string}> = (props) => {
  const {text} = props;
  return (
      <div>
          <input type="text" value={text} />
      </div>
  );
};

export default TextField;
```

probs type with interface.     

```typescript
import React from 'react';

interface TextFieldProps {
    text: string,
    number?: number // this is optional
    fn:(bob:string)=> string; // you can add function arguments, return type
}

const TextField: React.FC<TextFieldProps> = (props) => {
  const {text} = props;
  return (
      <div></div>
  );
};

export default TextField;
```

lifecycle hooks

```typescript
import React, { useRef, useState } from 'react';


// if useState value object the we can use "interface" if you need
interface ObjectType {
    text: string
}

const TextField: React.FC = () => {
  const [ count , setCount ]= useState<number | null>(5);

  // tsx will auto define type according to first value.
  // to fix it we can use pipe with multiple types eg <number | null>
  setCount(null);

  // use with interface
  const [ text, setText ]= useState<ObjectType>();
  setText({text:'xgen'});

  // with empty value hover on ref you can find the correct type eg "ref={}"
  // we should pass null as define type when it's undefine
  const devRef = useRef<HTMLDivElement>(null);


  return (
      <div ref={devRef}></div>
  );
};

export default TextField;
```

use type with events

```typescript
import React from 'react';

interface EventProbs{
    handleChange:(event:React.ChangeEvent<HTMLInputElement>)=> void;
}

const TextField: React.FC<EventProbs> = ({handleChange}) => {


  return (
      <div>
          <input  onChange={handleChange} />
      </div>
  );
};

export default TextField;
```

use type with "useReducer" hook. (only sample code)

```typescript
import React,{useReducer, useState} from 'react';

interface Todo {
    text: string,
    complete: boolean
}
type State =Todo[];
type Action=
       | {type:"add", text:string}
       | {type:"remove", idx: number};

const TodoReducer = (state:State , action:Action)=>{
    switch(action.type){
        case "add":
            return [...state, {text:action.text, complete:false}];
        case "remove":
            return state.filter((_, i)=> action.idx !==i);
        default:
            return state;
    }

}

const TextField: React.FC = ({}) => {

  const [todo, dispatch] = useReducer(TodoReducer, []);
  const [text, setText] = useState<string>('');

  return (
      <div>
          <button onClick={()=>{ dispatch({type:"add", text:text}); setText('')}}></button>
      </div>
  );
};

export default TextField;
```

use types in render probs #todo make this example better
```typescript
// parent component
import React from 'react';

import TextField from './components/text-field/text-field.component';


const App:React.FC =()=> {

  return (
    <div>
      <TextField>
        {
          (count,setCount)=>(
            <div>
              {count}
              <button onClick={()=>setCount(1)}>+</button>
            </div>
          )
        }
      </TextField>
    </div>
  );
}

export default App;

// child component
import React,{useReducer, useState} from 'react';

interface Props{
    children:(
        count:number,
        setCount:React.Dispatch<React.SetStateAction<number>>
    )=> JSX.Element | null;
}

const TextField: React.FC<Props> = ({children}) => {

  const [count , setCount]= useState<number>(0);

  return (
      <div>
          {children(count, setCount)}
      </div>
  );
};

export default TextField;


```
