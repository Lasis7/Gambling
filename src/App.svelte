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

  const modalClose = () => {
    modalVisible = false;
    modalClosed = true;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="view">
  {#if !isModalClosed}
    <button on:click={playStart}>Play</button>
  {:else}
    <button>Start</button>
  {/if}
</div>

{#if modalVisible}
  <Modal {submits} on:close={modalClose} />
{/if}

<style>
  .view {
    border: solid 1px white;
    margin-top: 100px;
  }
</style>
