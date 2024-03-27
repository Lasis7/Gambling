<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';
  import Info from './lib/Info.svelte';
  import PrintNumber from './lib/PrintNumber.svelte';
  import guess from './guessStore';
  import printNumber from './numberPrintStore';
  import { onDestroy } from 'svelte';

  let modalVisible = false;
  let modalClosed = false;
  let infoVisible = false;
  let numberSet = false;
  let submits = 8;

  //numbers is for guessStore.js, numbers2 is for numberPrintStore.js
  let numbers;
  let numbers2;

  let randomNumbers = [];

  $: isModalClosed = modalClosed;

  const unsub = guess.subscribe((storeNumber) => (numbers = storeNumber));
  const unsubscribe = printNumber.subscribe((print2) => (numbers2 = print2));

  onDestroy(() => {
    if (unsub) {
      unsub();
      unsubscribe();
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

  const setNumberPrint = () => {
    for (let i = 0; i < 8; i++) {
      randomNumbers.push(Math.floor(Math.random() * 20) + 1);
    }
    printNumber.set([...randomNumbers]);
    numberSet = true;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="container">
  <div class="view">
    <div class="insideView">
      {#if !isModalClosed && !numberSet}
        <div class="modalNotClosed">
          <button on:click={playStart}>Play</button>
          <button on:click={openInfo}>How to play?</button>
        </div>
      {:else if isModalClosed && !numberSet}
        <div class="modalClosed">
          <button on:click={setNumberPrint}
            >Start
            <p class="cost">$4</p></button
          >
          <button on:click={cancelApp}>Cancel</button>
        </div>
      {:else if isModalClosed && numberSet}
        <PrintNumber />
      {/if}
    </div>
  </div>
</div>

{#if infoVisible}
  <Info on:close={closeInfo} />
{/if}

{#if modalVisible}
  <Modal {submits} on:confirm={modalClose} on:cancel={cancelModal} />
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
