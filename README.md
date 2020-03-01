
### ![GA](https://cloud.githubusercontent.com/assets/40461/8183776/469f976e-1432-11e5-8199-6ac91363302b.png) General Assembly, Software Engineering Immersive

# NOTTIFY 🎧
by  [Michael Adair](https://github.com/mjadair) and [Sandor Desi](https://github.com/dsandor87)


## Overview
[Check out Nottify here](https://mjadair.github.io/Nottify/)

My second project with General Assembly was a 48 hour 'hackathon' to build a multiple-page website using React which consumes a public API. This was a pair programming task.

My partner, [Sandor](https://github.com/dsandor87) and I decided that our biggest shared passion was music and that we would both be keen to make an app which displayed a wide range of artists/albums and tracks. Not Spotify. [Nottify]((https://mjadair.github.io/Nottify/)). 

## The Brief
The app should:

* **Consume a public API** – this could be anything but it must make sense for your project.
* **Have several components** - At least one classical and one functional.
* **The app should include a router** - with several "pages".
* Have **semantically clean HTML** - you make sure you write HTML that makes structural sense rather than thinking about how it might look, which is the job of CSS.
* **Be deployed online** and accessible to the public.

## Technologies Used


<li>Javascript (ES6)</li>
<li>React.js</li>
<li>Axios</li>
<li>SCSS</li>
<li>HTML5, JSX</li>
<li>Bulma, SCSS</li>
<li>Git and GitHub</li>
<li> [Deezer API](https://developers.deezer.com/api/)  </li>
<li>Google Fonts</li>


## Approach

### The Router

The app is utilises the React Router `<BrowserRouter>` to keep the UI in sync with the URL. 

```js
import { BrowserRouter, Switch, Route } from 'react-router-dom'

const App = () => (
  <BrowserRouter basename="/Nottify">
    <Navbar />
    <Switch>
      <Route exact path="/Home" component={Home} />
      <Route exact path="/Charts" component={Tracks} />
      <Route exact path="/search/:userInput" component={SearchBar} />
      <Route exact path="/:id/album" component={SelectedArtist} />
      <Route exact path="/:id/tracks" component={SelectedAlbum} />
      <Route exact path="/" component={Home} />
    </Switch>
  </BrowserRouter>
)
```

<img  src=src/img/Screenshots/DesktopHome.png width=500> <img  src=src/img/Screenshots/PhoneHome.png height=250> 


### The NavBar

The Navbar, which contains the search bar, is continuously rendered on the UI. This searchbar utilises a class component as below:

```js
class Navbar extends React.Component {

  constructor(props) {
    super(props)

    this.state = {
      userinput: ''
    }
    this.handleSearch = this.handleSearch.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }

  handleSearch(e) {
    this.setState({ userinput: e.target.value })
  }

  handleSubmit(e) {
    e.preventDefault()
    this.props.history.push(`/search/${this.state.userinput}`)
  }
```
The user's input is set in state, and when the input is submitted, this state is pushed as props, which, via the `<BrowserRouter>` calls the `{SearchBar}` component. 


### The SearchBar

The SearchBar is another class component which initially sets state as an object with an empty `results` value. 


```js
class SearchBar extends React.Component {

  constructor() {
    super()
    this.state = {
      results: null
    }
  }
```

We then make a call to the [Deezer API]((https://developers.deezer.com/api/)), using `axios` and setting the results in state. 

```js
  getData() {
    axios.get('https://cors-anywhere.herokuapp.com/api.deezer.com/search/artist/?q=' + this.props.match.params.userInput + '&index=0&limit=50&output=json')
      .then(res => {
        this.setState({ results: res.data.data })
      })
      .catch(err => console.log(err))
  }
```

Because we want the user to be able to consistently make requests via the SearchBar, we utilise the `componentDidUpdate()` method as below:

```js
 componentDidUpdate(prevProps) {
    if (prevProps.location.pathname !== this.props.location.pathname) {
      this.getData()
    }
  }
```

This checks the current props passed to the searchBar against the previous props that would have been passed to it and updates the component's state and re-renders accordingly. 

Here is how the page renders when the user searches for an artist in the search bar:

<img src=src/img/Screenshots/DesktopArtists.png width=500> <img src=src/img/Screenshots/PhoneArtists.png height=250>

### Loading

If the data from the API has yet to return and be set in state, then a gif is displayed in the render to indicate loading:

```js
 render() {
    if (!this.state.results) return <Loading />
```

<img src= src/img/vinylspin.gif width= 150>


### Other Class Components 

The other class components `selectedAlbum` and `selectedArtist` are similar to the search bar above. Props are passed to them through the `BrowserRouter` using the unique ID that is provided from the Deezer API. An example is below:

Once a user has searched for an artist, the application maps through each of the returned results and renders a 'card' displaying that artist. Selecting that card will route the user using the unique id of the selected item.

` <Link className="subtitle" to={${results.id}/album}>{results.name}</Link>`

This passes through the router here:

`<Route exact path="/:id/album" component={SelectedArtist} />`

The unique id forms the path and so can be used as the props passed to the api in our request for more information on that specific artist:

```js  
componentDidMount() {
    axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${this.props.match.params.id}/albums`)
      .then(resp => this.setState({ albumResults: resp.data.data }))
      .catch(err => this.setState({ err: err.response.status }))
  } 
```

### Functional Components and Styling

Functional components such as `ChartCard` and `TrackCard` are used to render the information from the API in a visually pleasing way. Due to this being a hackathon, we used the Bulma CSS Framework to keep our layout simple, clean and easily suitable for mobile use. 

An example of the `TrackCard` functional component is below: 

```js
import React from 'react'

const TrackCard = ({ trackResults }) => (
  <div className="column is-one-quarter-desktop is-one-third-tablet is-half-mobile">
    <div className='container'>
      <p>{trackResults.track_position}. {trackResults.title_short}</p>
      <div className="card-content">
        <audio src={trackResults.preview} controls />
      </div>
    </div>
  </div>
)
export default TrackCard
```

The `className` is dictated by the Bulma CSS framework whilst the `trackResults` prop is passed from the `selectedAlbum` component.

<img src=src/img/Screenshots/DesktopMenu.png width=500> 




## Challenges

- This was our first use of the React App that utilised multiple components and routing. Understanding how to pass props between components took a little time to for us to get to grips with. 


## Successes
- My first experience of Pair programming is something I particularly enjoyed. The spirit of collaboration and comporomise made this a really fun project to work on and I'm proud of the finished product given that it was our first time-limited 'hackathon'. 


## Potential future features
- We built this using class components, but I'd be interested in refactoring it to utilise React Hooks in future, which I believe is a cleaner, nicer way of setting and updating state. 

- Due to the time constraints, the search functionality currently only looks for Artists. It would be fun to make the search facility wider to include Tracks, Albums and Artists.


## Lessons learned

- First experience of Pair Programming
- How to successfully pass props between React Components
- How to update State within the same Component with `componentDidUpdate()`

















