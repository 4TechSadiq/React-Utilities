# React 
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
## UseState() Hook
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

#### Multiple useState
- Multiple useState in function
- ```js
  export default function TwoState(){
    const [color,setColor]=useState("grey")
    const [color1, setColor1]=useState("green")

    return(
        <>
        <h2>Two Usestate</h2>
        <div style={{ width: "30%", height: 200, border:"1px solid black", background: color}}>
            <p style={{marginTop:"15%", marginLeft:"40%"}}>First Div</p>
        </div>
        <button onClick={()=>{setColor("red")}}>color change</button>


        <div style={{ width: "30%", height: 200, border:"1px solid black", background:color1}}>
            <p style={{marginTop:"15%", marginLeft:"40%"}}>second Div</p>
        </div>
        <button onClick={()=>{setColor1("blue")}}>color change</button>
        <button onClick={()=>{setColor("grey"); setColor1("grey")}}>reset</button>
        </>
    )
  }

- Multiple useState in class
- ```js
  class TwoState extends React.Component{
    constructor(){
        super();
        this.state = ({
            color: "grey",
            color1: "blue"
        })
    }
    changeColor = () => {
        this.setState({
            color:"red",
            color1:"yellow"
        }
        ) 
    }
    render(){
        return(
            <>
         <h2>Two Usestate</h2>
        <div style={{ width: "30%", height: 200, border:"1px solid black", background: this.state.color}}>
            <p style={{marginTop:"15%", marginLeft:"40%"}}>First Div</p>
        </div>
        <button onClick={this.changeColor}>color change</button>
        <div style={{ width: "30%", height: 200, border:"1px solid black", background:this.state.color1}}>
            <p style={{marginTop:"15%", marginLeft:"40%"}}>second Div</p>
        </div>
        <button onClick={this.changeColor}>color change</button>
        <button onClick>reset</button>
        </>
        )
    }
  }
  
  export default TwoState

#### use state with object
- it is difficult to write more div. Thats where the state comes in.
- Eg for changing color of multiple div
- syntax
- ```js
  import React, { useState } from "react";

  export default function StateObject(){
      const [color, setColor] = useState({
          div_1_color: "grey",
          div_2_color: "yellow"
      })

    function handlecolor(){
        setColor({...color, div_1_color:"red",div_2_color:"blue"})
    }
    return(
        <>
            <h2>Two Usestate</h2>
            <div style={{ width: "30%", height: 200, border:"1px solid black", background: color.div_1_color}}>
                <p style={{marginTop:"15%", marginLeft:"40%"}}>First Div</p>
            </div>
            <button onClick={handlecolor}>color change</button>


            <div style={{ width: "30%", height: 200, border:"1px solid black", background: color.div_2_color}}>
                <p style={{marginTop:"15%", marginLeft:"40%"}}>second Div</p>
            </div>
            <button onClick={handlecolor}>color change</button>
            <button>reset</button>
        </>
    )
    }
#### update object state
- ```js
  import React, { useState } from "react";

  
  export default function Object_Update(){
      const [value, setValue] = useState({
          name:"nikil",
          age: "23",
          dept: "science"
      })
      console.log(value);

    function handleUpdate(){
        setValue({
            ...value, //this is to divide items from array one by one. Changes will not apply to others.
            name:"shibi",
            
        })
    }
    return(
        <>
            <h2>Student Details</h2>
                <p>name: {value.name}</p>
                <p>age: {value.age}</p>
                <p>dept: {value.dept}</p>
                <button onClick={handleUpdate}>update</button> 
        </>
    )
    }
#### updating array elemets using useState
- ```js
  import React, { useState } from "react";
  
  export default function ArrayList(){
      const [lists, setLists] = useState(["apple", "orange","banana"])

    function updateButton(){
        const arr = ["grape", "lemon", "nuts"]
        setLists(arr)
    }
    return(
        <>
        <h2>Array list</h2>
        <h3>List of fruits</h3>
        <ul>
            {lists.map(
                (e,index)=>(
                    <li key={index}>{e}</li>
                )
            )}
        </ul>
        <button onClick={updateButton}>Update elemets</button>
        </>
    )
  }


## UseEffect Hook
- it is used to handle sideeffect
- eg during updation, manipulation, during umounting ...etc
- runs on every render.
- it works during render time.
- eg
- ```js
  export default function UseEffect_Fun(){
    const [value, setValue] = useState(0)

    useEffect(()=>{
        console.log("value changed or incremented")
    })

    return(
        <>
            <h2>use effect in function component</h2>
            <p>Count: {value}</p>
            <button onClick={()=>{setValue(value+1)}}>Increment</button>
        </>
    )
  }
#### different useEffect
- 1) ```js
     export default function UseEffect_Fun(){
      const [value, setValue] = useState(0)
  
      useEffect(()=>{
          limit_count()
          console.log("value changed or incremented")
      })
  
      function limit_count(){
          if(value>10){
              setValue(1)
          }
      }
  
      return(
          <>
              <h2>use effect in function component</h2>
              <p>Count: {value}</p>
              <button onClick={()=>{setValue(value+1)}}>Increment</button>
          </>
      )
     }
- 2) increment values on time interval
     ```js
     export default function Timer(){
      const [value,setValue] = useState(0)
  
      useEffect(()=>{
          setTimeout(()=>{
              setValue(value+1)
          },1000);
      })
      return(
          <>
              <h2>Timer using useEffect</h2>
              <h3>Count: {value}</h3>
          </>
      )
     }


