<script>
  //imported these two for store purposes (guessStore)
  import guess from '../guessStore';
  import { onDestroy } from 'svelte';

  //number that shows how many guesses you need to submit
  export let submits;

  //guessNumber is for user inputs, numbers is for storing the store data
  let guessNumber = null;
  let numbers;

  $: submitDisabled = submits < 1 || guessNumber < 1 || guessNumber > 20;

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
    console.log(numbers);
    guessNumber = '';
  };
</script>

<div class="backdrop" />
<div class="modal">
  <header>Guess and win!</header>
  <hr />
  <p>Submit {submits} more numbers</p>

  <div class="guess">
    <label>
      Guess a number <input type="number" bind:value={guessNumber} />
    </label>

    <button on:click={count} disabled={submitDisabled}>Submit</button>
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
