<script>
  import { onMount } from "svelte";
  import EmployeeForm from "../components/employeeform.svelte";
  const apiBaseUrl = "http://dummy.restapiexample.com/api/v1";
  let employees = [];
  onMount(async () => {
    const res = await fetch(apiBaseUrl + "/employees");
    let emp = await res.json();
    employees = emp.data;
  });

  const editEmployee = (id) => {
    console.log("edit emoployee", id);
  };

  const deleteEmployee = (id) => {
    console.log("delete emoployee", id);
  };
  const refreshList = (e) => {
    //update the rendered list here
    console.log(e.detail);
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
  <EmployeeForm on:postCreate={refreshList} />
</div>
<div class="row">
  {#if employees.length === 0}
    <h1>Loading...</h1>
  {:else}
    {#each employees as { id, employee_salary, employee_age, employee_name }}
      <div class="col s6">
        <div class="card">
          <div class="card-content">
            <p class="card-title">Name: {employee_name}</p>
            <p>Salary: {employee_salary}</p>
            <p>Age: {employee_age}</p>
            <div class="card-actions mgtp-10">
              <button
                class="btn"
                on:click={() => editEmployee(id)}>Edit</button>
              <button
                on:click={() => deleteEmployee(id)}
                class="btn delete-btn">Delete</button>
            </div>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
