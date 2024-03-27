<script>
  import printNumber from '../numberPrintStore';
  import { onDestroy } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  import guess from '../guessStore';

  //numbers = random number, numbers2 = guess numbers
  let numbers;
  let numbers2;

  let i = -1;
  let showNumber = false;
  let winOrLost;

  $: nextNumberDisabled = i > 6;

  const unsub = printNumber.subscribe(
    (randomNumbers) => (numbers = randomNumbers)
  );

  const unsubscribe = guess.subscribe(
    (guessNumbers) => (numbers2 = guessNumbers)
  );

  $: if (numbers2.includes(numbers)) {
    winOrLost = 'You won 1 dollar';
  } else {
    winOrLost = 'You did not win this time';
  }

  onDestroy(() => {
    if (unsub) {
      unsub();
      unsubscribe();
    }
  });

  const delay = (time) => {
    return new Promise((resolve) => setTimeout(resolve, time));
  };

  const nextNumber = async () => {
    showNumber = false;
    let yeah = await delay(1000);
    i++;
    showNumber = true;
    yeah = await delay(1000);
    console.log(winOrLost);
  };
</script>

{#if showNumber}
  <div class="container">
    <div
      class="ball"
      in:fly={{ duration: 750, x: -500, y: 0 }}
      out:scale={{ duration: 750 }}
    >
      {numbers[i]}
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
