<script>
  import Heading from './lib/Heading.svelte';
  import Navbar from './lib/Navbar.svelte';
  import Weather from './lib/Weather.svelte';
  import Modal from './lib/Modal.svelte';
  import Info from './lib/Info.svelte';
  import PrintNumber from './lib/PrintNumber.svelte';
  import TransferMoney from './lib/TransferMoney.svelte';
  import guess from './guessStore';
  import { onDestroy } from 'svelte';

  //_____VARIABLES BELOW THIS_____

  //------------------------------------
  //VARIABLES RELATED TO: __App.svelte__
  //------------------------------------

  let balance = 2;
  let gameOver = false;
  $: notEnoughMoney = balance < 4;

  //------------------------------------
  //VARIABLES RELATED TO: __Info.svelte__
  //------------------------------------

  let infoVisible = false;

  //----------------------------------------
  //VARIABLES RELATED TO: __Weather.svelte__
  //----------------------------------------

  let weatherVisible = false;

  //----------------------------------------------
  //VARIABLES RELATED TO: __TransferMoney.svelte__
  //----------------------------------------------

  //these are used for validation in TransferMoney.svelte

  let firstName = '';
  let secondName = '';
  let surname = '';
  let age = null;
  let dateOfBirthYear = null;
  let gmail = '';
  let iceCream = '';
  let shower = '';
  let finger = null;
  let waterTaste = '';
  let bank = '';
  let cardNumber = null;

  //these are used for making the modal itself visible + captcha functionality
  let moneyModal = false;
  let captchaLoading = false;
  let notice = true;
  let captchaComplete = false;

  //--------------------------------------
  //VARIABLES RELATED TO: __Modal.svelte__
  //--------------------------------------

  let howManyClicked = 0;
  let modalVisible = false;
  let modalClosed = false;
  let submits = 8;

  $: isModalClosed = modalClosed;

  //------------------------------------
  //VARIABLES RELATED TO: __PrintNumber.svelte__
  //------------------------------------

  let showNumber = false;
  let outcomeVisible = false;
  let randomNumber = null;
  let winOrLost = '';
  let howMuchWon = 0;
  let randomNumbers = [];
  let showProfit = false;

  let numberDraw = false;

  //_____FUNCTIONS BELOW THIS_____

  //------------------------------------
  //FUNCTIONS RELATED TO: __App.svelte__
  //------------------------------------

  //Cancel button in App.svelte
  const cancelApp = () => {
    modalClosed = false;
    guess.set([]);
  };

  //Start the game after paying
  const numberPrint = () => {
    numberDraw = true;
    balance -= 4;
  };

  //What is visible after the game ends
  const gameEnded = () => {
    isModalClosed = true;
    numberDraw = true;
    gameOver = true;
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
    outcomeVisible = false;
    showNumber = false;
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
    howManyClicked = 0;
    showProfit = false;
    outcomeVisible = false;
    showNumber = false;
    howMuchWon = 0;
    winOrLost = '';
    howManyClicked = 0;
    randomNumber = null;
    randomNumbers = [];
  };

  //-------------------------------------
  //FUNCTIONS RELATED TO: __Info.svelte__
  //-------------------------------------

  //Open the info-modal on the first view
  const openInfo = () => {
    infoVisible = true;
  };

  //closes the info-modal
  const closeInfo = () => {
    infoVisible = false;
  };

  //----------------------------------------
  //FUNCTIONS RELATED TO: __Weather.svelte__
  //----------------------------------------

  const showWeather = () => {
    weatherVisible = true;
  };

  const closeWeather = () => {
    weatherVisible = false;
  };
  //----------------------------------------------
  //FUNCTIONS RELATED TO: __TransferMoney.svelte__
  //----------------------------------------------

  //opens the modal for adding money in case you run out
  const moneyModalShow = () => {
    moneyModal = true;
  };

  //function for the captcha process
  const captchaCompleted = () => {
    captchaLoading = true;

    setTimeout(() => {
      captchaLoading = false;
      captchaComplete = true;
    }, 3000);

    setTimeout(() => {
      notice = false;
    }, 100);
  };

  const transferMoney = () => {
    balance += 16;
    moneyModal = false;
    firstName = '';
    secondName = '';
    surname = '';
    age = null;
    dateOfBirthYear = null;
    gmail = '';
    iceCream = '';
    shower = '';
    finger = null;
    waterTaste = '';
    bank = '';
    cardNumber = null;
    captchaComplete = false;
  };

  const cancelMoney = () => {
    moneyModal = false;
    firstName = '';
    secondName = '';
    surname = '';
    age = null;
    dateOfBirthYear = null;
    gmail = '';
    iceCream = '';
    shower = '';
    finger = null;
    waterTaste = '';
    bank = '';
    cardNumber = null;
    captchaComplete = false;
    captchaLoading = false;
  };

  //--------------------------------------
  //FUNCTIONS RELATED TO: __Modal.svelte__
  //--------------------------------------

  //Brings out the modal when you first press the play-button.
  const playStart = () => {
    modalVisible = true;
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

  //Cancel button in modal
  const cancelModal = () => {
    guess.set([]);
    modalVisible = false;
    modalClosed = false;
  };

  //----------------------------------------
  //FUNCTIONS THINGS RELATED TO: __guessStore.js__
  //----------------------------------------

  //numbers is for guessStore.js
  let numbers;

  const unsub = guess.subscribe((storeNumber) => (numbers = storeNumber));

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  //--------------------------------------------
  //FUNCTIONS RELATED TO: __PrintNumber.svelte__
  //--------------------------------------------

  //function for showing the results-view
  const profitShown = () => {
    showProfit = true;
  };

  //a small delay for the function below so that animations have time to finish
  const delay = (time) => {
    return new Promise((resolve) => setTimeout(resolve, time));
  };

  //this monster of a function is used for when the actual game starts
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

{#if !captchaLoading}
  <Heading heading="Ballgame©®℗™℠" />

  <Navbar />

  <div class="container">
    <div class="weather-moneyContainer">
      <div class="weatherItem">
        <div class="weatherContainer">
          <button class="weatherButton" on:click={showWeather}>Click me!</button
          >
        </div>
      </div>
      <div class="moneyItem">
        <div class="money">Balance: {balance}$</div>
        <div class="moreMoney">
          <button class="moreMoneyButton" on:click={moneyModalShow}
            >Transfer money</button
          >
        </div>
      </div>
    </div>
  </div>

  {#if weatherVisible}
    <Weather on:closeWeather={closeWeather} />
  {/if}

  {#if moneyModal}
    <TransferMoney
      {captchaLoading}
      {captchaComplete}
      on:captcha={captchaCompleted}
      on:confirm={transferMoney}
      on:cancel={cancelMoney}
      bind:firstName
      bind:secondName
      bind:surname
      bind:age
      bind:dateOfBirthYear
      bind:gmail
      bind:iceCream
      bind:shower
      bind:finger
      bind:waterTaste
      bind:bank
      bind:cardNumber
      {balance}
      {notice}
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
          {#if notEnoughMoney}
            <div class="errorPlayButton">Not enough money!</div>
            <div class="errorPlayButton">Transfer money to play!</div>
          {/if}
          <div class="container">
            <div class="modalClosed">
              <button on:click={numberPrint} disabled={notEnoughMoney}
                >Start
                <p class="cost">$4</p></button
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
  <!---->
{:else}
  <!---->
  <div class="loading">
    <Heading heading="Pallokeno" />

    <Navbar />

    <div class="container">
      <div class="weather-moneyContainer">
        <div class="weatherItem">
          <div class="weatherContainer">
            <button class="weatherButton" on:click={showWeather}
              >Click me!</button
            >
          </div>
        </div>
        <div class="moneyItem">
          <div class="money">Balance: {balance}$</div>
          <div class="moreMoney">
            <button class="moreMoneyButton" on:click={moneyModalShow}
              >Transfer money</button
            >
          </div>
        </div>
      </div>
    </div>

    {#if moneyModal}
      <TransferMoney
        {captchaLoading}
        {captchaComplete}
        on:cancel={cancelMoney}
        bind:firstName
        bind:secondName
        bind:surname
        bind:age
        bind:dateOfBirthYear
        bind:gmail
        bind:iceCream
        bind:shower
        bind:finger
        bind:waterTaste
        bind:bank
        bind:cardNumber
        {balance}
        {notice}
      />
    {/if}

    <div class="container">
      <div class="view">
        <div class="insideView">
          <div class="container">
            <div class="modalNotClosed">
              <button>Play</button>
              <button>How to play?</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
{/if}

<div class="container-norefund">
  <div class="item-norefund">
    Gaming is not a real solution to financial issues
  </div>
</div>

<style>
  .weatherContainer {
    display: flex;
    justify-content: left;
    align-content: flex-start;
  }

  .weatherButton {
    font-size: 1.3rem;
    color: rgb(64, 61, 61);
    background-color: blanchedalmond;
    margin-top: 10px;
    font-size: 0.76rem;
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
    color: rgb(64, 61, 61);
    background-color: blanchedalmond;
    margin-top: 0;
    font-size: 0.76rem;
  }

  .view {
    border: solid 1px white;
    background-color: rgb(99, 97, 97);
    display: flex;
    margin-top: 30px;
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

  .loading {
    cursor: progress;
  }

  .errorPlayButton {
    color: red;
    font-size: 0.9rem;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    padding-left: 0.5rem;
    padding-right: 0.5rem;
  }

  .weather-moneyContainer {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    margin-bottom: 20px;
  }

  .weatherItem {
    display: grid;
    grid-column: 1;
  }

  .moneyItem {
    display: grid;
    grid-column: 3;
  }

  .container-norefund {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }

  .item-norefund {
    display: grid;
    grid-column: 2;
    justify-content: right;
    color: red;
    font-size: 0.5rem;
  }

  button:hover {
    color: aqua;
  }

  button:disabled {
    color: rgb(103, 98, 98);
    cursor: default;
  }
</style>
