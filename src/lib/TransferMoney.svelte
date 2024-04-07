<script>
  export let captchaLoading;
  export let captchaComplete;
</script>

<div class="backdrop">
  <div class="modal">
    <header>Transfer money</header>
    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="firstName">First name</label>
        <input type="text" id="firstName" />
      </div>

      <div class="flex-item1">
        <label for="2ndName">Second name</label>
        <input type="text" id="2ndName" />
      </div>

      <div class="flex-item1">
        <label for="surname">Surname</label>
        <input type="text" id="surname" />
      </div>
    </div>

    <hr />

    <div class="container1">
      <div class="flex-item1">
        <label for="age">Age</label>
        <input type="number" id="age" />
      </div>

      <div class="flex-item1">
        <label for="DoB">Date of birth</label>
        <input type="text" id="DoB" />
      </div>

      <div class="flex-item1">
        <label for="sex">Gmail</label>
        <input type="text" id="gmail" />
      </div>
    </div>

    <hr />

    <h2>Security questions</h2>

    <div class="container2">
      <div class="flex-item2">
        <label for="iceCream">Favourite ice cream?</label>
        <input type="text" id="iceCream" />
      </div>

      <div class="flex-item2">
        <label for="DoB">Did you shower today?</label>
        <input type="text" id="shower" />
      </div>

      <div class="flex-item2">
        <label for="alphabet">The number of fingers in hand?</label>
        <input type="text" id="alphabet" />
      </div>

      <div class="flex-item2">
        <label for="alphabet">The taste of water?</label>
        <input type="text" id="alphabet" />
      </div>
    </div>
    <hr />

    <div class="captcha">
      {#if !captchaComplete && !captchaLoading}
        <label for="buttonRed">Complete the captcha</label>
        <button class="buttonCaptcha" id="buttonRed" on:click></button>
      {:else if !captchaComplete && captchaLoading}
        <label for="buttonYellow">Loading...</label>
        <button
          class="loading"
          id="buttonYellow"
          on:click
          disabled={captchaLoading}
        ></button>
      {:else if captchaComplete && !captchaLoading}
        <label for="buttonGreen">✅Captcha completed</label>
        <button
          class="buttonCaptchaYes"
          id="buttonGreen"
          on:click
          disabled={captchaComplete}
        ></button>
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

  .captcha {
    display: flex;
    justify-content: flex-start;
    margin-left: 300px;
    margin-bottom: 20px;
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
    background-color: red;
    outline: none;
  }
</style>
