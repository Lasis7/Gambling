<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';

  let modalVisible = false;
  let submits = 8;

  //Brings out the modal when you first press the play-button.
  const playStart = () => {
    modalVisible = true;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="view">
  <button on:click={playStart}>Play</button>
</div>

{#if modalVisible}
  <Modal {submits} />
{/if}

<style>
  .view {
    border: solid 1px white;
    margin-top: 100px;
  }
</style>
