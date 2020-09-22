<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let editingEmployee = {};
  $: employee = editingEmployee;
  const apiBaseUrl = "http://dummy.restapiexample.com/api/v1";
  let loading = false;

  const onSubmit = async (e) => {
    e.preventDefault();
    if (
      employee.employee_name &&
      employee.employee_age &&
      employee.employee_salary &&
      !employee.id
    ) {
      //create call
      let empobj = {
        name: employee.employee_name,
        age: employee.employee_age,
        salary: employee.employee_salary,
      };
      loading = true;
      const res = await fetch(`${apiBaseUrl}/create`, {
        method: "POST",
        body: empobj,
      });
      res
        .json()
        .then((data) => {
          loading = false;
          employee = {};
          dispatch("postCreate", data.message);
        })
        .catch((err) => {
          alert(err);
          loading = false;
          employee = {};
        });
    } else if (employee.id) {
      //update call
      loading = true;
      fetch(`${apiBaseUrl}/update/${employee.id}`, {
        method: "PUT",
        body: employee,
      })
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          //success
          loading = false;
          employee = {};
          dispatch("postUpdate", data.message);
        })
        .catch((err) => {
          alert(err);
          loading = false;
          employee = {};
        });
    } else {
      return;
    }
  };
</script>

<style>
  form {
    margin: 30px 10px;
  }
</style>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label
        for="employee_name"
        class={employee.id ? 'active' : ''}>Name</label>
      <input type="text" bind:value={employee.employee_name} />
    </div>
    <div class="input-field">
      <label for="employee_age" class={employee.id ? 'active' : ''}>Age</label>
      <input type="number" bind:value={employee.employee_age} />
    </div>
    <div class="input-field">
      <label
        for="employee_salary"
        class={employee.id ? 'active' : ''}>Salary</label>
      <input type="number" bind:value={employee.employee_salary} />
    </div>
    <button
      type="submit"
      class="waves-effect waves-light btn">{employee.id ? 'Update' : 'Add'}</button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
