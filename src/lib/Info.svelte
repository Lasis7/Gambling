<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
</script>

<div class="backdrop" />

<div class="modal">
  <header>How to play</header>
  <hr />
  <ol type="1">
    <li>Press play.</li>
    <li>Submit the asked number of numbers.</li>
    <li>Press start. Each game costs $4.</li>
    <li>
      The numbers will be randomly generated. The message indicates whether you
      won or not.
    </li>
  </ol>
  <hr />
  <p>
    Note: if you run out of money, transfer more to your balance using "Transfer
    money"-button.
  </p>
  <hr />
  <footer>
    <button on:click={() => dispatch('close')}>Exit</button>
  </footer>
</div>

<style>
  header {
    font-size: 3em;
    color: black;
  }

  footer {
    display: flex;
    justify-content: left;
  }

  ol li {
    color: black;
    text-align: left;
  }

  p {
    text-align: left;
    color: black;
    margin-left: 25px;
  }

  footer {
    justify-content: right;
  }

  button {
    margin-top: 20px;
    margin-right: 30px;
    margin-bottom: 20px;
  }

  button:hover {
    color: rgb(218, 248, 46);
  }

  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 1000vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    top: 200px;
    left: 305px;
    width: 60%;
    max-height: 80vh;
    background: white;
    border-radius: 50px;
    z-index: 100;
  }
</style>
