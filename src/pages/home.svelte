<script>
  import { onMount } from "svelte";
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
</script>

<style>
  .delete-btn {
    color: red !important;
  }
</style>

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
            <div class="card-actions">
              <button on:click={() => editEmployee(id)}>Edit</button>
              <button
                on:click={() => deleteEmployee(id)}
                class="delete-btn">Delete</button>
            </div>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
