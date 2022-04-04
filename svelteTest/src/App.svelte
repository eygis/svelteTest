<script>
  let name = prompt("What's your name?");
  name = name ? name : "world";
  let count = 0;
  let clickFunction = () => {
    count++;
  };
</script>

<main>
  <h1>Hello {name}!</h1>
  <button id="button" on:click={clickFunction}
    >You have clicked this button {count}
    {count == 1 ? "time" : "times"}.</button
  >
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
