
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
The user's input is set in state, and when the input is submitted this state is pushed as props, which, via the `<BrowserRouter>` calls the `{SearchBar}` component. 


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

We then make a call to the Deezer API, using `axios` and setting the results in state. 

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

This checks the current props passed to the searchBar against the previous props that would have been passed to it 



## Challenges


## Successes


## Potential future features


## Lessons learned


















