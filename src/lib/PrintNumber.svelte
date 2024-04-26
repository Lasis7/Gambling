<script>
  import { onDestroy } from 'svelte';
  import { fly, scale, slide } from 'svelte/transition';
  import guess from '../guessStore';
  import { createEventDispatcher } from 'svelte';

  let numbers; //variable for storing the contents of guess store

  export let showNumber; //show next randomly generated number
  export let howManyClicked; //check, when next number is disabled
  export let winOrLost; //winOrLost changes depending on the value of the randomNumber-variable
  export let randomNumber; //randomly generated number, shown one by one
  export let randomNumbers; //generated numbers are pushed here for the results-screen
  export let howMuchWon; //used for checking if you made profit or nah
  export let outcomeVisible; //used for the window that shows result for each drawn number
  export let showProfit; //is results screen visible

  $: nextNumberButton = howManyClicked >= 8;
  $: didProfit = howMuchWon > 4;
  $: noProfit = howMuchWon < 4;
  $: brokeEven = howMuchWon === 4;

  const dispatch = createEventDispatcher();

  const unsub = guess.subscribe((guessNumbers) => (numbers = guessNumbers));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });
</script>

<!--Numbers fly to the screen-->
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

<!--All numbers have been drawn, next number is disabled-->

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

  <!--Results shown when you did profit-->
{:else if nextNumberButton && showProfit && didProfit}
  <h1>Results</h1>
  <div class="scoreboard">
    Winnings: {howMuchWon}$
  </div>
  <div class="scoreboard">
    Profit: <div class="profit">+{0 - 4 + howMuchWon}$</div>
  </div>
  <div class="scoreboard">Your guesses: {numbers.join(', ')}</div>
  <div class="scoreboard">Drawn numbers: {randomNumbers.join(', ')}</div>
  <div class="next">
    <button on:click={() => dispatch('next')}>Next</button>
  </div>

  <!--Results shown when you broke even-->
{:else if nextNumberButton && showProfit && brokeEven}
  <h1>Results</h1>
  <div class="scoreboard">
    Winnings: {howMuchWon}$
  </div>
  <div class="scoreboard">
    Profit: <div class="even">+{howMuchWon - howMuchWon}$</div>
  </div>
  <div class="scoreboard">Your guesses: {numbers.join(', ')}</div>
  <div class="scoreboard">Drawn numbers: {randomNumbers.join(', ')}</div>
  <div class="next">
    <button on:click={() => dispatch('next')}>Next</button>
  </div>

  <!--Results shown when you lost money-->
{:else if nextNumberButton && showProfit && noProfit}
  <h1>Results</h1>
  <div class="scoreboard">
    Winnings: {howMuchWon}$
  </div>
  <div class="scoreboard">
    Profit: <div class="loss">-{4 - howMuchWon}$</div>
  </div>
  <div class="scoreboard">Your guesses: {numbers.join(', ')}</div>
  <div class="scoreboard">Drawn numbers: {randomNumbers.join(', ')}</div>
  <div class="next">
    <button on:click={() => dispatch('next')}>Next</button>
  </div>
{/if}

<!--The window that shows results for each drawn number-->
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
  h1 {
    font-size: 3rem;
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

  .ball {
    border: 1px solid white;
    border-radius: 50px;
    padding: 1rem;
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
    display: flex;
  }

  .profit {
    color: rgb(5, 174, 5);
    display: flex;
    margin-left: 3px;
  }

  .even {
    color: rgb(195, 195, 195);
    display: flex;
    margin-left: 3px;
  }

  .loss {
    color: red;
    display: flex;
    margin-left: 3px;
  }
</style>
