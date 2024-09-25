### ADD IMAGE INTO PAGE


```js
import axios from 'axios';
import React, { useState } from 'react'
import { toast } from 'react-toastify';

function Create_Image() {

    const[formData, setFormData]=useState({})

    const handleInput = (e)=>{
        const{name,value} = e.target;
        setFormData({
            ...formData,
            [name]: value,
        })
    }

    const handleInputImage = (e) =>{
        const file = e.target.files[0]
        const formDataImage = new FormData()

        formDataImage.append("emp_name",formData.emp_name)
        formDataImage.append("email",formData.email)
        formDataImage.append("phone",formData.phone)
        formDataImage.append("image", file)

        setFormData(formDataImage)
    }

    const handleSubmit = async(e)=>{
        e.preventDefault()
        console.log("Entered value", formData)
        try{
            
            const response = await axios.post('http://127.0.0.1:8000/create/',formData,
               {
                 method: 'POST',
                 headers: {
                    'Content-Type': 'multipart/form-data',// for including image field
                 }
                }
            )

            if (response.status === 201){
                toast.success("data added",
                    {position: toast.POSITION.TOP_CENTER,
                    theme: 'colored'}
                )
            }

        }catch(error){
            console.log(error.response.data)
        }
    }

    

  return (
    <>
    <div class="container shadow" style={{width:'60%', marginBottom:50, padding:20}}>
    <form onSubmit={handleSubmit}>
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Employee name</label>
        <input type="text" name='emp_name' class="form-control" id="name" aria-describedby="emailHelp" onChange={handleInput}></input>
        <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label" >Email ID</label>
        <input type="email" name='email' class="form-control" onChange={handleInput} id="email" aria-describedby="emailHelp"></input>
        <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Phone</label>
        <input type="number" name='phone' class="form-control"  onChange={handleInput} id="exampleInputPassword1"></input>
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Employee Pic</label>
        <input type="file"  name='image' accept='image/*' class="form-control"  onChange={handleInputImage} id="exampleInputPassword1"></input>
    </div>
    <button type="reset" class="btn btn-danger">reset</button>
    <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    </div>

    
    </>
  )
}

export default Create_Image
```
