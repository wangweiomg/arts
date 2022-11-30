## Learn - React-Hook useState

### class写法

```js
import React, { Component } from 'react'

export default class App extends Component {

    state = {
        count: 0
    }

    setCount = ()=>{
        this.setState(state=>({
            count: ++state.count
        }))
    }

  render() {
    return (
      <div>
        <p>you click {this.state.count} times</p>
        <button onClick={this.setCount}>
            Click me!
        </button>
      </div>
    )
  }
}

```



### hook写法

```js
import React, { useState } from 'react'

export default function App() {

    const [count, setCount] = useState(0)
    
    return (
        <div>
            <p>you click {count} times</p>
            <button onClick={()=> setCount(count+1)}>
                Click me
            </button>
        </div>
    )

}

```

