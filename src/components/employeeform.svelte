<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  let employee = {};
  const apiBaseUrl = "http://dummy.restapiexample.com/api/v1";
  let loading = false;
  const onSubmit = async (e) => {
    e.preventDefault();
    if (employee.name && employee.age && employee.salary) {
      loading = true;
      const res = await fetch(`${apiBaseUrl}/create`, {
        method: "POST",
        body: employee,
      });
      res
        .json()
        .then((data) => {
          loading = false;
          dispatch("postCreate", data.data);
          employee = {};
          console.log(data);
          if (data.status == "success") {
            console.log(data.message);
          }
        })
        .catch((err) => {
          console.log(err);
          loading = false;
          employee = {};
        });
    } else {
      return;
    }
  };
</script>

<style>
</style>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label for="name">Name</label>
      <input type="text" bind:value={employee.name} />
    </div>
    <div class="input-field">
      <label for="age">Age</label>
      <input type="text" bind:value={employee.age} />
    </div>
    <div class="input-field">
      <label for="salary">Salary</label>
      <input type="text" bind:value={employee.salary} />
    </div>
    <button type="submit" class="waves-effect waves-light btn">Add </button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
