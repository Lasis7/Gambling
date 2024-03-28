<script>
  import { onDestroy } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  import guess from '../guessStore';
  import { createEventDispatcher } from 'svelte';

  //numbers = guess store
  let numbers;

  let showNumber = false;

  let howManyClicked = 0;

  let winOrLost = '';
  let randomNumber = null;

  $: next = howManyClicked >= 8;

  const dispatch = createEventDispatcher();

  const unsub = guess.subscribe((guessNumbers) => (numbers = guessNumbers));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  const delay = (time) => {
    return new Promise((resolve) => setTimeout(resolve, time));
  };

  const nextNumber = async () => {
    howManyClicked++;
    showNumber = false;
    randomNumber = Math.floor(Math.random() * 20) + 1;
    if (numbers.includes(randomNumber)) {
      winOrLost = 'You won a dollar';
      dispatch('victory');
    } else {
      winOrLost = 'You won nothing';
    }
    console.log(winOrLost);
    let yeah = await delay(1000);
    showNumber = true;
  };
</script>

{#if showNumber}
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

{#if !next}
  <div class="container">
    <button on:click={nextNumber}>Next number</button>
  </div>
{:else}
  <div class="container">
    <div class="next">
      <button on:click={nextNumber} disabled={next}>Next number</button>
      <button on:click={() => dispatch('next')}>Next</button>
    </div>
  </div>
{/if}

<div class="window">
  {winOrLost}
</div>

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
    position: absolute;
    margin-top: 30px;
  }
</style>
