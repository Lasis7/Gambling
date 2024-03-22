<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';

  let modalVisible = false;
  let modalClosed = false;
  let submits = 8;

  $: isModalClosed = modalClosed;

  //Brings out the modal when you first press the play-button.
  const playStart = () => {
    modalVisible = true;
  };

  //Used on custom event, one for closing the modal, other for recognizing that it has been closed, so the button can change
  const modalClose = () => {
    modalVisible = false;
    modalClosed = true;
  };

  const cancel = () => {
    modalClosed = false;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="view">
  <div class="insideView">
    {#if !isModalClosed}
      <button on:click={playStart}>Play</button>
    {:else}
      <div class="modalClosed">
        <button on:click={cancel}>Cancel</button>
        <button
          >Start
          <p class="cost">$4</p></button
        >
      </div>
    {/if}
  </div>
</div>

{#if modalVisible}
  <Modal {submits} on:close={modalClose} />
{/if}

<style>
  .view {
    border: solid 1px white;
    margin-top: 100px;
    background-color: rgb(99, 97, 97);
  }

  .insideView {
    margin: 300px;
  }

  .modalClosed {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
    margin-bottom: 10px;
  }

  .cost {
    font-size: 0.9em;
    color: rgb(162, 158, 158);
  }

  button:hover {
    color: aqua;
  }
</style>
