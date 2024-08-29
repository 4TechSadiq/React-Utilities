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
  - 
