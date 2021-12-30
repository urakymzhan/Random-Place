---
Display a Random Place
---

## Show a random public place

What if people wanted to find new places or just see something random? You could make a button to do this.

To generate a random number with JavaScript you can use `Math.random()`. This method returns a random number between 0.0 and 1.0. It doesn't take any parameters or provide any other options. Typical output might look like this:

- 0.18688631567790337
- 0.2833598281388573
- 0.07430992503059852
- 0.3424389185783543
- 0.780044321714769
- 0.10202516555210439

I generated these numbers with `Math.random()`. If you need numbers that look different from this you'll need to do some work.

First you can multiply the value by the range of numbers you need. For example: `Math.random() * 10` might output:

- 3.9817693263500997
- 1.9900180705556125
- 6.624325586714287
- 6.776353541892094
- 9.51050569288244
- 9.292188988843789

Now you need to round the numbers off. JS supplies three choices for rounding:

- `Math.floor()` - rounds down `Math.floor(3.9817693263500997) // 3`
- `Math.ceil()` - rounds up `Math.floor(9.292188988843789) // 10`
- `Math.round()` - rounds up/down
- `Math.floor(9.51050569288244) // 10`
- `Math.floor(9.292188988843789) // 9`

The situation we are in provides us with an array of places and we need to select a random place from the array. The number of places in the array is provided by the length:

```JS
data.length
```

The index of elements in an array start at 0 and end at length - 1. So `Math.floor()` might be our best choice! Here is some sample code:

```JS
const index = Math.floor(Math.random() * data.length)
```

Or we could break that up across several lines to make it easier to read:

```JS
const length = data.length
const randomNumber = Math.random() * length
const index = Math.floor(randomNumber)
// get a random place with: data[index]
```

Since we're using react Router and have a route to show a detail page with an index if we generate a route with that index we can show the page. For example: `http://localhost:3000/?#/details/2` if the 2 were a random number it would display a random page.

```JS
const length = data.length
const randomNumber = Math.random() * length
const id = Math.floor(randomNumber)
// Router could work with this:
// /details/:id
```

**IMPORTANT NOTE**

A quick aside, I'm regretting naming the the compoennt Title, when that component became the Header or PageHeader. This would a good place to rename these! <br>
So lets rename Title component to be a PageHeader. Fix import errors. From now on i will refer Title as PageHeader. Update classNames to from **Title** to **PageHeader** in `PageHeader.css` and `PageHeader.js`

### React Router History

Displaying a new random page requires we use React Router. Remember React Router manages the "Pages" in our Single Page Site.

The method I chose was `history`. Using history will allow random searches to be tracked by the history and users will be able to use the back button in the browser to go back to the previous place they viewed.

Now we need a button or link to initiate the action. I added a button to the `PageHeader` component. You could put this button anywhere. So our strategy will be to make a component that is just the button that loads a random place and then we can import that button where ever we might want to use it.

> [action]
>
> Make a new folder: `components/RandomPlace/`
>
> Next make a new file: `RandomPlace/RandomPlace.js`
>
> Now add the following to `RandomPlace.js`:

> If you use react-router v5 use useHistory instead of useValidate

```JS
import { useValidate } from 'react-router-dom'
import './RandomPlace.css';
import data from '../../places-data.json'

function RandomPlace() {
    const navigate = useNavigate();
  return (
        <button onClick={(e) => {
            const id = Math.floor(Math.random() * data.length)
            navigate(`/details/${id}`)
        }}>Show me a random place</button>
  )
}

export default RandomPlace
```

This creates a component that outputs a single button.

I imported `useNavigate`. `useNavigate` is a 'hook' that we can use with React Router to navigate to a new page.

I also imported `places-data.json` as `data` since I'll need to know the length of the list.

The code here handles a click on the button with `onClick`. Without all of the extra code the button might look like:

```JS
<button onClick={() => {
    // run some code here when the clicked
}}>Show me a random Space</button>
```

The code above uses an arrow function. These are similar to regular functions:

```JS
// Old school function
function(a,b,c) {

}

// Arrow function
(a,b,c) => {

}
```

Inside the onClick function the code gets a random number from 0 to the length - 1 as id. Then uses `navigate()` to navigate to the details page with the random id.

```JS
const id = Math.floor(Math.random() * data.length)
navigate(`/details/${id}`)
```

### Use the new RandomPlace component

Use the new component like any other component.

- Import
- then put it in your JSX

Lets add a RandomPlace button to the PageHeader.

> [action]
>
> Edit `PageHeader.js`. Add the following at the top and adjust the path to fit your arrangement of files.

```JS
import RandomPlace from '../RandomPlace/RandomPlace'
```

> Inside the function that defines the component find a place to add the new button component:

```JS
function PageHeader() {
  return (
    <div className="PageHeader">
      <header>
        ...
        <div>
          ...
          <RandomPlace />
        </div>
      </header>
    </div>
  )
}
```

>

I put the random place button after the list of NavLinks. You could put it anywhere.

### Style the RandomPlace button

Add a style sheet and style the RandomPlace button. You style the button. Follow these steps:

- create a style sheet `RandomPlace.css`
- import your style sheet into `RandomPlace.js`
- add some styles...

My Example:

```CSS
.RandomPlace {
    background-color: transparent;
    padding: 0.5em 1em;
    color: rgba(255, 255, 255, 0.835);
    font-weight: bold;
    border: 1px solid rgba(255, 255, 255, 0.835);
    border-radius: 5px;
    cursor: pointer;
    margin-left: 10px;
}
```

# Now Commit

> [action]

```bash
$ git add .
$ git commit -m 'add random places'
$ git push
```
