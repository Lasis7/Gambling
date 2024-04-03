<script>
</script>

<div class="backdrop">
  <div class="modal">
    <header>Transfer money</header>
    <hr />

    <label for="firstName">First name</label>
    <input type="text" id="firstName" />

    <label for="2ndName">Second name</label>
    <input type="text" id="2ndName" />

    <label for="surname">Surname</label>
    <input type="text" id="surname" />

    <hr />
    <label for="age">Age</label>
    <input type="number" id="age" />

    <label for="DoB">Date of birth</label>
    <input type="text" id="DoB" />

    <label for="sex">Sex</label>
    <input type="text" id="sex" />
    <hr />
    <label for="iceCream">Favourite ice cream</label>
    <input type="text" id="iceCream" />

    <label for="DoB">Date of birth</label>
    <input type="text" id="DoB" />

    <label for="sex">Sex</label>
    <input type="text" id="sex" />
    <hr />
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

  header {
    font-size: 3rem;
    color: black;
  }

  label {
    color: black;
    font-size: 0.8rem;
    margin: 0;
  }
</style>
