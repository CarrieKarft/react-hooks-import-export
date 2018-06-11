# React Modular Code

## Introduction
In this lesson we'll discuss the ES6 keywords `import` and `export`, and how they allow us to share modularized code across multiple files within a JavaScript application.

## Objectives
1. Understand why it's important to split up our code into smaller files (modules)
2. Learn how `import` and `export` support our ability to build modular code
3. Understand the different ways to import and export code


## Modular Code

Maintaining single-responsibility is key to writing clean and DRY code. As our applications grow in size, it's important to separate our code into easy-to-read, reusable segments. This separation makes our programs simpler to navigate and our code quicker to debug.

Using React, we have available to us multiple ways to define components. The most common way uses the React class component syntax:

```js
class Hogwarts extends React.Component {
  render() {
    return (
      <div className="Hogwarts">
        "Harry. Did you put your name in the Goblet of Fire?"
      </div>
    )
  }
}
```

Since React applications can become rather large, we want to make sure we keep them organized. In an effort to do just this, it's standard practice to give each of these components their own file. It is not uncommon to see a React program file tree that looks something like this:

```bash
├── README.md
├── public
└── src
     ├── App.js
     ├── Hogwarts.js
     └── Houses.js
```

In the example above we see that our components are modular <!-- TODO: " because..." -->(i.e., they have their own files). Now, all we have to do is figure out how to access the code defined in one file within a different file. Well, this is pretty easy to do in React! Introducing IMPORT EXPORT!

![import-meme](https://memegenerator.net/img/instances/11027875/yo-dawg-we-heard-you-like-to-import-data-so-we-put-an-export-feature-into-your-data-import-maps-so-y.jpg)


### Import and Export

<!-- TODO: enable us to use... -->
On a fundamental level, `import` and `export` enable us to use modules in other modules, which becomes increasingly important as we build out larger programs. <!-- TODO: why --> 

<!-- TODO: does not reduce the number of bugs, but instead makes it easier to debug -->
Sectioning off our code into smaller components is good practice, as it supports the single-responsibility principle as well as inherently reducing the number of bugs. Can you imagine trying to find one line that's breaking our entire program, when there are 1000 lines of code?

Let's look at an example of how importing/exporting can be used from a high level. Circling back to our Hogwarts file tree:

```bash
├── README.md
├── public
└── src
    ├── App.js
    ├── Hogwarts.js
    └── houses
        ├── Gryffindor.js
        ├── Slytherin.js
        ├── Hufflepuff.js
        ├── Ravenclaw.js
        └── HagridsHouse.js
```

Hogwarts School of Witchcraft and Wizardry has four houses that make up its student and teacher population. If we were making a React App, we might want to have the `Hogwarts` component make use of every house component. To do this, we would need to make sure to `export` the house components so they are available for use (via `import`) in the rest of our React application. The code might look like this:

First, we `export`:

```js
// src/houses/Gryffindor.js
import React from 'react'

// make note of the `export` keyword below
export default class Gryffindor extends React.Component{

  render() {
    return (
      <div>GRYFFINDOR, HOME TO HERMIONE GRANGER</div>
    )
  }
}
```

...and then, we `import`:

```js
// src/Hogwarts.js

import React from 'react'
import Gryffindor from './houses/Gryffindor'
import Slytherin from './houses/Slytherin'
import Ravenclaw from './houses/Ravenclaw'
import Hufflepuff from './houses/Hufflepuff'

export default class Hogwarts extends React.Component {
  render() {
    return (
      <div>
        <Gryffindor />
        <Ravenclaw />
        <Hufflepuff />
        <Slytherin />
      </div>
    )
  }
}
```

<!-- TODO: "if we did not import those components, but attempted to use them in our Hogwarts render() method above, our program would error because it doesn't know what Gryffindor/Ravenclaw/etc is" -->

We `import` and `export` local files by declaring their relative path to the file that we are currently in. We do this to ensure that we are accurately referencing the correct file.

Notice, at the top of our React component files, we are importing React from `'react'`. This is not magic. All we are doing is referencing the React library, located inside the `node_modules` folder. 

<!-- TODO: a quick mention on node_modules folder here. "its not uncommon for React web apps to make use of packages, or bundles of third party code (afterall, React itself is third party code). `node_modules` is a specific folder in node/react projects that holds such packages. `import x from y` will look for (y) that specific package in that folder." -->

### `export Default`

<!-- TODO: oops -->

We use `export default` to move the entirety of a file, whether that be a single function or an entire component, and access its content from other locations in our program.

To do this, we call `export default` on a reference to what we want to export. This can be done when defining the class itself such as `export default class Hogwarts extends React.Component {}` or by calling `export default Hogwarts` at the end of the file.

```js
// src/houses/HagridsHouse.js
import React from 'react';

function whoseHouse() {
  console.log(`HAGRID'S HOUSE!`)
}

export default whoseHouse;
```

We can then use `import` to make use of that function elsewhere. Default export allows us to name the exported code whatever we want when importing it. For example, `import nameThisAnything from './HagridsHouse.js'` will provide us with the same code as `import whoseHouse from './HagridsHouse.js'`-- this is called aliasing!

```js
// src/Hogwarts.js
import whoseHouse from './house.js';
import ReactDOM from 'react-dom';

// TODO: remove reactDOM.render

ReactDOM.render(
  whoseHouse()
  // > `HAGRID'S HOUSE!`,
  document.getElementById('root')
);

```

<!-- TODO: if we can export default functions, we can export default components! like so... (replace following bit) -->
We can also use `export default` to extract entire components from their respective files, like so:

```js
// src/houses/Hufflepuff.js
import React from 'react';

export default class Hufflepuff extends React.Component{
  render() {
    return (
      <div>
        NOBODY CARES ABOUT US
      </div>
    )
  }
}
```

Then, we can import the entire component to any other file in our application, using whatever naming convention that we see fit:

```js
// src/Hogwarts.js
import React from 'react';
import HooflePoof from './houses/Hufflepuff.js';

export default class Hogwarts extends React.Component{
  render(){
    return(
      <div>
        <HooflePoof/>
        //> Will render `NOBODY CARES ABOUT US`, regardless that we renamed `Hufflepuff` to `HooflePoof`
      </div>
    )
  }
}

```


### Named Exports

<!-- needs rework on why named export/import is useful -->

`export default` is great because it allows us to TODO: FILL IN,... Named exports allow us to export several specific things at once.

```js
// src/houses/Gryffindor.js

export function colors() {
  console.log("Scarlet and Gold")
}

function values() {
  console.log("Courage, Bravery, Nerve and Chivalry")
}

export function mascot() {
  console.log("The Lion")
}
```

We can then `import` specific functions from a file via identifying them by name. Let's look at an example:

```js
// src/Hogwarts.js
import { colors, mascot } from './houses/Gryffindor.js'

colors()
// > 'Scarlet and Gold'

mascot()
// > 'The Lion'

values()
// > ReferenceError: values is not defined
```

Now we're going to go import ourselves to Platform 9 3/4!!!

<!-- TODO: don't forget correct syntax when importing and exporting, otherwise you will feel like this idiot Ron. -->

![import-meme](https://collegecandy.files.wordpress.com/2015/02/toptenthingsonmyharrypotterbucketlist7.gif?w=639&h=235)

<!-- TODO: Recap Section -->
> what export/import do for us
> how we export (default or not)
> how we import (named or not)


## External Resources

Understanding how to create absolute paths is outside the scope of this lab, but for further reading check out: https://coderwall.com/p/th6ssq/absolute-paths-require.
