With styled components you write the styles of the application in the javascript application. Usually their are coupled with the components that need to be created. The biggest advantage is that you have no (or less)global styling, reducing the risk that a class or selector overwrite component it shouldn't apply too. 

To do this we use the library `styled-components`.  With styled-componets we can create inline an html attribute (e.g. `styled.div`), and give it css. We can import this html component with css in our app. 

```js
import React, { Component } from 'react';
import styled, { ThemeProvider, injectGlobal } from 'styled-components';
import Header from './Header';
import Meta from './Meta';

const theme = {
  red: '#FF0000',
  black: '#393939',
  grey: '#3A3A3A',
  lightgrey: '#E1E1E1',
  offWhite: '#EDEDED',
  maxWidth: '1000px',
  bs: '0 12px 24px 0 rgba(0, 0, 0, 0.09)',
};

const StyledPage = styled.div`
  background: white;
  color: ${props => props.theme.black};
`;

const Inner = styled.div`
  max-width: ${props => props.theme.maxWidth};
  margin: 0 auto;
  padding: 2rem;
`;

class Page extends Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <StyledPage>
          <Meta />
          <Header />
          <Inner>{this.props.children}</Inner>
        </StyledPage>
      </ThemeProvider>
    );
  }
}

export default Page;
```
If you use a styled component multiple times, it's convention to put the styled component in seperate folder. 
```js
import styled from 'styled-components';

const CloseButton = styled.button`
  background: black;
  color: white;
  font-size: 3rem;
  border: 0;
  position: absolute;
  z-index: 2;
  right: 0;
`;

export default CloseButton;
```
You can have a variable, you can declare the variables in a seperate part and include it in a theme, to reuse that variable throughout the application. We start with creating the theme (include strings, because it's not a styled component):
```js
const theme = {
  red: '#FF0000',
  black: '#393939',
  grey: '#3A3A3A',
  lightgrey: '#E1E1E1',
  offWhite: '#EDEDED',
  maxWidth: '1000px',
  bs: '0 12px 24px 0 rgba(0, 0, 0, 0.09)',
};
``` 
We import the `ThemeProvider` and wrap the entire application around this `Themeprovider` and pass it the `theme`. The `Themeprovider` uses the React Context Api; it allows you to specify variables uphigh (e.g. the theme we passed) and any component can than access that variable, without having to passing it from component to component. We pass the props 
```js
import styled, { ThemeProvider } from 'styled-components';

class Page extends Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <StyledPage>
          <Meta />
          <Header />
          <Inner>{this.props.children}</Inner>
        </StyledPage>
      </ThemeProvider>
    );
  }
}

export default Page;
```
You can then access the variables in `theme` through `.prop.theme` if you wrapped the `ThemeProvider` around it. :
```js
const StyledPage = styled.div`
  background: white;
  color: ${props => props.theme.black};
`;
```




