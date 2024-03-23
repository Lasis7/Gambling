<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';
  import guess from './guessStore';
  import { onDestroy } from 'svelte';
  import Info from './lib/Info.svelte';

  let modalVisible = false;
  let modalClosed = false;
  let infoVisible = false;
  let submits = 8;
  //numbers is for storing the store data
  let numbers;

  $: isModalClosed = modalClosed;

  const unsub = guess.subscribe((storeNumber) => (numbers = storeNumber));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  //Brings out the modal when you first press the play-button.
  const playStart = () => {
    modalVisible = true;
  };

  const openInfo = () => {
    infoVisible = true;
  };

  //Used on custom event, one for closing the modal, other for recognizing that it has been closed, so the button can change
  const modalClose = () => {
    modalVisible = false;
    modalClosed = true;
  };

  //Cancel button in App.svelte
  const cancelApp = () => {
    modalClosed = false;
  };

  //Cancel button in modal
  const cancelModal = () => {
    guess.set([]);
    modalVisible = false;
    modalClosed = false;
  };

  const closeInfo = () => {
    infoVisible = false;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="container">
  <div class="view">
    <div class="insideView">
      {#if !isModalClosed}
        <div class="modalNotClosed">
          <button on:click={playStart}>Play</button>
          <button on:click={openInfo}>How to play?</button>
        </div>
      {:else}
        <div class="modalClosed">
          <button
            >Start
            <p class="cost">$4</p></button
          >
          <button on:click={cancelApp}>Cancel</button>
        </div>
      {/if}
    </div>
  </div>
</div>

{#if infoVisible}
  <Info on:close={closeInfo} />
{/if}

{#if modalVisible}
  <div class="container">
    <Modal {submits} on:confirm={modalClose} on:cancel={cancelModal} />
  </div>
{/if}

<style>
  .view {
    border: solid 1px white;
    margin-top: 100px;
    background-color: rgb(99, 97, 97);
    display: flex;
    justify-content: center;
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

  .modalNotClosed {
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
