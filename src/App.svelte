<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Modal from './lib/Modal.svelte';
  import Info from './lib/Info.svelte';
  import PrintNumber from './lib/PrintNumber.svelte';
  import TransferMoney from './lib/TransferMoney.svelte';
  import guess from './guessStore';
  import { onDestroy } from 'svelte';

  let howManyClicked = 0;
  let showNumber = false;
  let outcomeVisible = false;
  let randomNumber = null;
  let winOrLost = '';
  let howMuchWon = 0;
  let randomNumbers = [];
  let showProfit = false;
  let guessNumber = null;

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
  let moneyModal = false;
  let captchaLoading = false;
  let captchaComplete = false;
  let submits = 8;

  let balance = 15;

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
  const modalClose = (ce) => {
    guess.set(ce.detail);
    modalVisible = false;
    modalClosed = true;
  };

  //Cancel button in App.svelte
  const cancelApp = () => {
    modalClosed = false;
    guess.set([]);
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
    balance -= 3;
  };

  //What is visible after the game ends
  const gameEnded = () => {
    isModalClosed = true;
    numberDraw = true;
    gameOver = true;
  };

  const profitShown = () => {
    showProfit = true;
  };

  const transferMoney = () => {
    moneyModal = true;
  };

  const captchaCompleted = () => {
    captchaLoading = true;

    setTimeout(() => {
      captchaLoading = false;
      captchaComplete = true;
    }, 3000);
  };

  //Takes you to the first view (aka main menu)
  const appMain = () => {
    guess.set([]);
    modalVisible = false;
    modalClosed = false;
    infoVisible = false;
    numberDraw = false;
    gameOver = false;
    showProfit = false;
    howMuchWon = 0;
    winOrLost = '';
    howManyClicked = 0;
    randomNumber = null;
    randomNumbers = [];
  };

  //Opens the modal and starts a new game
  const playAgain = () => {
    guess.set([]);
    modalVisible = true;
    modalClosed = false;
    infoVisible = false;
    numberDraw = false;
    gameOver = false;
    showProfit = false;
    howManyClicked = 0;
    showProfit = false;
    howMuchWon = 0;
    winOrLost = '';
    howManyClicked = 0;
    randomNumber = null;
    randomNumbers = [];
  };

  const delay = (time) => {
    return new Promise((resolve) => setTimeout(resolve, time));
  };

  const showNextNumber = async () => {
    howManyClicked++;
    showNumber = false;
    outcomeVisible = false;
    randomNumber = Math.floor(Math.random() * 20) + 1;
    if (numbers.includes(randomNumber)) {
      winOrLost = 'You won a dollar';
      howMuchWon++;
      balance++;
    } else {
      winOrLost = 'You won nothing';
    }
    randomNumbers.push(randomNumber);
    let yeah = await delay(1000);
    showNumber = true;
    outcomeVisible = true;
  };
</script>

{#if captchaLoading}
  <div class="loading">
    <Heading heading="Pallokeno" />

    <Navbar />

    <div class="container">
      <div class="money">Balance: {balance}$</div>
      <div class="moreMoney">
        <button class="moreMoneyButton" on:click={transferMoney}
          >Transfer money</button
        >
      </div>
    </div>

    {#if moneyModal}
      <TransferMoney
        {captchaLoading}
        {captchaComplete}
        on:click={captchaCompleted}
      />
    {/if}

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
                  <p class="cost">$3</p></button
                >
                <button on:click={cancelApp}>Cancel</button>
              </div>
            </div>
          {:else if isModalClosed && numberDraw && !gameOver}
            <PrintNumber
              {howManyClicked}
              {showProfit}
              {outcomeVisible}
              {winOrLost}
              {showNumber}
              {randomNumber}
              {randomNumbers}
              {howMuchWon}
              on:profit={profitShown}
              on:next={gameEnded}
              on:nextNumber={showNextNumber}
            />
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
  </div>
{:else}
  <Heading heading="Pallokeno" />

  <Navbar />

  <div class="container">
    <div class="money">Balance: {balance}$</div>
    <div class="moreMoney">
      <button class="moreMoneyButton" on:click={transferMoney}
        >Transfer money</button
      >
    </div>
  </div>

  {#if moneyModal}
    <TransferMoney
      {captchaLoading}
      {captchaComplete}
      on:click={captchaCompleted}
    />
  {/if}

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
                <p class="cost">$3</p></button
              >
              <button on:click={cancelApp}>Cancel</button>
            </div>
          </div>
        {:else if isModalClosed && numberDraw && !gameOver}
          <PrintNumber
            {howManyClicked}
            {showProfit}
            {outcomeVisible}
            {winOrLost}
            {showNumber}
            {randomNumber}
            {randomNumbers}
            {howMuchWon}
            on:profit={profitShown}
            on:next={gameEnded}
            on:nextNumber={showNextNumber}
          />
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
{/if}

<style>
  .view {
    border: solid 1px white;
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

  .money {
    display: flex;
    justify-content: right;
    align-content: flex-start;
    font-size: 1.3rem;
    margin-top: 10px;
    margin-bottom: 5px;
    color: gray;
  }

  .moreMoney {
    display: flex;
    justify-content: right;
    align-content: flex-start;
    margin-top: 0;
  }

  .moreMoneyButton {
    display: flex;
    font-size: 1.3rem;
    margin-bottom: 50px;
    color: rgb(64, 61, 61);
    background-color: blanchedalmond;
    margin-top: 0;
    font-size: 0.76rem;
  }

  button:hover {
    color: rgb(103, 98, 98);
  }

  .loading {
    cursor: progress;
  }
</style>
