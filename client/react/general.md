# Components
- Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
- React lets you define components as classes or functions.

## class nameComponent extends React.Component {} 
- The only method you must define in a React.Component subclass is called `render()`. All the other methods described are optional.
- `setState()` enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the updated state. This is the primary method you use to update the user interface in response to event handlers and server responses.
- `The state` contains data specific to this component that may change over time. The state is user-defined, and it should be a plain JavaScript object.

#### constructor(props) {}
- Constructor is the only place where you should assign this.state directly. In all other methods, you need to use this.setState() instead.
- Avoid copying props into state. 

#### render(){}
The render() method is the only required method in a class component. When called, it should examine this.props and this.state and return one of the following types:

    - React elements. Typically created via JSX. For example, <div /> and <MyComponent /> are React elements that instruct React to render a DOM node, or another user-defined component, respectively.
    - Arrays and fragments. Let you return multiple elements from render. See the documentation on fragments for more details.
    - Portals. Let you render children into a different DOM subtree. See the documentation on portals for more details.
    - String and numbers. These are rendered as text nodes in the DOM.
    - Booleans or null. Render nothing. (Mostly exists to support return test && <Child /> pattern, where test is boolean.)


### ReactDOM.render()

```jsx
import React, { Component} from 'react';
import ReactDOM from 'react-dom';
import YTSearch from 'youtube-api-search';
import lodash from 'lodash';
import SearchBar from './components/search_bar';
import VideoList from './components/video_list';
import VideoDetail from './components/video_detail';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      videos: [],
      selectedVideo: null
    };

    this.videoSearch('surfborads');
  }

    videoSearch(term) {
      YTSearch({key: process.env.API_KEY, term: term}, (videos) => {
        this.setState({
          videos, videos,
          selectedVideo: videos[0]
         });
      });
    }

  render() {
    const videoSearch = lodash.debounce((term) => { this.videoSearch(term) }, 300);

    return (
      <div>
        <SearchBar onSearchTermChange={videoSearch} />
        <VideoList
          onVideoSelect={selectedVideo => this.setState({selectedVideo})}
          videos={this.state.videos}/>
        <VideoDetail video={this.state.selectedVideo}/>
      </div>
    );
  }
}

ReactDOM.render(<App />, document.querySelector('.container'));
```
