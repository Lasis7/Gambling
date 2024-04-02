<script>
  let isDisabled = true;

  const navBar = ['Blackjack', 'Roulette', 'Bingo', 'Pinball'];
</script>

<div class="container">
  <p>Coming soon!</p>
  <div class="navItem">
    {#each navBar as navItem}
      <button disabled={isDisabled}>{navItem}</button>
    {/each}
  </div>
</div>

<style>
  .navItem {
    background-color: root;
    padding: 0.2em;
    margin: 1px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
  }

  button {
    background: none;
  }

  button:disabled {
    cursor: default;
  }

  p {
    color: red;
    display: flex;
    justify-content: center;
    margin-bottom: 0;
  }
</style>
