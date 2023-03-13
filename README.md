# Koala Fintech Frontend
*Based off Atomic React (**ARc**)*




## Setup
**1. Install dependencies**

    npm install

**2. Run the app**

    npm run dev

It will start the development server with HMR on top of it.

http://localhost:3000 — Development server
http://localhost:3001 — Webpack assets server (for redux-ssr only)

*Now you can open http://localhost:3000 in browser and start developing.*

## Story Book
During the creation of a new component, you'll want to test it on browser. Usually, you'll render it in a top level component with every possible state so you can check if it's working properly:
```jsx
const HomePage = () => (
  <PageTemplate>
    ...
    <MyNewAwesomeComponent />
    <MyNewAwesomeComponent brilliant />
    <MyNewAwesomeComponent sexy />
    <MyNewAwesomeComponent brilliant sexy />
    ...
  </PageTemplate>
)
```
Once you've finished the work, you need to remove that component from `HomePage`. But, as we all know, 2 weeks later you'll come back to change something, add a new state, fix some bugs etc. And, when that happens, you'll need to repeat that task, rendering every state of that component, to make sure the new and the old states are working.

Fortunately, [storybook](https://github.com/storybooks/storybook) solves this problem by letting you to write every state of your component on a file and keep it there so you can always use it.
```jsx
storiesOf('MyNewAwesomeComponent', module)
  .add('default', () => (
    <MyNewAwesomeComponent />
  ))
  .add('brilliant', () => (
    <MyNewAwesomeComponent brilliant />
  ))
  .add('sexy', () => (
    <MyNewAwesomeComponent sexy />
  ))
  .add('brilliant and sexy', () => (
    <MyNewAwesomeComponent brilliant sexy />
  ))
```

Then, just use `npm run storybook`.

## Workflow
This template is structured in a way that makes it easier to work on different parts of the application with distinct focuses. It is based off of ARc.

When introducing a new feature or even starting the project from scratch:

1. **Create [`presentational components`](https://github.com/diegohaz/arc/wiki/Atomic-Design)**

    You'll usually start by writing dumb components, just focusing on presentational logic. [`Storybook`](https://github.com/diegohaz/arc/wiki/Storybook) can help you with that. At this point, you can use [`generact`](https://github.com/diegohaz/generact) to generate new components and improve your productivity:

    ![image](https://cloud.githubusercontent.com/assets/3068563/26834556/0beafcb4-4aac-11e7-8918-ee6ca974ccc3.png)

2. **Create [`containers`](https://github.com/diegohaz/arc/wiki/Containers)**
  
    Then, you can focus on what that feature will do with the app state ([`actions`](https://github.com/diegohaz/arc/wiki/Actions)) and which part of the state it will need ([`selectors`](https://github.com/diegohaz/arc/wiki/Selectors)). [`generact`](https://github.com/diegohaz/generact) can also help you here.

3. **Write [`redux module tests`](https://github.com/diegohaz/arc/wiki/Testing-redux-modules)** (recommended)

    After creating containers, you'll have a good sense of what [`actions`](https://github.com/diegohaz/arc/wiki/Actions) and [`selectors`](https://github.com/diegohaz/arc/wiki/Selectors) you'll need. You can write integration tests to make sure that you'll have proper state when dispatching an action.

4. **Create [`redux modules`](https://github.com/diegohaz/arc/wiki/Redux-modules)**

    The last step is to figure out how [`reducers`](https://github.com/diegohaz/arc/wiki/Reducers) and/or [`sagas`](https://github.com/diegohaz/arc/wiki/Sagas) will respond to dispatched actions so you can modify the app state and/or make side effects.

## Atomic Design
This project leverages the [Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) methodology to create a scalable and easy to maintain component folder structure.

> In short, Atomic Design is a methodology proposed by [**@bradfrost**](https://github.com/bradfrost) by which you can better organize your UI components. It can be done by separating components in `atoms`, `molecules`, `organisms`, `pages` and `templates`. 

However, these are not rules you must follow to the letter. Just take them as a basis for your project and adapt it for what better fits your use case (see [this discussion](https://github.com/diegohaz/arc/issues/50))

### Atoms

An atom is a native html tag, a React Component that renders an html tag or any third party component:

<sub>**atoms/Input.js**</sub>
```jsx
const Input = props => <input {...props} />
```
<sup>See [src-example/components/atoms/Input/index.js](https://github.com/diegohaz/arc/blob/master/src-example/components/atoms/Input/index.js)</sup>

<sub>**atoms/Label.js**</sub>
```jsx
const Label = props => <label {...props} />
```
<sup>See [src-example/components/atoms/Label/index.js](https://github.com/diegohaz/arc/blob/master/src-example/components/atoms/Label/index.js)</sup>

### Molecules

A molecule is a group of atoms:

<sub>**molecules/Field.js**</sub>
```jsx
const Field = ({ label, ...inputProps }) => (
  <Label>
    {label}
    <Input {...inputProps} />
  <Label>
)
```
<sup>See [src-example/components/molecules/Field/index.js](https://github.com/diegohaz/arc/blob/master/src-example/components/molecules/Field/index.js)</sup>

### Organisms

An organism is a group of atoms, molecules and/or other organisms:

<sub>**organisms/Form.js**</sub>
```jsx
const Form = (props) => (
  <form {...props}>
    <Field label="Name" type="text" />
    <Field label="Email" type="email" />
  <form>
)
```
<sup>See [src-example/components/organisms/PostForm/index.js](https://github.com/diegohaz/arc/blob/redux/src-example/components/organisms/PostForm/index.js)</sup>

### Pages

A page is... well, a page, where you will put mostly (but not exclusively) organisms:

<sub>**pages/HomePage.js**</sub>
```jsx
const HomePage = () => (
  <PageTemplate header={<Header />}>
    <Form />
  </PageTemplate>
)
```
<sup>See [src-example/components/pages/HomePage/index.js](https://github.com/diegohaz/arc/blob/master/src-example/components/pages/HomePage/index.js)</sup>

### Templates

A template is a layout to be used on pages, see [why templates are good practice](https://github.com/diegohaz/arc/issues/20#issuecomment-265934388):

<sub>**templates/PageTemplate.js**</sub>
```jsx
const PageTemplate = ({ header, children }) => (
  <main>
    {header && <div>{header}</div>}
    {children}
  </main>
)
```
<sup>See [src-example/components/templates/PageTemplate/index.js](https://github.com/diegohaz/arc/blob/master/src-example/components/templates/PageTemplate/index.js)</sup>

---------------

### Do not worry

Atomic Design should be a solution, **not another problem**. If you want to create a component and don't know where to put it (`atoms`, `molecules`, `organisms` etc.), **do not worry, do not think too much, just put it anywhere**. After you realize what it is, just move the component folder to the right place. Everything else should work.

<p align="center"><img src="https://cloud.githubusercontent.com/assets/3068563/21237760/6b941f76-c2e7-11e6-92e3-bbb7c82b3622.gif"></p>

This is possible because all components are dynamically exported on [`src-example/components/index.js`](https://github.com/diegohaz/arc/blob/master/src-example/components/index.js) and imported in a way that Atomic Design structure doesn't matter:

```js
import { Input, Label, Field, HomePage, PageTemplate } from 'components'
```

> Keep in mind that that approach comes with some drawbacks. Most developers will not fight it though. See [#131](https://github.com/diegohaz/arc/issues/131)

## Styling 
ARc combines [`styled-components`](https://github.com/styled-components/styled-components) with [`styled-tools`](https://github.com/diegohaz/styled-tools) and [`styled-theme`](https://github.com/diegohaz/styled-theme) for styling components.

You can refer to their GitHub repositories or to the components on [`src-example/components`](https://github.com/diegohaz/arc/tree/master/src-example/components) so you can learn how to use them properly. Here's a basic usage:

```js
import styled from 'styled-components'
import { font, palette } from 'styled-theme'

const Atom = styled.span`
  font-family: ${font('primary')};
  color: ${palette({ grayscale: 0 }, 1)};
`
```

### Theming

Use [`src/components/themes/default.js`](https://github.com/diegohaz/arc/tree/master/src/components/themes/default.js) to define a default theme for your application.

## Testing components

ARc uses [Jest](http://facebook.github.io/jest/) and [Enzyme](http://airbnb.io/enzyme/) for testing components.

Each test file lives inside the component folder with a standard name (`index.test.js`). That's a basic and common test:
```jsx
import React from 'react'
import { shallow } from 'enzyme'
import Component from '.'

it('renders children when passed in', () => {
  const wrapper = shallow(<Component>test</Component>)
  expect(wrapper.contains('test')).toBe(true)
})
```

All components except the one being tested are mocked in [`test/componentsMock.js`](https://github.com/diegohaz/arc/blob/1e94fd0f817f866bc99f31dd8cadb653064b5d12/test/componentsMock.js). That makes it easier to write real unit tests.

## Example components
The best way to understand the example components is through [`storybook`](https://github.com/diegohaz/arc/wiki/Storybook):
```sh
npm run storybook:example
```
Now you can open http://localhost:9001 on browser and navigate through components to see their possible states. You can also see their code on `index.stories.js` inside each component folder.

Here's some components that could be tricky to use:

### [`Icon`](https://github.com/diegohaz/arc/tree/master/src-example/components/atoms/Icon)

To add a new icon, just put `share.svg` file into the `icons` folder. Then, you can use it as `<Icon icon="share" />`.

To be able to have dynamic colors (i.e. changing SVG `fill` and `stroke` properties based on the `color` of the parent element) on the svg file, you'll probably need to change the svg code a little.

Consider the following SVG:
```xml
<svg xmlns="http://www.w3.org/2000/svg" version="1.0" x="0px" y="0px" width="24px" height="24px" viewBox="0 0 24 24">
<circle style="fill:#000000" cx="20" cy="4" r="4" fill="#000000"/>
<circle style="fill:#000000" cx="20" cy="20" r="4" fill="#000000"/>
<circle style="fill:#000000" cx="4" cy="12" r="4" fill="#000000"/>
<polyline style="fill:none;stroke:#000000;stroke-width:2;stroke-miterlimit:10" points="20,4 4,12 20,20 " fill="#000000"/>
</svg>
```

If we render it with the following rules:
```jsx
const Paragraph = styled.p`
  color: red;
`

<Paragraph><Icon icon="share" /></Paragraph>
```
It will not render as we expect:

![image](https://cloud.githubusercontent.com/assets/3068563/26010494/79299822-3723-11e7-97ed-8d008b75f04b.png)

It happens because the SVG already has `fill` and `stroke` properties set to `#000000`. Let's remove them so we can control it from the `Icon` component:

```diff
  <svg xmlns="http://www.w3.org/2000/svg" version="1.0" x="0px" y="0px" width="24px" height="24px" viewBox="0 0 24 24">
- <circle style="fill:#000000" cx="20" cy="4" r="4" fill="#000000"/>
+ <circle cx="20" cy="4" r="4"/>
- <circle style="fill:#000000" cx="20" cy="20" r="4" fill="#000000"/>
+ <circle cx="20" cy="20" r="4"/>
- <circle style="fill:#000000" cx="4" cy="12" r="4" fill="#000000"/>
+ <circle cx="4" cy="12" r="4"/>
- <polyline style="fill:none;stroke:#000000;stroke-width:2;stroke-miterlimit:10" points="20,4 4,12 20,20 " fill="#000000"/>
+ <polyline style="fill:none;stroke-width:2;stroke-miterlimit:10" points="20,4 4,12 20,20 "/>
  </svg>
```

Now it renders as we want (red), but there're still red strokes around circles:

![image](https://cloud.githubusercontent.com/assets/3068563/26010723/73f8b2b0-3724-11e7-858d-509334f2d080.png)

That happens because `Icon` sets not only `fill` but also `stroke` color with the `currentcolor` value. We need to set `stroke="none"` to fix that:

```diff
  <svg xmlns="http://www.w3.org/2000/svg" version="1.0" x="0px" y="0px" width="24px" height="24px" viewBox="0 0 24 24">
- <circle cx="20" cy="4" r="4"/>
+ <circle cx="20" cy="4" r="4" stroke="none"/>
- <circle cx="20" cy="20" r="4"/>
+ <circle cx="20" cy="20" r="4" stroke="none"/>
- <circle cx="4" cy="12" r="4"/>
+ <circle cx="4" cy="12" r="4" stroke="none"/>
  <polyline style="fill:none;stroke-width:2;stroke-miterlimit:10" points="20,4 4,12 20,20 "/>
  </svg>
```

And now it renders properly:

![image](https://cloud.githubusercontent.com/assets/3068563/26010945/5c1cc13a-3725-11e7-9daf-6483246b4dcb.png)
