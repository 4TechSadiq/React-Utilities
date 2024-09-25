### Search in react

```js
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { ToastContainer, toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';

export default function Crud() {
    const [data, setData] = useState([]);
    const [update, setUpdate] = useState({});

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await axios.get("http://127.0.0.1:8000/create/");
                setData(response.data);
            } catch (error) {
                console.log("error", error);
            }
        };
        fetchData();
    }, []); // Run only once on component mount

    const updateDetails = (id) => {
        fetch(`http://127.0.0.1:8000/details/${id}/`)
            .then(response => response.json())
            .then(res => setUpdate(res));
    };

    const handleInputChange = (event, fieldName) => {
        const value = event.target.value;
        setUpdate((prevUpdate) => ({
            ...prevUpdate,
            [fieldName]: value,
        }));
    };

    const handleSubmit = async (e, id) => {
        e.preventDefault();
        const requestData = {
            id: update.id,
            emp_name: update.emp_name,
            email: update.email,
            phone: update.phone
        };
        await axios.put(`http://127.0.0.1:8000/update/${id}/`, requestData, {
            headers: {
                'Content-Type': 'application/json'
            }
        });
        toast.success("Employee updated successfully", {
            position: toast.POSITION.TOP_CENTER,
            theme: "colored",
        });
    };

    const handleDelete = async (id) => {
        await fetch(`http://127.0.0.1:8000/delete/${id}/`, {
            method: "DELETE"
        });
        setData(data.filter(item => item.id !== id)); // Update the local state
        toast.success("Employee deleted successfully", {
            position: toast.POSITION.TOP_CENTER,
            theme: "colored",
        });
    };

    // Pagination logic
    const [currentPage, setCurrentPage] = useState(1);

    const [searchItem,setSearchItem] = useState('')

    const filterData = data.filter((item)=>
        item.emp_name.toLowerCase().includes(searchItem.toLocaleLowerCase())
    )



    const recordPerPage = 3;
    const lastIndex = currentPage * recordPerPage;
    const firstIndex = lastIndex - recordPerPage;
    const records = filterData.slice(firstIndex, lastIndex);
    const npage = Math.ceil(data.length / recordPerPage);
    const numbers = [...Array(npage + 1).keys()].slice(1);

    function prevPage() {
        if (currentPage !== firstIndex) {
            setCurrentPage(currentPage - 1);
        }
    }

    function nextPage() {
        if (currentPage !== lastIndex) {
            setCurrentPage(currentPage + 1);
        }
    }

    function changePage(id) {
        setCurrentPage(id);
    }

    return (
        <>
            <div className='container-lg p-5 shadow rounded mb-5'>
                <div className='row'>
                    <label className='form-label'>Search</label>
                    <input className='form-control' placeholder='search here' style={{width: '50%'}} type='text' value={searchItem} onChange={(e)=>{
                        setSearchItem(e.target.value)
                        setCurrentPage(1)
                    }}></input>
                </div>
                <table className="table">
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
                        {records.map((item) => (
                            <tr key={item.id}>
                                <th scope="row">{item.id}</th>
                                <td><img src={item.image} style={{width: '50px',height: '50px', borderRadius:'100px'}}></img>{item.emp_name}</td>
                                <td>{item.email}</td>
                                <td>{item.phone}</td>
                                <td>
                                    <button className='btn btn-warning' data-bs-toggle="modal" data-bs-target="#exampleModal" onClick={() => { updateDetails(item.id) }}>Update</button>
                                    &nbsp;
                                    <button type="button" className="btn btn-primary" data-bs-toggle="modal" data-bs-target="#deletemodal" onClick={() => { updateDetails(item.id) }}>
                                        Delete
                                    </button>
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </table>
                <nav aria-label="Page navigation example">
                    <ul className="pagination">
                        <li className="page-item"><a className="page-link" onClick={prevPage}>Previous</a></li>
                        {numbers.map((n, i) => (
                            <li className={`page-item ${currentPage === n ? 'active' : ''}`} key={i}>
                                <a className="page-link" onClick={() => changePage(n)}>{n}</a>
                            </li>
                        ))}
                        <li className="page-item"><a className="page-link" onClick={nextPage}>Next</a></li>
                    </ul>
                </nav>
            </div>

            {/* Update Modal */}
            <div className="modal fade" id="exampleModal" tabIndex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div className="modal-dialog">
                    <div className="modal-content">
                        <div className="modal-header">
                            <h1 className="modal-title fs-5" id="exampleModalLabel">Update Modal</h1>
                            <button type="button" className="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div className="modal-body">
                            <div className='container'>
                                <form method='post' onSubmit={(e) => handleSubmit(e, update.id)}>
                                    <div className='form-group'>
                                        <label>Name</label>
                                        <input className='form-control' type='text' value={update.emp_name} onChange={(event) => handleInputChange(event, "emp_name")}></input>
                                    </div>
                                    <div className='form-group'>
                                        <label>Email</label>
                                        <input className='form-control' type='email' value={update.email} onChange={(event) => handleInputChange(event, "email")}></input>
                                    </div>
                                    <div className='form-group'>
                                        <label>Phone</label>
                                        <input className='form-control' type='number' value={update.phone} onChange={(event) => handleInputChange(event, "phone")}></input>
                                    </div>
                                    <button type="submit" className="btn btn-primary">Update</button>
                                </form>
                            </div>
                        </div>
                        <div className="modal-footer">
                            <button type="button" className="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                        </div>
                    </div>
                </div>
            </div>

            {/* Delete Modal */}
            <div className="modal fade" id="deletemodal" tabIndex="-1" aria-labelledby="deleteModalLabel" aria-hidden="true">
                <div className="modal-dialog">
                    <div className="modal-content">
                        <div className="modal-header">
                            <h1 className="modal-title fs-5" id="deleteModalLabel">Delete Modal</h1>
                            <button type="button" className="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div className="modal-body">
                            <p>Are you sure you want to delete this employee?</p>
                        </div>
                        <div className="modal-footer">
                            <button type="button" className="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" className="btn btn-danger" onClick={() => handleDelete(update.id)}>Delete</button>
                        </div>
                    </div>
                </div>
            </div>
        </>
    );
}
```
