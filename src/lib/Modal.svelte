<script>
</script>

<!--Each "line" of modal has two spots to fill in your guess-->
<div class="backdrop" />
<div class="modal">
  <header>Make you guesses</header>
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    top: 200px;
    left: 120px;
    width: 80%;
    max-height: 80vh;
    background: white;
    border-radius: 5px;
    z-index: 100;
  }
</style>
