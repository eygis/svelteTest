<script>
  //imports
  import { writable } from "svelte/store";
  import Timer from "./Timer.svelte";

  //basics, adding data;
  let name = prompt("What's your name?");
  name = name ? name : "world";
  let count = 0;
  let clickFunction = () => {
    count++;
  };
  let even = "";

  //logic, if;
  $: if (count % 2 == 0 && count > 0) {
    even = `${count} is an even number.`;
  } else {
    even = "";
  }
  let oneClickTest = () => {
    alert("If you click the button again, nothing will happen.");
  };

  //bindings
  let currentTime = "morning";
  let feelings = ["okay"];

  let times = ["morning", "afternoon", "night"];
  let choices = ["okay", "happy", "sad", "stressed", "angry"];

  let evaluator = (feelings) => {
    if (feelings.length === 1) {
      return feelings[0];
    } else {
      return `${feelings.slice(0, -1).join(", ")} and ${
        feelings[feelings.length - 1]
      }`;
    }
  };

  //lifecycle
  let counting = true;
  let seconds = 0;
  let timerClick = () => {
    counting = counting ? (counting = false) : (counting = true);
  };

  //stores
  let currentUser;
  let tempName = writable(name);
  tempName.subscribe((newName) => {
    currentUser = newName;
  });
  let updateName = (e) => {
    e.preventDefault();
    let submittedName = document.getElementById("submittedName").value;
    tempName.set(submittedName);
  };
</script>

<main>
  <h1>Hello {currentUser}!</h1>
  <button id="button" on:click={clickFunction}
    >You have clicked this button {count}
    {count == 1 ? "time" : "times"}.</button
  >
  <p>
    {even}
  </p>
  {#if count % 2 != 0 && count > 0}
    <p>
      {count} is an odd number.
    </p>
  {:else if count > 0}
    <p>&#8593;</p>
  {/if}

  <button on:click|once={oneClickTest}>
    You can click this button, and recieve an alert only once.
  </button>

  <h2>Time of Day:</h2>
  {#each times as time, i}
    <label>
      <input type="radio" bind:group={currentTime} name="times" value={time} />
      {time}
    </label>
  {/each}
  <h2>Your current mood:</h2>
  {#each choices as feeling}
    <label>
      <input
        type="checkbox"
        bind:group={feelings}
        name="feelings"
        value={feeling}
      />
      {feeling}
    </label>
  {/each}
  {#if feelings.length === 0}
    <p>Please the time and at least one feeling.</p>
  {:else}
    <p>
      It is {currentTime} and you feel {evaluator(feelings)}.
    </p>
  {/if}

  {#if counting}
    <Timer bind:seconds />
  {/if}
  <button id="timerStop" on:click={timerClick}
    >{counting
      ? "Please stop counting."
      : "Okay start counting again please."}</button
  >

  <h2>The Current User is:</h2>
  <p>{currentUser}</p>
  <form on:submit={updateName}>
    <input type="text" id="submittedName" /><button>Submit New User Name</button
    >
  </form>
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
