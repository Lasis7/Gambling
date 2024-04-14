<script>
  import { createEventDispatcher } from 'svelte';
  import { fly } from 'svelte/transition';

  export let captchaLoading;
  export let notice;
  export let captchaComplete;

  export let balance;

  export let firstName = '';
  export let secondName = '';
  export let surname = '';
  export let age = null;
  export let dateOfBirthYear = null;
  export let gmail = '';
  export let iceCream = '';
  export let shower = '';
  export let finger = null;
  export let waterTaste = '';
  export let bank = '';
  export let cardNumber = null;

  $: confirmDisabled =
    firstName.length < 1 ||
    surname.length < 1 ||
    age < 0 ||
    age === null ||
    dateOfBirthYear === null ||
    gmail.length < 1 ||
    iceCream.length < 1 ||
    shower.length < 1 ||
    finger < 1 ||
    waterTaste.length < 1 ||
    bank.length < 1 ||
    cardNumber <= 0 ||
    !captchaComplete ||
    balance >= 3;

  $: showNotice =
    firstName.length < 1 ||
    surname.length < 1 ||
    age < 0 ||
    age === null ||
    dateOfBirthYear === null ||
    gmail.length < 1 ||
    iceCream.length < 1 ||
    shower.length < 1 ||
    finger < 1 ||
    waterTaste.length < 1 ||
    bank.length < 1 ||
    cardNumber <= 0 ||
    !captchaComplete;

  $: disableCaptcha =
    firstName.length < 1 ||
    surname.length < 1 ||
    age < 0 ||
    age === null ||
    dateOfBirthYear === null ||
    gmail.length < 1 ||
    iceCream.length < 1 ||
    shower.length < 1 ||
    finger < 1 ||
    waterTaste.length < 1 ||
    bank.length < 1 ||
    cardNumber <= 0;

  $: if (
    firstName.length < 1 ||
    surname.length < 1 ||
    age < 0 ||
    age === null ||
    dateOfBirthYear === null ||
    gmail.length < 1 ||
    iceCream.length < 1 ||
    shower.length < 1 ||
    finger < 1 ||
    waterTaste.length < 1 ||
    bank.length < 1 ||
    cardNumber <= 0
  ) {
    notice = true;
  }

  const dispatch = createEventDispatcher();
</script>

