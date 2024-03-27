<script>
  import { onDestroy } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  import guess from '../guessStore';

  //numbers = guess store
  let numbers;

  let showNumber = false;

  let howManyClicked = 0;

  let winOrLost = '';
  let randomNumber = null;

  $: nextNumberDisabled = howManyClicked >= 8;

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
    } else {
      winOrLost = 'You won nothing';
    }
    console.log(winOrLost);
    let yeah = await delay(1000);
    showNumber = true;
    yeah = await delay(1000);
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

<div class="container">
  <button on:click={nextNumber} disabled={nextNumberDisabled}
    >Next number</button
  >
</div>

<style>
  .ball {
    border: 1px solid white;
    border-radius: 50px;
    padding: 1rem;
  }

  button {
    margin-top: 50px;
  }
</style>
