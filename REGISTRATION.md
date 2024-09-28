# Registration
### admin registraion
### login using admin credentials

This module talks about creating an admin like interface in react application. The CRUD operations are much like in the normal apps. By using react making it in such a way that if the person logged is admin then he can do cruds


1) make react project
2) make django-rest project
3) run both projects parallally
4) Create models for users and admins


### Script for Admin redistration
```js
  import React, { useState } from 'react'
import { toast } from 'react-toastify';
import axios from 'axios';

function Register() {

  const [formData, setFormData] = useState({})
  const handleInput = (e)=>{
    const{name,value} = e.target;
    setFormData({
        ...formData,
        [name]: value,
    })
  }

  const handleSubmit = async(e)=>{
    e.preventDefault()
    console.log("Entered value", formData)
    try{
        
        const response = await axios.post('http://127.0.0.1:8000/create-admin/',formData,
           {
             method: 'POST',
             headers: {
                'Content-Type': 'application/json',
             }
            }
        )

        if (response.status === 201){
            toast.success("admin added successfully added",
                {position: toast.POSITION.TOP_CENTER,
                theme: 'colored'}
            )
        }

    }catch(error){
        console.log(error.response.data)
        toast.success("admin not inserted succesfully",
          {
            position:toast.POSITION.BOTTOM_RIGHT,
            theme: "colored"
          }
        )
    }
  }

  return (
    <>
    <h2 className='text-center'>Admin Registration</h2>

    <div class="container shadow" style={{width:'60%', marginBottom:50, padding:20}}>
    <form onSubmit={handleSubmit}>
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Admin name</label>
        <input type="text" name='admin_name' class="form-control" id="name" aria-describedby="emailHelp" onChange={handleInput}></input>
        <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label" >Admin Email ID</label>
        <input type="email" name='mail' class="form-control" onChange={handleInput} id="email" aria-describedby="emailHelp"></input>
        <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Username</label>
        <input type="text" name='username' class="form-control"  onChange={handleInput} id="exampleInputPassword1"></input>
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Password</label>
        <input type="text" name='password' class="form-control"  onChange={handleInput} id="exampleInputPassword1"></input>
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Confirm Password</label>
        <input type="text" name='con_password' class="form-control"  onChange={handleInput} id="exampleInputPassword1"></input>
    </div>
    <button type="reset" class="btn btn-danger">reset</button>
    <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    </div>
    </>
  )
}

export default Register
```

## Routing through pages. If admin exists then login to admin dashboard
### Login page


```js
import React, { useState } from 'react'
import { Link, useNavigate } from 'react-router-dom';
import { toast } from 'react-toastify';

function Login() {

    const [formData,setFormData] = useState({})
    const navigate = useNavigate()
    console.log(formData)
    const handleInput = (e)=>{
        const{name,value} = e.target;
        setFormData({
            ...formData,
            [name]: value,
        })
    }

    const handleSubmit = async(e)=>{
        e.preventDefault();
        
        try{
            const response = await fetch("http://127.0.0.1:8000/user_login/",{
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(formData)
            })
            console.log(response.ok)
            if(response.ok){
                toast.success("Login Succesfull: "+formData.username,{
                    position: toast.POSITION.TOP_CENTER,
                    theme: "colored"
                })
                navigate("/dash")

            }else{
                toast.error("Invalid credentials: "+formData.username,{
                    position: toast.POSITION.TOP_CENTER,
                    theme: "colored"
                })
            }

        }catch(error){
    
        }
    }

    




  return (
    <>
    <h2 className='text-center'>Login Here</h2>
    <div className='container shadow rounded p-4'>
    <form onSubmit={handleSubmit}>
    <div className="mb-3">
        <label for="exampleInputPassword1" className="form-label">Username</label>
        <input type="text" name='username' className="form-control"  onChange={handleInput}></input>
    </div>
    <div className="mb-3">
        <label for="exampleInputPassword1" className="form-label">Password</label>
        <input type="password" name='password' className="form-control"  onChange={handleInput}></input>
    </div>
    <button type="reset" className="btn btn-danger">reset</button>
    <button type="submit" className="btn btn-primary">Submit</button>
    <p className='text-center'>Dont Have an account<Link to="/register">Register here</Link></p>
    </form>
    </div>
    </>
  )
}

export default Login
```

### App.js
```js
import React from "react";
import Crud from "./components/Crud";
import Create from "./components/Create";
import Home from "./Home";
import Create_Image from "./components/Create_Image";
import Register from "./components/Credentials/Register";
import Login from "./components/Credentials/Login";
import { BrowserRouter,Routes, Route} from "react-router-dom"

function App() {
  return (
      <>
      {/* <h3 className='text-center'>CRUD OPERATIONS</h3> */}
      {/* <Crud/>
       <Create/>
      <Create_Image/>
      <Register/>
      <Login/> */}

      <BrowserRouter>
        <Routes>
          <Route exact path="/" element={<Login/>}></Route>
          <Route path="register/" element={<Register/>}></Route>
          <Route path="dash/" element={<Crud/>}></Route>
          <Route path="create/" element={<Create/>}></Route>
        </Routes>
      </BrowserRouter>
      </>

  );
}

export default App;
```
