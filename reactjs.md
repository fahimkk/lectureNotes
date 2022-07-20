# React js

## Introduction

React js is a library in javascript, used to create web apps.

To run react js, we have to install the required versions of nodejs and npm.

To create a react app

```bash
npx create-react-app appName
```

to run the app (live server)

```bash
cd appName
npm start
```

To create an element in react

```nodejs
var x = React.createElement("p", {id:"test"}, "some content")
```

here first arg is the component(tag) in html, 2nd is the argument and the 3rd is the content inside that tag.
If we create html by this method, we have to createElement for each tag (ie each line)

So instead of this method we can use `jsX`. jsX is using baba preprocessor to compile the text(html type) into real html format.

We have to wrap the elements inside the parent tag `<div>` tag.

```nodejs
<div>
    <p> Title 1 </p>
</div>
```

If you want to pass the element without parent tag we can use fragment option.

```nodejs
<React.Fragment>
    <p> Title 1 </p>
</React.Fragment>
```

We can execute any javascript code inside it by using curly brackets {}.

```nodejs
    function App(){
        var name = "x"
        return (
            <div>
                <p>
                    Welcome {name}
                </p>
            </div>
        );
    }
```

```nodejs
    function App(){
        var age = 18;
        return (
            <div>
                <p>
                    You are {age>=18?"eligible":"not eligible"}
                </p>
            </div>
        );
    }
```

## Components

In react.js there are two type of components

1. Class component -> stateful component. Components using Class
2. Function Component -> stateless component. Components using Function

## Function Components

In the above example function App is a function component.
When we creating a Component use First letter in capital.

Normally we write each components in separate files, and each components returns `jsX` object.

To use this component inside another file (`import`), we have to type export command at the end

```nodejs
export default FunctionName;
```

When can only use a single function as default. The advantage of default export is that when we importing the file we can call it by any name.

```nodejs
import FunctionName from './FileName';
or
import FuncName from './FileName';
```

If you have more than one functions to export you can't set default,

```nodejs
export {FunctionName1, FunctionName2};
```

So when importing this function, 

```nodejs
import {FunctionName1} from './FileName';
```
