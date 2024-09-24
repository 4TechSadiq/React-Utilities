### This section contains the the javascript utilities to for View,Update and Delete operaions in react

```js
    import React, { useEffect, useState } from 'react'
    import axios from 'axios'// We have to download
    import { ToastContainer, toast } from 'react-toastify';
    import 'react-toastify/dist/ReactToastify.css';
    
    
    export default function Crud() {
        const [data,setData] = useState([])
        const [update, setUpdate] = useState({})
        useEffect(()=>{
            axios //used to connect with api
            .get("http://127.0.0.1:8000/create/") // get data from api
            .then((response)=>{
                setData(response.data)
            })// after getting data
            .catch((error)=>{ // catching error
                console.log("error",error)
            })
        },[data])
    
        const updateDetails = (id) =>{
            console.log(id)
            fetch(`http://127.0.0.1:8000/details/${id}/`) // passing id with api
            .then(response=>response.json())
            .then(res=>setUpdate(res))
        }
    
        const handleInputChange = (event,fieldName) => {
            const value = event.target.value;
    
            setUpdate((prevUpdate)=>({
                ...prevUpdate,
                [fieldName] : value,
            }))
        }
    
        const handleSubmit = async(e,id)=>{
            e.preventDefault();
            const requestData ={
                id: update.id,
                emp_name: update.emp_name,
                email: update.email,
                phone:update.phone
            };
            const response = await axios.put(`http://127.0.0.1:8000/update/${id}/`, requestData,{
                headers:{
                    'Content-Type':'application/json'
                }
            });
            toast.success("Employ updated succesfully",{
                position: toast.POSITION.TOP_CENTER,
                theme:"colored",
            })
        }
    
        const handleDelete=((id)=>{
            fetch(`http://127.0.0.1:8000/delete/${id}/`,
                {method:"DELETE"}
            )
            .then(()=>{
                console.log("deleted")
            })
        })
    
      return(
        <>
            <div className='container-lg p-5'>
            <table class="table">
                <thead>
                    <tr>
                    <th scope="col">ID</th>
                    <th scope="col">NAME</th>
                    <th scope="col">EMAIL</th>
                    <th scope="col">PHONE</th>
                    <th scope="col">OPERATIONS</th>
                    </tr>
                </thead>
                <tbody>
                    {
                        data.map((item)=>(
                            <tr key={item.id}>
                            <th scope="row">{item.id}</th>
                            <td>{item.emp_name}</td>
                            <td>{item.email}</td>
                            <td>{item.phone}</td>
                            <td>
                                <button className='btn btn-warning' data-bs-toggle="modal" data-bs-target="#exampleModal" onClick={()=>{updateDetails(item.id)}}>update</button> &nbsp;
                                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#deletemodal" onClick={()=>{updateDetails(item.id)}}>
                                Delete
                                </button>
                            </td>
                            </tr>
                        ))
                    }
                   
                </tbody>
            </table>
            </div>
    
    
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Update Model</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                {/* {update.id} {update.emp_name} */}
                <div className='container'>
                    <form method='post' onSubmit={(e)=>handleSubmit(e, update.id)}>
                        <div className='form-group'>
                            <label>Name</label>
                            <input className='form-control' type='text' value={update.emp_name} onChange={(event)=>handleInputChange(event,"emp_name")}></input>
                        </div>
                        <div className='form-group'>
                            <label>Email</label>
                            <input className='form-control' type='email' value={update.email} onChange={(event)=>handleInputChange(event,"email")}></input>
                        </div>
                        <div className='form-group'>
                        <label>Phone</label>
                        <input className='form-control' type='number' value={update.phone} onChange={(event)=>handleInputChange(event,"phone")}></input>
                        </div>
                        <button type="submit" class="btn btn-primary">Update</button>
                    </form>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            </div>
            </div>
        </div>
        </div>
        <div class="modal fade" id="deleteModal" tabindex="-1" aria-labelledby="deleteModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Delete Model</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            ...
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="submit" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
    
    
    
    
    <div class="modal fade" id="deletemodal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Delete Modal</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <p>Do ypu want to Delete <b>{update.emp_name}</b> </p>
          </div>
          <div class="modal-footer">
            
            <button type="button" class="btn btn-danger" data-bs-dismiss="deletemodal" data onClick={()=>{handleDelete(update.id)}}>Delete</button>
          </div>
        </div>
      </div>
    </div>
    
    
        
        </>
      )
    }

```
### this is for create operation in react

```js
import axios from 'axios';
import React, { useState } from 'react'
import { toast } from 'react-toastify';

function Create() {

    const[formData, setFormData]=useState({})

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
            
            const response = await axios.post('http://127.0.0.1:8000/create/',formData,
               {
                 method: 'POST',
                 headers: {
                    'Content-Type': 'application/json',
                 }
                }
            )

            if (response.status == 201){
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
    <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    </div>

    
    </>
  )
}

export default Create
