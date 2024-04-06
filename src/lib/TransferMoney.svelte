<script>
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

    <div class="container2">
      <div class="flex-item2">
        <label for="iceCream">Favourite ice cream</label>
        <input type="text" id="iceCream" />
      </div>

      <div class="flex-item2">
        <label for="DoB">Did you shower today?</label>
        <input type="text" id="shower" />
      </div>

      <div class="flex-item2">
        <label for="alphabet">The last alphabet</label>
        <input type="text" id="alphabet" />
      </div>

      <div class="flex-item2">
        <label for="alphabet">The last alphabet</label>
        <input type="text" id="alphabet" />
      </div>
    </div>
    <hr />

    <div class="captcha">
      {#if captchaComplete}
        <label for="button1">Complete the captcha</label>
        <button
          class="buttonCaptchaYes"
          id="button1"
          on:click
          disabled={captchaComplete}
        ></button>
      {:else}
        <label for="button1">Complete the captcha</label>
        <button class="buttonCaptcha" id="button1" on:click></button>
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
    top: 200px;
    left: 305px;
    width: 60%;
    max-height: 80vh;
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
