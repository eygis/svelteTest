<script>
  //imports
  import { writable } from "svelte/store";
  import Timer from "./Timer.svelte";
  import Card from "./card.svelte";

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
    } else if (feelings.length === 2) {
      return `${feelings.slice(0, -1)} and ${feelings[feelings.length - 1]}`;
    } else {
      return `${feelings.slice(0, -1).join(", ")}, and ${
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

  //progress
  let progress = writable(0);
  let progresser = (percent) => {
    progress.set(percent);
  };

  //email binding
  let email = "";
  let message = "";
</script>

<main>
  <h1 class="big">Hello {currentUser}!</h1>
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
    <p>Please choose the time and at least one feeling.</p>
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

  <progress value={$progress} />
  <div>
    <button on:click={() => progresser(0)}>0%</button>
    <button on:click={() => progresser(0.25)}>25%</button>
    <button on:click={() => progresser(0.5)}>50%</button>
    <button on:click={() => progresser(0.75)}>75%</button>
    <button on:click={() => progresser(1)}>100%</button>
  </div>

  <Card>
    <span slot="name"> T. T. Chopper </span>
    <span slot="address"> The Ocean </span>
  </Card>

  <form
    id="form"
    action="mailto:{email}"
    method="post"
    enctype="multipart/form-data"
  >
    <div>
      <label for="email">E-mail Address: </label>
      <input type="email" id="emailBox" bind:value={email} />
      <label for="message">Message: </label>
      <textarea name="message" id="messageBox" bind:value={message} />
    </div>
    <button type="submit">Submit</button>
  </form>
  <div id="signup">
    <div id="pictureArea">
      <h1 id="title">Lo-cal Calzone Zone</h1>
    </div>
    <div id="formArea">
      <div id="formText">
        <p id="formp">
          There's fast food hamburgers, there's fast food Mexican, fast food
          Chinese, blah blah blah. Have you ever wondered why there's no fast
          food option for italian food?
        </p>
      </div>
      <div id="accountArea">
        <button id="createButton">Get More Information</button>
      </div>
    </div>
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  .big {
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

  progress {
    width: 70%;
    margin-top: 1.5em;
  }

  #form {
    border: solid black;
    width: 20%;
    margin: 1em auto;
  }

  #signup {
    display: flex;
    border: solid black;
    height: 80vh;
  }

  #pictureArea {
    width: 40%;
    border-right: solid black;
    background-image: url("/calzone.png");
    background-repeat: no-repeat;
    background-size: 100%;
  }

  #formArea {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    width: 60%;
  }

  #accountArea {
    text-align: left;
    height: 50%;
    background-color: #dfdfdf;
    z-index: -1;
  }

  #formp {
    margin: 7em 2em 0.5em 0.5em;
    width: 80%;
  }

  #formText {
    height: 50%;
    box-shadow: 0 10px 20px -10px black;
  }

  #createButton {
    padding: 1em 2em;
    width: 250px;
    color: white;
    background-color: green;
    border-radius: 1em;
    margin-left: 1.5em;
    margin-top: 2em;
  }

  #title {
    margin-top: 1.5em;
    background-color: rgba(255, 255, 255, 0.5);
    border-top: 1px solid black;
    border-bottom: 1px solid black;
    color: black;
  }
</style>
