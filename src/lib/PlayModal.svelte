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
  let guessNumbers = [];

  $: submitDisabled = submits < 1 || guessNumber < 1 || guessNumber > 20;
  $: errorMessage = submits > 0;
  $: confirmButton = submits > 0;

  const unsub = guess.subscribe((storeNumber) => (numbers = storeNumber));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  //pushes the guessed number into the array and then sets the value of it back to null
  const count = () => {
    guessNumbers.push(guessNumber);
    submits--;
    guessNumber = null;
  };

  //after the contents have been sent to App.svelte, the array is emptied again
  const guesses = (userGuesses) => {
    dispatch('confirm', userGuesses);
    guessNumbers = [];
  };

  //You can use enter when submitting numbers
  const enterPress = (event) => {
    if (event.key === 'Enter' && !submitDisabled) {
      count();
    }
  };
</script>

<div class="backdrop" />

<div class="modal">
  <header>Guess and win!</header>
  <hr />

  {#if errorMessage}
    <p class="error" out:fly={{ duration: 2000, x: 500, y: 0 }}>
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
        on:keypress={enterPress}
      />
    </label>

    <button on:click={count} disabled={submitDisabled}>Submit</button>
  </div>

  <hr />

  <div class="cancelConfirm">
    <button on:click={() => dispatch('cancel')}>Cancel</button>
    <button on:click={() => guesses(guessNumbers)} disabled={confirmButton}
      >Confirm</button
    >
  </div>
</div>

<style>
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

  label {
    color: black;
  }

  button:disabled {
    cursor: not-allowed;
    color: gray;
  }

  button:hover {
    color: rgb(218, 248, 46);
  }

  input[type='number']::-webkit-inner-spin-button,
  input[type='number']::-webkit-outer-spin-button {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    margin: 0;
  }

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

  .guess {
    display: inline-block;
    flex-wrap: wrap;
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
