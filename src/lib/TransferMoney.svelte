<script>
  import { createEventDispatcher } from 'svelte';

  export let captchaLoading;
  export let captchaComplete;

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

  $: confirmDisabled =
    firstName.length < 1 ||
    secondName.length < 1 ||
    surname.length < 1 ||
    age < 0 ||
    age === null ||
    dateOfBirthYear === null ||
    gmail.length < 1 ||
    iceCream.length < 1 ||
    shower.length < 1 ||
    finger < 1 ||
    waterTaste.length < 1 ||
    !captchaComplete;

  const dispatch = createEventDispatcher();
</script>

<div class="backdrop">
  <div class="modal">
    <header>Transfer money</header>
    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="firstName">First name</label>
        <input type="text" id="firstName" bind:value={firstName} />
      </div>

      <div class="flex-item1">
        <label for="2ndName">Second name</label>
        <input type="text" id="2ndName" bind:value={secondName} />
      </div>

      <div class="flex-item1">
        <label for="surname">Surname</label>
        <input type="text" id="surname" bind:value={surname} />
      </div>
    </div>

    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="age">Age</label>
        <input type="number" id="age" bind:value={age} />
      </div>

      <div class="flex-item1">
        <label for="DoBY">Date birth &#40;Year&#41;</label>
        <input type="number" id="DoBY" bind:value={dateOfBirthYear} />
      </div>

      <div class="flex-item1">
        <label for="gmail">Gmail</label>
        <input type="text" id="gmail" bind:value={gmail} />
      </div>
    </div>

    <hr />

    <h2>Security questions</h2>

    <div class="container2">
      <div class="flex-item2">
        <label for="iceCream">Favourite ice cream?</label>
        <input type="text" id="iceCream" bind:value={iceCream} />
      </div>

      <div class="flex-item2">
        <label for="DoB">Did you shower today?</label>
        <input type="text" id="shower" bind:value={shower} />
      </div>

      <div class="flex-item2">
        <label for="finger">The number of fingers in hand?</label>
        <input type="number" id="finger" bind:value={finger} />
      </div>

      <div class="flex-item2">
        <label for="water">The taste of water?</label>
        <input type="text" id="water" bind:value={waterTaste} />
      </div>
    </div>
    <hr />

    <div class="grid-container">
      {#if !captchaComplete && !captchaLoading}
        <div class="flex-containerCaptcha">
          <label for="buttonRed">Complete the captcha</label>
          <button
            class="buttonCaptcha"
            id="buttonRed"
            on:click={() => dispatch('captcha')}
          ></button>
        </div>
        <div class="flex-containerCancelConfirm">
          <button on:click={() => dispatch('cancel')}>Cancel</button>
          <button disabled={confirmDisabled}>Confirm</button>
        </div>
      {:else if !captchaComplete && captchaLoading}
        <div class="flex-containerCaptcha">
          <label for="buttonYellow">Loading...</label>
          <button class="loading" id="buttonYellow" disabled={captchaLoading}
          ></button>
        </div>
        <div class="flex-containerCancelConfirm">
          <button on:click={() => dispatch('cancel')}>Cancel</button>
          <button disabled={confirmDisabled}>Confirm</button>
        </div>
      {:else if captchaComplete && !captchaLoading}
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
    top: 150px;
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

  .buttonCaptcha {
    width: 50px;
    height: 50px;
    background-color: red;
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
</style>
