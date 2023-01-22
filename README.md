# MERN Stack Best Practices

## Table of Contents

<details>
  <summary>
    <a href="#1-reactjs-best-practices">1. ReactJS Best Practices</a>
  </summary>

&emsp;&emsp;[1.1 Use JSX ShortHand](#11-use-jsx-shorthand)</br>
&emsp;&emsp;[1.2 Use Ternary Operators](#12-use-ternary-operators)</br>
&emsp;&emsp;[1.3 Use Object Literals](#13-use-object-literals)</br>
&emsp;&emsp;[1.4 Use Fragments](#14-use-fragments)</br>
&emsp;&emsp;[1.5 Do not define a function inside Render](#15-do-not-define-a-function-inside-render)</br>
&emsp;&emsp;[1.6 Use Memo](#16-use-memo)</br>
&emsp;&emsp;[1.7 Use Object Destructuring](#17-use-object-destructuring)</br>
&emsp;&emsp;[1.8 String Props do not need Curly Braces](#18-string-props-do-not-need-curly-braces)</br>
&emsp;&emsp;[1.9 Remove JS Code From JSX](#19-remove-js-code-from-jsx)</br>
&emsp;&emsp;[1.10 Use Template Literals](#110-use-template-literals)</br>
&emsp;&emsp;[1.11 Import in Order](#111-import-in-order)</br>
&emsp;&emsp;[1.12 Use Implicit return](#112-use-implicit-return)</br>
&emsp;&emsp;[1.13 Component Naming](#113-component-naming)</br>
&emsp;&emsp;[1.14 Reserved Prop Naming](#114-reserved-prop-naming)</br>
&emsp;&emsp;[1.15 Quotes](#115-quotes)</br>
&emsp;&emsp;[1.16 Prop Naming](#116-prop-naming)</br>
&emsp;&emsp;[1.17 JSX in Parentheses](#117-jsx-in-parentheses)</br>
&emsp;&emsp;[1.18 Self-Closing Tags](#118-self-closing-tags)</br>
&emsp;&emsp;[1.19 Underscore in Method Name](#119-underscore-in-method-name)</br>
&emsp;&emsp;[1.20 Alt Prop](#120-alt-prop)</br>
&emsp;&emsp;[1.21 Short-Circuit evaluation in JSX](#121-short-circuit-evaluation-in-jsx)</br>

</details>
<details>
  <summary>
    <a href="#2-nodejs-best-practices">2. TailwindCSS Best Practices</a>
  </summary>

</details>
<details>
  <summary>
    <a href="#3-nodejs-best-practices">3. NodeJS Best Practices</a>
  </summary>

</details>

<br/>
<hr />
<br/>

# `1. ReactJS Best Practices`

## `1.1 Use JSX ShortHand`

Try to use JSX shorthand for passing boolean variables. Let\'s say you want to control the title visibility of a Navbar component.

**Bad:**

```jsx
return <Navbar showTitle={true} />;
```

**Good:**

```jsx
return <Navbar showTitle />;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.2 Use Ternary Operators`

Let\'s say you want to show a particular user\'s details based on role.

**Bad:**

```jsx
const { role } = user;

if (role === ADMIN) {
  return <AdminUser />;
} else {
  return <NormalUser />;
}
```

**Good:**

```jsx
const { role } = user;

return role === ADMIN ? <AdminUser /> : <NormalUser />;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.3 Use Object Literals`

Object literals can help make our code more readable. Let\'s say you want to show three types of users based on their role. You can\'t use ternary because the number of options is greater than two.

**Bad:**

```jsx
const { role } = user;

switch (role) {
  case ADMIN:
    return <AdminUser />;
  case EMPLOYEE:
    return <EmployeeUser />;
  case USER:
    return <NormalUser />;
}
```

**Good:**

```jsx
const { role } = user;

const components = {
  ADMIN: AdminUser,
  EMPLOYEE: EmployeeUser,
  USER: NormalUser,
};

const Component = components[role];

return <Componenent />;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.4 Use Fragments`

Always use Fragment over Div. It keeps the code clean and is also beneficial for performance because one less node is created in the virtual DOM.

**Bad:**

```jsx
return (
  <div>
    <Component1 />
    <Component2 />
    <Component3 />
  </div>
);
```

**Good:**

```jsx
return (
  <>
    <Component1 />
    <Component2 />
    <Component3 />
  </>
);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.5 Do not define a function inside Render`

Don\'t define a function inside render. Try to keep the logic inside render to an absolute minimum.

**Bad:**

```jsx
return <button onClick={() => dispatch(ACTION_TO_SEND_DATA)}> // NOTICE HERE This is a bad example</button>;
```

**Good:**

```jsx
const submitData = () => dispatch(ACTION_TO_SEND_DATA);

return <button onClick={submitData}>This is a good example</button>;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.6 Use Memo`

React.PureComponent and Memo can significantly improve the performance of your application. They help us to avoid unnecessary rendering.

**Bad:**

```jsx
import React, { useState } from "react";

export const TestMemo = () => {
  const [userName, setUserName] = useState("faisal");
  const [count, setCount] = useState(0);

  const increment = () => setCount((count) => count + 1);

  return (
    <>
      <ChildrenComponent userName={userName} />
      <button onClick={increment}> Increment </button>
    </>
  );
};

const ChildrenComponent = ({ userName }) => {
  console.log("rendered", userName);
  return <div> {userName} </div>;
};
```

Although the child component should render only once because the value of count has nothing to do with the ChildComponent. But, it renders each time you click on the button.
Output

Let\'s edit the ChildrenComponent to this:

**Good:**

```jsx
import React, { useState } from "react";

const ChildrenComponent = React.memo(({ userName }) => {
  console.log("rendered");
  return <div> {userName}</div>;
});
```

Now no matter how many times you click on the button, it will render only when necessary.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.7 Use Object Destructuring`

Use object destructuring to your advantage. Let\'s say you need to show a user\'s details.

**Bad:**

```jsx
return (
  <>
    <div> {user.name} </div>
    <div> {user.age} </div>
    <div> {user.profession} </div>
  </>
);
```

**Good:**

```jsx
const { name, age, profession } = user;

return (
  <>
    <div> {name} </div>
    <div> {age} </div>
    <div> {profession} </div>
  </>
);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.8 String Props do not need Curly Braces`

When passing string props to a children component.

**Bad:**

```jsx
return <Navbar title={"My Special App"} />;
```

**Good:**

```jsx
return <Navbar title="My Special App" />;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.9 Remove JS Code From JSX`

Move any JS code out of JSX if that doesn\'t serve any purpose of rendering or UI functionality.

**Bad:**

```jsx
return (
  <ul>
    {posts.map((post) => (
      <li
        onClick={(event) => {
          console.log(event.target, "clicked!"); // <- THIS IS BAD
        }}
        key={post.id}>
        {post.title}
      </li>
    ))}
  </ul>
);
```

**Good:**

```jsx
const onClickHandler = (event) => {
  console.log(event.target, "clicked!");
};

return (
  <ul>
    {posts.map((post) => (
      <li onClick={onClickHandler} key={post.id}>
        {" "}
        {post.title}{" "}
      </li>
    ))}
  </ul>
);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.10 Use Template Literals`

Use template literals to build large strings. Avoid using string concatenation. It\'s nice and clean.

**Bad:**

```jsx
const userDetails = user.name + "'s profession is" + user.proffession;

return <div> {userDetails} </div>;
```

**Good:**

```jsx
const userDetails = `${user.name}'s profession is ${user.proffession}`;

return <div> {userDetails} </div>;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.11 Import in Order`

Always try to import things in a certain order. It improves code readability.

**Bad:**

```jsx
import React from "react";
import ErrorImg from "../../assets/images/error.png";
import styled from "styled-components/native";
import colors from "../../styles/colors";
import { PropTypes } from "prop-types";
```

**Good:**

The rule of thumb is to keep the import order like this:
Built-in
External
Internal
So the example above becomes:

```jsx
import React from "react";

import { PropTypes } from "prop-types";
import styled from "styled-components/native";

import ErrorImg from "../../assets/images/error.png";
import colors from "../../styles/colors";
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.12 Use Implicit return`

Use the JavaScript feature of implicit return to write beautiful code. Let\'s say your function does a simple calculation and returns the result.

**Bad:**

```jsx
const add = (a, b) => {
  return a + b;
};
```

**Good:**

```jsx
const add = (a, b) => a + b;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.13 Component Naming`

Always use PascalCase for components and camelCase for instances.

**Bad:**

```jsx
import reservationCard from "./ReservationCard";

const ReservationItem = <ReservationCard />;
```

**Good:**

```jsx
import ReservationCard from "./ReservationCard";

const reservationItem = <ReservationCard />;
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.14 Reserved Prop Naming`

Do not use DOM component prop names for passing props between components because others might not expect these names.

**Bad:**

```jsx
<MyComponent style="dark" />

<MyComponent className="dark" />
```

**Good:**

```jsx
<MyComponent variant="fancy" />
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.15 Quotes`

Use double quotes for JSX attributes and single quotes for all other JS.

**Bad:**

```jsx
<Foo bar='bar' />

<Foo style={{ left: "20px" }} />
```

**Good:**

```jsx
<Foo bar="bar" />

<Foo style={{ left: '20px' }} />
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.16 Prop Naming`

Always use camelCase for prop names or PascalCase if the prop value is a React component.

**Bad:**

```jsx
<Component UserName="hello" phone_number={12345678} />
```

**Good:**

```jsx
<MyComponent userName="hello" phoneNumber={12345678} Component={SomeComponent} />
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.17 JSX in Parentheses`

If your component spans more than one line, always wrap it in parentheses.

**Bad:**

```jsx
return;
<MyComponent variant="long">
  <MyChild />
</MyComponent>;
```

**Good:**

```jsx
return (
  <MyComponent variant="long">
    <MyChild />
  </MyComponent>
);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.18 Self-Closing Tags`

If your component doesn\'t have any children, then use self-closing tags. It improves readability.

**Bad:**

```jsx
<SomeComponent variant="stuff"></SomeComponent>
```

**Good:**

```jsx
<SomeComponent variant="stuff" />
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.19 Underscore in Method Name`

Do not use underscores in any internal React method.

**Bad:**

```jsx
const _onClickHandler = () => {
  // do stuff
};
```

**Good:**

```jsx
const onClickHandler = () => {
  // do stuff
};
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.20 Alt Prop`

Always include an alt prop in your `<img >` tags. And do not use picture or image in your alt property because the screenreaders already announce img elements as images. No need to include that.

**Bad:**

```jsx
<img src="hello.jpg" />

<img src="hello.jpg" alt="Picture of me rowing a boat" />
```

**Good:**

```jsx
<img src="hello.jpg" alt="Me waving hello" />
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## `1.21 Short-Circuit evaluation in JSX`

**Bad:**

```jsx
// Avoid
const sampleComponent = () => {
  return isTrue ? <p>True!</p> : null;
};
```

**Good:**

```jsx
// Recommended: short-circuit evaluation
const sampleComponent = () => {
  return isTrue && <p>True!</p>;
};
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