## Forms in react
- To handle form submit, onSubmit on button is used.
- eg
- ```js
    import React, { useState } from 'react'
  
  export default function Register() {
  
      const [name,setValue] = useState("")
      console.log("Name",name)
  
      const handleSubmit = (e) => {
          e.preventDefault()
          console.log("submited")
      }
    return (
      <>
      <div>Register Form</div>
      <form onSubmit={handleSubmit}>
          <label>First name</label>
          <input type='text' onChange={(e)=>{setValue(e.target.value)}}></input> {/*  // to detect change and act accordingly */}
          <button type='submit'>submit</button>
      </form>
      <p>Name: {name}</p>
      </>
    )
  }

#### Multiple input value submission
- to handle multiple value, just create multiple useStates
- eg
- ```js
  import React, { useState } from 'react'

export default function Register() {

    const [name,setValue] = useState("")
    console.log("Name",name)
    const [age, setAge] = useState("")
    const [mail, setMail] = useState("")

    const handleSubmit = (e) => {
        e.preventDefault()
        console.log("submited")
    }
    return (
      <>
      <div>Register Form</div>
      <form onSubmit={handleSubmit}>
          <label>First name</label>
          <input type='text' onChange={(e)=>{setValue(e.target.value)}}></input><br></br> {/*  // to detect change and act accordingly */}
          <label>Age</label>
          <input type='number' onChange={(e)=>{setAge(e.target.value)}}></input><br></br>
          <label>mail</label>
          <input type='email' onChange={(e)=>{setMail(e.target.value)}}></input><br></br>
          <button type='submit'>submit</button>
      </form>
      <p>Name: {name}</p>
      <p>age: {age}</p>
      <p>mail:{mail}</p>
      </>
    )
    }

#### handling multiple form input using single usestate
- ```js
  import React, { useState } from 'react'

  export default function Register() {

    const [formData,setFormData] = useState({})

    const handleSubmit = (e) => {
        e.preventDefault()
        console.log("submited: ", formData)
    }
      return (
        <>
        <div>Register Form</div>
        <form onSubmit={handleSubmit}>
            <label>First name</label>
            <input type='text' onChange={(e)=>{setFormData((prev)=>{return{...prev,name:e.target.value}})}}></input><br></br> {/*  // to detect change and act accordingly */}
            <label>Age</label>
            <input type='number' onChange={(e)=>{setFormData((prev)=>{return{...prev,age:e.target.value}})}}></input><br></br>
            <label>mail</label>
            <input type='email' onChange={(e)=>{setFormData((prev)=>{return{...prev,email:e.target.value}})}}></input><br></br>
            <button type='submit'>submit</button>
        </form>
        {/* <p>Name: {name}</p>
        <p>age: {age}</p>
        <p>mail:{mail}</p> */}
        </>
      )
    }

#### Simplifying onChange Handler Code
- It is also use to handle multiple form inputs
- With every onChange we call a function
- The function will detect onChange and will act accordingly
- ```js
  import React, { useState } from 'react'
    
    export default function Register() {

    const [formData,setFormData] = useState({})

    const handleInput = (e) =>{
        const {name,value} = e.target;
        setFormData({
            ...formData,
            [name] : value, 
        })
    }

    const handleSubmit = (e) => {
        e.preventDefault()
        console.log("submited: ", formData)
    }

      return (
        <>
        <div>Register Form</div>
        <form onSubmit={handleSubmit}>
            <label>First name</label>
            <input type='text' name='name' value={formData.name} onChange={handleInput}></input><br></br> {/*  // to detect change and act accordingly */}
            <label>Age</label>
            <input type='number' name='age' value={formData.age} onChange={handleInput}></input><br></br>
            <label>mail</label>
            <input type='email' name='email' value={formData.email} onChange={handleInput}></input><br></br>
            <button type='submit'>submit</button>
        </form>
        {/* <p>Name: {name}</p>
        <p>age: {age}</p>
        <p>mail:{mail}</p> */}
        </>
      )
    }

#### Select Dropdown in react
-  In select section we give
-  ```js
   <select name='language' value={formData.language} onChange={handleInput}>
- ```js
  import React, { useState } from 'react'

  export default function Register() {
    
    const [formData,setFormData] = useState({})

    const handleInput = (e) =>{
        const {name,value} = e.target;
        setFormData({
            ...formData,
            [name] : value, 
        })
    }

    const handleSubmit = (e) => {
        e.preventDefault()
        console.log("submited: ", formData)
    }

      return (
        <>
        <div>Register Form</div>
        <form onSubmit={handleSubmit}>
            <label>First name</label>
            <input type='text' name='name' value={formData.name} onChange={handleInput}></input><br></br> {/*  // to detect change and act accordingly */}
            <label>Age</label>
            <input type='number' name='age' value={formData.age} onChange={handleInput}></input><br></br>
            <label>mail</label>
            <input type='email' name='email' value={formData.email} onChange={handleInput}></input><br></br>
            <label>Select Language</label>
            <select name='language' value={formData.language} onChange={handleInput}>
                <option value={"malayalam"}>malayalam</option>
                <option value={"english"}>english</option>
                <option value={"hindi"}>Hindi</option>
            </select>
            <br></br>
            <button type='submit'>submit</button>
        </form>
        {/* <p>Name: {name}</p>
        <p>age: {age}</p>
        <p>mail:{mail}</p> */}
        </>
      )
    }

#### select initial value form value
- we pass inital values with usestate
- ```js
  const [formData,setFormData] = useState({
        number:"+91",
        email: "@gmail.com"
    })
