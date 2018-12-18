With server-side rendering everything needs to be prepared and fetched before we send it to the router. If we don't do this in relation to css, you initially would see everything unstyled of you would use styled-components. 

In order to handle this, we need to include a custom document called `_document` to the `pages` folder :
```js
// import the document
import Document, { Head, Main, NextScript } from 'next/document';
import { ServerStyleSheet } from 'styled-components';

export default class MyDocument extends Document {
  static getInitialProps({ renderPage }) {
    const sheet = new ServerStyleSheet();
    // it renders out the app and crawl everysingle component and see if a style needs to be collected
    const page = renderPage(App => props => sheet.collectStyles(<App {...props} />));
    const styleTags = sheet.getStyleElement();
    return { ...page, styleTags };
  }

  // the steps above will happen before rendering happens that eventually will send it to the browser
  render() {
    return (
      <html>
        <Head>{this.props.styleTags}</Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </html>
    );
  }
}
```
