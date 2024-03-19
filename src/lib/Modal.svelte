<script>
  export let number;

  let guessNumber;

  $: submitDisabled = number < 1;

  const count = () => {
    number--;
  };
</script>

<div class="backdrop" />
<div class="modal">
  <header>Guess and win!</header>
  <hr />
  <p>Submit {number} more numbers</p>

  <div class="guess">
    <label>
      Guess a number <input type="number" bind:value={guessNumber} />
    </label>
    {#if number >= 1}
      <button on:click={count} disabled={submitDisabled}>Submit</button>
    {:else}
      <button on:click={count} disabled={submitDisabled}>Confirm</button>
    {/if}
  </div>

  <hr />
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    top: 200px;
    left: 120px;
    width: 80%;
    max-height: 80vh;
    background: white;
    border-radius: 5px;
    z-index: 100;
  }

  header {
    color: black;
    font-size: 2em;
    text-align: left;
    margin-left: 20px;
  }

  p {
    color: black;
  }

  input[type='number']::-webkit-inner-spin-button,
  input[type='number']::-webkit-outer-spin-button {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    margin: 0;
  }

  label {
    color: black;
  }

  .guess {
    display: inline-block;
    flex-wrap: wrap;
  }
</style>
