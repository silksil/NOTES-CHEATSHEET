# Components
- Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
- React lets you define components as classes or functions.
- Whether you declare a component as a function or a class, it must never modify its own props. It should be a `pure function`

## Element
An element describes what you want to see on the screen:
`const element = <h1>Hello, world</h1>;`

- Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements. Elements are what components are “made of”. 
- React elements are immutable. Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

## React.Component
- The only method you must define in a React.Component subclass is called `render()`. All the other methods described are optional.
- `setState()` enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the updated state. This is the primary method you use to update the user interface in response to event handlers and server responses.
- `The state` contains data specific to this component that may change over time. The state is user-defined, and it should be a plain JavaScript object.

#### constructor(props) {}
- When the component class is being created, the constuctor is called first. So it's the right place initialize everything - including state. 
- Constructor is the only place where you should assign this.state directly. In all other methods, you need to use this.setState() instead.
- Avoid copying props into state. 
- The super keyword is used to access and call function in an object's parent. 

#### render(){}
- The render() method is the only required method in a class component. When called, it should examine this.props and this.state and return one of the following types:
    - React elements. Typically created via JSX. For example, <div /> and <MyComponent /> are React elements that instruct React to render a DOM node, or another user-defined component, respectively.
    - Arrays and fragments. Let you return multiple elements from render. See the documentation on fragments for more details.
    - Portals. Let you render children into a different DOM subtree. See the documentation on portals for more details.
    - String and numbers. These are rendered as text nodes in the DOM.
    - Booleans or null. Render nothing. (Mostly exists to support return test && <Child /> pattern, where test is boolean.)

## ReactDOM.render()
ReactDOM.render() controls the contents of the container node you pass in. Any existing DOM elements inside are replaced when first called. Later calls use React’s DOM diffing algorithm for efficient updates.

## Example
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
        <SearchBar onSearchTermChange={videoSearch}/>
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
## Passing props to child component
The example below displays how a property can be passed on to a child componnent. In this case, the RenderWeather function receives multiple objects that contain weather data regarding a specific city. For that city we want to create seperate charts that display the temp, humidity and pressure. To avoid duplication, we create a `Chart` component that can be used to display the data of the three paramaters.
#### Mother component
```jsx
import Chart from '../components/chart';

renderWeather(cityData) {
 const name = cityData.city.name;
 const temp = cityData.list.map(weather => weather.main.temp);
 const pressure = cityData.list.map(weather => weather.main.pressure);
 const humidity = cityData.list.map(weather => weather.main.humidity);

 return (
 <tr key={name}>
  <td>{name}</td>
  <td> <Chart data={temp} color="orange"/> </td>
  <td> <Chart data={pressure} color="red"/> </td>
  <td> <Chart data={humidity} color="blue"/> </td>
 </tr>
  );
}
```
#### Child component
```jsx
import _ from 'lodash';
import React from 'react';
import { Sparklines, SparklinesLine, SparklinesReferenceLine } from 'react-sparklines';

export default (props) => {
 function average(data) {
  return _.round(_.sum(data)/data.length);
}
return (
 <div>
  <Sparklines height={110} width={180} data={props.data}>
   <SparklinesLine color={props.color}/>
   <SparklinesReferenceLine type="avg"/>
  </Sparklines>
  <div>{average(props.data)} {props.units}</div>
 </div>
 );
}
```

