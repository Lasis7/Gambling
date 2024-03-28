<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';
  import Info from './lib/Info.svelte';
  import PrintNumber from './lib/PrintNumber.svelte';
  import guess from './guessStore';
  import { onDestroy } from 'svelte';

  /*
  modalvisible =  Is the modal visible, playStart-function
  modalClosed = Has the modal been closed/has the user given their numbers. Used to determine what is visible on view
  infoVisible = Is the info modal visible, closeInfo-function
  numberDraw = Indicates whether the game has been started (start-button), numberPrint-function
  gameOver = Indicates whether the game has ended. appMain- and playAgain-functions.
  submits = variable for Modal.svelte, shows how many more numbers u need to submit
  */
  let modalVisible = false;
  let modalClosed = false;
  let infoVisible = false;
  let numberDraw = false;
  let gameOver = false;
  let submits = 8;

  //numbers is for guessStore.js
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

  //Open the info-modal on the first view
  const openInfo = () => {
    infoVisible = true;
  };

  /*
  Used on custom event, one for closing the modal, other for recognizing that it has been closed
  so the button can change
  */
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

  //closes the info-modal
  const closeInfo = () => {
    infoVisible = false;
  };

  //Start the game after paying
  const numberPrint = () => {
    numberDraw = true;
  };

  //What is visible after the game ends
  const gameEnded = () => {
    isModalClosed = true;
    numberDraw = true;
    gameOver = true;
  };

  //Takes you to the first
  const appMain = () => {
    guess.set([]);
    modalVisible = false;
    modalClosed = false;
    infoVisible = false;
    numberDraw = false;
    gameOver = false;
  };

  //Opens the modal and starts a new game
  const playAgain = () => {
    guess.set([]);
    modalVisible = true;
    modalClosed = false;
    infoVisible = false;
    numberDraw = false;
    gameOver = false;
  };
</script>

<Heading heading="Pallokeno" />

<Navbar />

<div class="container">
  <div class="view">
    <div class="insideView">
      {#if !isModalClosed && !numberDraw && !gameOver}
        <div class="container">
          <div class="modalNotClosed">
            <button on:click={playStart}>Play</button>
            <button on:click={openInfo}>How to play?</button>
          </div>
        </div>
      {:else if isModalClosed && !numberDraw && !gameOver}
        <div class="container">
          <div class="modalClosed">
            <button on:click={numberPrint}
              >Start
              <p class="cost">$4</p></button
            >
            <button on:click={cancelApp}>Cancel</button>
          </div>
        </div>
      {:else if isModalClosed && numberDraw && !gameOver}
        <PrintNumber on:next={gameEnded} />
      {:else if isModalClosed && numberDraw && gameOver}
        <div class="container">
          <div class="gameOver">
            <button on:click={appMain}>Main</button>
            <button on:click={playAgain}>Play again</button>
          </div>
        </div>
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

  .gameOver {
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
