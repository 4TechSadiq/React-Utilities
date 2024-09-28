### this for testing with fake api

```js
import React, { useEffect, useState } from 'react'
import axios from 'axios';

function Fake_api() {
    const[data,setData] = useState([]);
    const[formData,setFormData] =useState([]);

    useEffect(()=>{
        axios.get('https://jsonplaceholder.typicode.com/users/')
        .then((response) => setData(response.data))
        .then((json) => console.log(json));
    },[])

    const handleInput = (e)=>{
        const {name,value} = e.target;
        setFormData({
            ...formData,
            [name]:value,
        })
    }
    const handleSubmit = async(e) =>{
        e.preventDefault()
        const insertData = {
            name: formData.name,
            email: formData.email,
            phone: formData.phone,
        }
        fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(insertData),
            headers: {
              'Content-type': 'application/json; charset=UTF-8',
            },
          })
            .then((response) => {
                if(response.ok){
                    return response.json()
                }
            } )
            .then((newData) => (
                setData([...data,newData])
            ));
    }


  return (
    <>
    {/* {data.map((item)=>(
        <p>{item.name}</p>
    ))} */}

    <table class="table">
    <thead>
        <tr>
        <th scope="col">#</th>
        <th scope="col">Namw</th>
        <th scope="col">Mail</th>
        <th scope="col">Phone NUmber</th>
        </tr>
    </thead>
    <tbody>
    {data.map((item)=>(
        <tr>
            <th scope="row">#</th>
            <td>{item.name}</td>
            <td>{item.email}</td>
            <td>{item.phone}</td>
        </tr>
    ))}
    </tbody>
    </table>
    <h2 className='text-center'>Add Data</h2>
    <div className='container shadow d-flex justify-content-center'>
        <form onSubmit={handleSubmit}>
            <div className='mb-3'>
                <label className='form-label'>Name</label>
                <input name='name' className='form-control' onChange={handleInput} type="text"></input>
            </div>
            <div className='mb-3'>
                <label className='form-label'>Email</label>
                <input name='email' onChange={handleInput} className='form-control' type="text"></input>
            </div>
            <div className='mb-3'>
                <label className='form-label'>Phone</label>
                <input name='phone' onChange={handleInput} className='form-control' type="text"></input>
            </div>
            <div className='mb-3'>
                <button className='btn btn-primary ' type='submit'>Submit</button>
            </div>
        </form>
    </div>
    
    </>
  )
}

export default Fake_api
```
