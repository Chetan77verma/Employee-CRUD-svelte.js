<script>
  import { onMount } from "svelte";
  import EmployeeForm from "../components/employeeform.svelte";
  const apiBaseUrl = "http://dummy.restapiexample.com/api/v1";
  let employees = [];
  let editingEmployee = {
    employee_name: "",
    employee_age: "",
    employee_salary: "",
    id: null,
  };
  onMount(async () => {
    const res = await fetch(apiBaseUrl + "/employees");
    let emp = await res.json();
    employees = emp.data;
  });

  const editEmployee = (employee) => {
    window.scrollTo(0, 0);
    console.log("edit emoployee", employee);
    editingEmployee = employee;
  };

  const deleteEmployee = (id) => {
    if (confirm("Are you sure you want to delete ?")) {
      fetch(`${apiBaseUrl}/delete/${id}`, {
        method: "DELETE",
      })
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          //success
          alert(data.message);
        })
        .catch((err) => {
          alert(err);
        });
    }
  };
  const refreshList = (e) => {
    //data recieved from child component
    //update the rendered list here
    editingEmployee = {};
    alert(e.detail);
  };
</script>

<style>
  .delete-btn {
    background-color: red !important;
  }
  .mgtp-10 {
    margin-top: 10px;
  }
</style>

<div class="row">
  <EmployeeForm
    on:postCreate={refreshList}
    on:postUpdate={refreshList}
    {editingEmployee} />
</div>
<div class="row">
  {#if employees.length === 0}
    <h1>Loading...</h1>
  {:else}
    {#each employees as employee}
      <div class="col s6">
        <div class="card">
          <div class="card-content">
            <p class="card-title">Name: {employee.employee_name}</p>
            <p>Salary: {employee.employee_salary}</p>
            <p>Age: {employee.employee_age}</p>
            <div class="card-actions mgtp-10">
              <button
                class="btn"
                on:click={() => editEmployee(employee)}>Edit</button>
              <button
                on:click={() => deleteEmployee(employee.id)}
                class="btn delete-btn">Delete</button>
            </div>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
