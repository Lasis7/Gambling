<script>
  let isDisabled = true;
</script>

<ul class="f">
  <button disabled={isDisabled}>Blackjack</button>
  <button disabled={isDisabled}>Roulette</button>
  <button disabled={isDisabled}>Poker</button>
  <button>ddg</button>
</ul>

<style>
  .f {
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
    cursor: not-allowed;
  }
</style>
