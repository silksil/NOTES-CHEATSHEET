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
