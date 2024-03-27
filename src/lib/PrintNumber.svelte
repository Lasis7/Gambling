<script>
  import printNumber from '../numberPrintStore';
  import { onDestroy } from 'svelte';

  let numbers;

  const unsub = printNumber.subscribe(
    (randomNumbers) => (numbers = randomNumbers)
  );

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });
</script>

{#each numbers as number}
  <div class="ball">
    {number}
  </div>
{/each}

<style>
  .ball {
    border: 1px solid white;
    border-radius: 50px;
  }
</style>
