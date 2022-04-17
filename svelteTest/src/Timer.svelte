<script>
  import { onDestroy } from "svelte";

  export let seconds;

  let timer = () => {
    const interval = setInterval(() => seconds++, 1000);
    onDestroy(() => {
      clearInterval(interval);
    });
  };
  timer();
</script>

<h1 class="big" id="timer">
  This page has been open for {seconds}
  {seconds === 1 ? "second" : "seconds"}.
</h1>

<style>
  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  #timer {
    color: blue;
    font-size: 3em;
  }
</style>
