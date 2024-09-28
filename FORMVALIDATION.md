## Form validation

This module includes the data for formvalidation

```js

import React, { useState } from 'react'
import { toast } from 'react-toastify';
import axios from 'axios';
import { useNavigate } from 'react-router-dom';

function Register() {

  const [formData, setFormData] = useState({})
  const navigate = useNavigate()// used to redirec
  const handleInput = (e)=>{
    const{name,value} = e.target;
    setFormData({
        ...formData,
        [name]: value,
    })
  }

  const checkValidation = () =>{
    const requiredField = ["admin_name","mail","username", "password", "con_password"]
    for(const field of requiredField){
      if(!formData[field]){
        toast.warning(`${field.replace("_",)} is required`,{
          position: toast.POSITION.TOP_CENTER,
          theme: 'colored',
        });
        return false
      }
    }
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if(!emailRegex.test(formData.mail.toLowerCase())){
      toast.warning("Please enter valid mail ID",{
        position: toast.POSITION.TOP_CENTER,
        theme: 'colored',
      })
    }
    if (formData.password !== formData.con_password){
      toast.warning("Password not matching",{
        position: toast.POSITION.TOP_CENTER,
        theme: 'colored',
      })
    }
    return true
  }

  const handleSubmit = async(e)=>{
    e.preventDefault()
    console.log("Entered value", formData)
    if(!checkValidation()){
      return;
    }
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
        navigate("/")



    }catch(error){
        // console.log(error.response.data)
        toast.error("admin not inserted succesfully",
          {
            position:toast.POSITION.BOTTOM_CENTER,
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
