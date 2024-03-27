<script>
  //imported these two for store purposes (guessStore)
  import guess from '../guessStore';
  import { onDestroy } from 'svelte';

  import { fly } from 'svelte/transition';

  //custom event
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  //number that shows how many guesses you need to submit
  export let submits;

  //guessNumber is for user inputs, numbers is for storing the store data
  let guessNumber = null;
  let numbers;

  $: submitDisabled = submits < 1 || guessNumber < 1 || guessNumber > 20;
  //$: tooBigGuess = (guessNumber < 1 || guessNumber > 20) && submits !== 0;
  $: errorMessage = submits > 0;
  $: confirmButton = submits > 0;

  //hasClicked for the error message, checking if the user has clicked the input field already
  // let hasClicked = false;

  // const blur = () => {
  //   hasClicked = true;
  // };

  const unsub = guess.subscribe((storeNumber) => (numbers = storeNumber));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  //array is what is already stored into the guessStore
  const count = () => {
    guess.update((array) => [...array, guessNumber]);
    submits--;
    guessNumber = null;
  };
</script>

<div class="backdrop" />

<div class="modal">
  <header>Guess and win!</header>
  <hr />

  {#if errorMessage}
    <p class="error" out:fly={{ duration: 1000, x: 500, y: 0 }}>
      The number needs to be between 1-20!
    </p>
  {/if}

  <p>Submit <u>{submits}</u> more numbers</p>

  <div class="guess">
    <label>
      Guess a number <input
        type="number"
        bind:value={guessNumber}
        on:blur={blur}
      />
    </label>

    <button on:click={count} disabled={submitDisabled}>Submit</button>
  </div>

  <hr />

  <div class="cancelConfirm">
    <button on:click={() => dispatch('cancel')}>Cancel</button>
    <button on:click={() => dispatch('confirm')} disabled={confirmButton}
      >Confirm</button
    >
  </div>
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 1000vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    top: 200px;
    left: 305px;
    width: 60%;
    max-height: 80vh;
    background: white;
    border-radius: 50px;
    z-index: 100;
  }

  header {
    color: black;
    font-size: 2em;
    text-align: left;
    margin-left: 20px;
    margin-top: 10px;
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

  button:disabled {
    cursor: not-allowed;
  }

  button:hover {
    color: aqua;
  }

  .error {
    color: black;
    font: bold;
    font-family: cursive;
    font-size: 1.2em;
  }

  .cancelConfirm {
    margin-top: 20px;
    margin-right: 30px;
    margin-bottom: 20px;
    display: flex;
    justify-content: right;
    gap: 10px;
  }
</style>
