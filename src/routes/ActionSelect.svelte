<script>
  // All possible actions and their `id`s 
  let editOptions = [
    {id: 1, text: "Select a Action"},
    {id: 2, text: "Add Images"},
    {id: 3, text: "Remove Images"},
  ];

  // For dynamically changing the submit buttons visibility and text content  
  let actionButtonTextOptions = [
    null,
    "Add",
    "Remove",
  ];


  export let selected;
  let actionButtonText = null; 

  // If an action is selected, change the text of the submit button reactively
  $: if (selected) {
    actionButtonText = actionButtonTextOptions[selected.id - 1];
  }


</script>

<form on:submit|preventDefault>
  <!-- 
    [Challenge]:
    Make the dropdown input bind to the `selected` prop
  -->
  <select>
    {#each editOptions as editOption}
      <option value="{editOption}">{editOption.text}</option>
    {/each}
  </select>

  {#if actionButtonText}
    <input type="submit" id="submit_button" value="{actionButtonText}">
  {/if}
</form>

<style>
  form {
    margin-bottom: 30px;
  }

  #submit_button {
    margin-top: 20px;
    min-width: 70px;    
    font-size: 20px;
    height: 50px;
    display: block;
  }
</style>
