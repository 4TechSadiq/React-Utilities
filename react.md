o# React 
### Applying style in react page

### React Hooks

### Lifecycle of components
- Mounting Phase(Putting component into DOM)
- Updating Phase
- Unmounting Phase
- These combined phase is called lifecycle of component.

- inline css
  <h3 style={{marginLeft:100, color:"red"}}>Complete details</h3>

- internal css
  before return function of js, make a variable 
  { const style = {
          fontSize: 30px,
          color: "green",
          ........
          }
        }
  <h3 style={style}>Complete details</h3>

- External css
  - make external css file
  - use className inside the tag and give class
  - import and use

- to display image in react
  - give image tag to where you want to display image
  - info : in jsx, every tag should be closed by <tagname /> or <tagname></tagname> else error will be generated
  
### React Hooks
  - Introduced after react 16.
#### UseState() Hook
  - it is used to change the state of a component
  - Example, when clicking a button , value will increment, when button click color change etc
  - it can be used to change a state of a component from one state to another
  - Suntax useState in function
  - ```js
    const [state, setState] = useState(0)

    return (
      <>
      <h2>Usestate</h2>
      <h2>current state: {state}</h2>
      <button onClick={()=>{setState(state+1)}}>add state</button>
      </>
    )

  - Syntax by using class
    ```js
      function handleClick(){
      seState(state+1);
    }
        <button onClick={handleClick}>add state</button>
    
  - Syntax useSate in class
    ```js
    class stateClass extends React.Component{
    constructor(){
        super();
        this.state = {count:23}
    }
    increment = ()=>{
        this.setState({count:this.state.count+1})
    }
    render(){
        return(
            <>
                <h2>State changing using class component</h2>
                <h3>Counter: {this.state.count}</h3>
                <button onClick={this.increment}>Increment</button>
            </>
        )
    }
    }