<div class="backdrop">
  <div class="modal">
    <header>Transfer money</header>
    {#if showNotice && notice}
      <p
        in:fly={{ duration: 2000, x: -500, y: 0 }}
        out:fly={{ duration: 2000, x: 500, y: 0 }}
      >
        Please fill the required fields with appropriate inputs
      </p>
    {/if}
    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="firstName"
          >First name
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="firstName" bind:value={firstName} />
      </div>

      <div class="flex-item1">
        <label for="2ndName">Second name</label>
        <input type="text" id="2ndName" bind:value={secondName} />
      </div>

      <div class="flex-item1">
        <label for="surname"
          >Surname
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="surname" bind:value={surname} />
      </div>
    </div>

    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="age"
          >Age
          <div class="requiredSymbol">*</div></label
        >
        <input type="number" id="age" bind:value={age} />
      </div>

      <div class="flex-item1">
        <label class="labelDob" for="DoBY"
          >Date birth &#40;Year&#41;
          <div class="requiredSymbol">*</div></label
        >
        <input type="number" id="DoBY" bind:value={dateOfBirthYear} />
      </div>

      <div class="flex-item1">
        <label for="gmail"
          >Gmail
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="gmail" bind:value={gmail} />
      </div>
    </div>

    <hr />

    <h2>Security questions</h2>

    <div class="container2">
      <div class="flex-item2">
        <label for="iceCream"
          >Favourite ice cream?
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="iceCream" bind:value={iceCream} />
      </div>

      <div class="flex-item2">
        <label for="DoB"
          >Did you shower today?
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="shower" bind:value={shower} />
      </div>

      <div class="flex-item2">
        <label for="finger"
          >The number of fingers in hand?
          <div class="requiredSymbol">*</div></label
        >
        <input type="number" id="finger" bind:value={finger} />
      </div>

      <div class="flex-item2">
        <label for="water"
          >The taste of water?
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="water" bind:value={waterTaste} />
      </div>
    </div>

    <hr />

    <div class="container2">
      <div class="flex-item2">
        <label for="bank"
          >Your bank
          <div class="requiredSymbol">*</div></label
        >
        <input type="text" id="bank" bind:value={bank} />
      </div>

      <div class="flex-item2">
        <label for="cardNumber"
          >Funny bank card number
          <div class="requiredSymbol">*</div></label
        >
        <input type="number" id="cardNumber" bind:value={cardNumber} />
      </div>
    </div>

    <hr />

    <div class="grid-container">
      {#if !captchaComplete && !captchaLoading}
        <div class="flex-infoText">
          Please note that you can't transfer more money if you already have
          enough for a game!
        </div>
        <div class="flex-containerCaptcha">
          <label for="buttonRed">Complete the captcha</label>
          <button
            class="buttonCaptcha"
            id="buttonRed"
            disabled={disableCaptcha}
            on:click={() => dispatch('captcha')}
          ></button>
        </div>
        <div class="flex-containerCancelConfirm">
          <button on:click={() => dispatch('cancel')}>Cancel</button>
          <button disabled={confirmDisabled}>Confirm</button>
        </div>
        <div class="flex-required">
          <div class="requiredText">*Information required</div>
        </div>
      {:else if !captchaComplete && captchaLoading}
        <div class="flex-infoText">
          Please note that you can't transfer more money if you already have
          enough for a game!
        </div>
        <div class="flex-containerCaptcha">
          <label for="buttonYellow">Loading...</label>
          <button class="loading" id="buttonYellow" disabled={captchaLoading}
          ></button>
        </div>
        <div class="flex-containerCancelConfirm">
          <button on:click={() => dispatch('cancel')}>Cancel</button>
          <button disabled={confirmDisabled}>Confirm</button>
        </div>
        <div class="flex-required">
          <div class="requiredText">*Information required</div>
        </div>
      {:else if captchaComplete && !captchaLoading}
        <div class="flex-infoText">
          Please note that you can't transfer more money if you already have
          enough for a game!
        </div>
        <div class="flex-containerCaptcha">
          <label for="buttonGreen">✅Captcha completed</label>
          <button
            class="buttonCaptchaYes"
            id="buttonGreen"
            on:click
            disabled={captchaComplete}
          ></button>
        </div>
        <div class="flex-containerCancelConfirm">
          <button on:click={() => dispatch('cancel')}>Cancel</button>
          <button
            on:click={() => dispatch('confirm')}
            disabled={confirmDisabled}>Confirm</button
          >
        </div>
        <div class="flex-required">
          <div class="requiredText">*Information required</div>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 1000vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    margin: auto;
    top: 70px;
    left: 305px;
    width: 60%;
    max-height: 100vh;
    background: white;
    border-radius: 50px;
    z-index: 100;
  }

  .container1 {
    display: flex;
    flex-wrap: wrap;
  }

  .flex-item1 {
    flex-basis: 33%;
  }

  .container2 {
    display: flex;
    flex-wrap: wrap;
  }

  .flex-item2 {
    flex-basis: 50%;
    margin-bottom: 10px;
  }

  header {
    font-size: 3rem;
    color: black;
  }

  p {
    font-size: 1rem;
    color: black;
  }

  h2 {
    font-size: 1.8rem;
    color: black;
    font-weight: 500;
  }

  label {
    color: black;
    font-size: 0.8rem;
    display: inline-block;
    width: 100px;
  }

  .labelDob {
    font-size: 0.79rem;
  }

  .buttonCaptcha {
    width: 50px;
    height: 50px;
    background-color: red;
  }

  .buttonCaptcha:disabled {
    width: 50px;
    height: 50px;
    background-color: red;
    cursor: default;
  }

  .loading {
    cursor: progress;
    width: 50px;
    height: 50px;
    background-color: yellow;
  }

  .buttonCaptchaYes {
    width: 50px;
    height: 50px;
    background-color: green;
    cursor: default;
  }

  .buttonCaptcha:focus {
    width: 50px;
    height: 50px;
    background-color: red;
    outline: none;
  }

  .buttonCaptchaYes:focus {
    width: 50px;
    height: 50px;
    background-color: green;
    outline: none;
  }

  .grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    margin-bottom: 20px;
  }

  .flex-containerCaptcha {
    display: flex;
    grid-column: 2;
    justify-content: center;
    align-items: center;
  }

  .flex-containerCancelConfirm {
    display: flex;
    grid-column: 3;
    justify-content: flex-end;
    align-items: center;
    margin-right: 40px;
    gap: 10px;
  }

  .requiredSymbol {
    color: red;
    display: inline-block;
  }

  .flex-required {
    display: flex;
    grid-column: 3;
    justify-content: flex-end;
    align-items: center;
    margin-right: 40px;
    margin-top: 10px;
  }

  .flex-infoText {
    display: flex;
    grid-column: 1;
    justify-content: center;
    align-items: center;
    margin-left: 40px;
    margin-top: 10px;
    color: black;
    border: 2.5px solid black;
  }

  .requiredText {
    color: red;
    font-size: 0.7rem;
  }
</style>
