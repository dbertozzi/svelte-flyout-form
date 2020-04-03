<script>
  import { writable } from "svelte/store";
  import { fade } from "svelte/transition";
  import { createEventDispatcher } from "svelte";

  export let satisfaction;
  export let detailsInput;
  export let data;

  const dispatch = createEventDispatcher();

  let key, keyCode;
  const handleKeydown = event => {
    key = event.key;
    keyCode = event.keyCode;
    const enterKeyCode = 13;
    if (keyCode === enterKeyCode)
      document.getElementById(`${data.form.id}-submit`).click();
  };
</script>

<style>
  form {
    grid-row: 2;
    grid-column: 1 / span 2;
    display: grid;
    grid-gap: 0.5em;
  }
  form label:first-of-type {
    grid-row: 1;
  }
  ul.range-label {
    grid-row: 2;
    align-self: start;
    grid-column: 1;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    list-style-type: none;
    padding-left: 0px;
    font-size: 8px;
    margin-bottom: 0px;
    margin-top: 0px;
  }
  ul.range-label li:first-of-type {
    justify-self: start;
    grid-row: 1;
  }
  ul.range-label li:last-of-type {
    justify-self: end;
    grid-row: 1;
  }
  ul.range-label li:nth-of-type(2) {
    justify-self: center;
    grid-row: 1;
  }
  input[type="range"] {
    grid-row: 3;
    grid-column: 1;
    align-self: center;
    height: 20px;
    margin: 0px;
  }
  form label:nth-of-type(2) {
    grid-row: 4;
  }
  form textarea {
    grid-row: 5;
    grid-column: 1;
    margin: 0px;
  }
  form p {
    grid-row: 5;
    grid-column: 1;
    align-self: end;
    justify-self: end;
    margin-right: 20px;
    margin-bottom: 5px;
    font-size: 8px;
  }
  form button {
    grid-row: 6;
    border-radius: 4px;
  }
</style>

<svelte:window on:keydown={handleKeydown} />

<form in:fade={{ duration: 500, delay: 500 }}>
  <label for={data.inputs.range.id}>
    {data.inputs.range.label} {satisfaction}
  </label>
  <ul class="range-label">
    {#each data.inputs.range.scaleLabel as label}
      <li>{label}</li>
    {/each}
  </ul>
  <input
    id={data.inputs.range.id}
    name={data.inputs.range.id}
    required
    type="range"
    min="0"
    max="10"
    bind:value={satisfaction} />
  <label for={data.inputs.textarea.id}>{data.inputs.textarea.label}</label>
  <textarea
    id={data.inputs.textarea.id}
    name={data.inputs.textarea.id}
    maxlength={data.maxLengthDetailsInput}
    rows={data.maxLengthDetailsInput / 25}
    placeholder={data.inputs.textarea.placeholder}
    bind:value={detailsInput} />
  <p>{data.maxLengthDetailsInput - detailsInput.length} characters remaining</p>
  <button type="button" id={`${data.form.id}-submit`} on:click={() => dispatch('submit')}>
    Submit
  </button>
</form>
