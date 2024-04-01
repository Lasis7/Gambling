<script>
  import { onDestroy } from 'svelte';
  import { fly, scale, slide } from 'svelte/transition';
  import guess from '../guessStore';
  import { createEventDispatcher } from 'svelte';

  let numbers; //variable for storing the contents of guess store

  export let showNumber; //show next randomly generated number

  export let howManyClicked;

  export let winOrLost; //winOrLost changes depending on the value of the randomNumber-variable
  export let randomNumber; //randomly generated number, shown one by one
  export let randomNumbers; //generated numbers are pushed here for the results-sceen

  $: nextNumberButton = howManyClicked >= 8;
  $: didProfit = howMuchWon > 3;
  $: noProfit = howMuchWon < 3;
  $: even = howMuchWon = 0;

  export let howMuchWon;

  export let outcomeVisible;
  export let showProfit;

  const dispatch = createEventDispatcher();

  const unsub = guess.subscribe((guessNumbers) => (numbers = guessNumbers));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });
</script>

{#if showNumber && !showProfit}
  <div class="container">
    <div
      class="ball"
      in:fly={{ duration: 750, x: -500, y: 0 }}
      out:scale={{ duration: 750 }}
    >
      {randomNumber}
    </div>
  </div>
{/if}

{#if !nextNumberButton && !showProfit}
  <div class="container">
    <button on:click={() => dispatch('nextNumber')}>Next number</button>
  </div>
{:else if nextNumberButton && !showProfit}
  <div class="container">
    <div class="next">
      <button disabled={nextNumberButton}>Next number</button>
      <button on:click={() => dispatch('profit')}>Next</button>
    </div>
  </div>
{:else if nextNumberButton && showProfit}
  <div class="scoreboard">
    Winnings: {howMuchWon}$
  </div>
  <div class="scoreboard">Profit:</div>
  <div class="scoreboard">Your guesses: {numbers.join(', ')}</div>
  <div class="scoreboard">Drawn numbers: {randomNumbers.join(', ')}</div>
  <div class="next">
    <button on:click={() => dispatch('next')}>Next</button>
  </div>
{/if}

{#if outcomeVisible && !showProfit}
  <div class="container">
    <div
      class="window"
      in:slide={{ duration: 750, axis: 'x' }}
      out:slide={{ duration: 1000, axis: 'y' }}
    >
      {winOrLost}
    </div>
  </div>
{/if}

<style>
  .ball {
    border: 1px solid white;
    border-radius: 50px;
    padding: 1rem;
  }

  button {
    margin-top: 20px;
  }

  button:hover {
    color: aqua;
  }

  button:disabled {
    cursor: not-allowed;
    color: gray;
  }

  .next {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
    margin-bottom: 10px;
  }

  .window {
    background-color: gray;
    padding: 1em;
    border-radius: 5px;
    font-size: 1.5rem;
    display: flex;
    justify-content: center;
    margin-top: 30px;
  }

  .scoreboard {
    justify-content: center;
    margin-top: 10px;
  }
</style>
