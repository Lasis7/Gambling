<script>
</script>

<!--Each "line" of modal has two spots to fill in your guess-->
<div class="backdrop">
  <div class="modal">
    <slot name="header" />
    <hr />
    <slot name="guess1" />
    <hr />
    <slot name="guess2" />
    <hr />
    <slot name="guess3" />
    <hr />
    <slot name="guess4" />
    <hr />
    <slot name="footer" />
  </div>
</div>

<style>
  .backdrop {
    background-color: gray;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vw;
    background: 0, 0, 0, 0.5;
    z-index: 1;
  }

  .modal {
    position: absolute;
    top: 50%;
    left: 50%;
    background: rgb(54, 115, 110);
    width: 50%;
    height: 400px;
  }
</style>
