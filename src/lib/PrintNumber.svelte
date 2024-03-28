<script>
  import { onDestroy } from 'svelte';
  import { fly, scale, slide } from 'svelte/transition';
  import guess from '../guessStore';
  import { createEventDispatcher } from 'svelte';

  //numbers = guess store
  let numbers;
  //show next randomly generated number
  let showNumber = false;

  let howManyClicked = 0;

  //winOrLost changes depending on the value of the randomNumber-variable
  let winOrLost = '';
  let randomNumber = null;

  $: nextNumberButton = howManyClicked >= 8;
  let howMuchWon = 0;

  let outcomeVisible = false;
  let showProfit = false;

  const dispatch = createEventDispatcher();

  const unsub = guess.subscribe((guessNumbers) => (numbers = guessNumbers));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  const profitShown = () => {
    showProfit = true;
  };

  const delay = (time) => {
    return new Promise((resolve) => setTimeout(resolve, time));
  };

  const nextNumber = async () => {
    howManyClicked++;
    showNumber = false;
    outcomeVisible = false;
    randomNumber = Math.floor(Math.random() * 20) + 1;
    if (numbers.includes(randomNumber)) {
      winOrLost = 'You won a dollar';
      howMuchWon++;
      dispatch('victory');
    } else {
      winOrLost = 'You won nothing';
    }
    console.log(winOrLost);
    let yeah = await delay(1000);
    showNumber = true;
    outcomeVisible = true;
  };
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
    <button on:click={nextNumber}>Next number</button>
  </div>
{:else if nextNumberButton && !showProfit}
  <div class="container">
    <div class="next">
      <button on:click={nextNumber} disabled={nextNumberButton}
        >Next number</button
      >
      <button on:click={profitShown}>Next</button>
    </div>
  </div>
{:else if nextNumberButton && showProfit}
  {howMuchWon}
  <button on:click={() => dispatch('next')}>Next</button>
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
</style>
