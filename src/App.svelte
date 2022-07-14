<script>
  import Hangman from "./lib/Hangman.svelte"; 
  import words from './words'

  const word = [...words[Math.floor(Math.random() * words.length)]];
  let holder = word.map(() => "-");

  let playable = true

  let lives = 11;

  const handleInput = (e) => {
    const guess = e.target.value.toLowerCase() 

    if (word.includes(guess)) {
      const matches = word.reduce(function (prev, curr, i) {
        if (curr === guess) prev.push(i);
        return prev;
      }, []);

      for (let i = 0; i < matches.length; i++) {
        holder[matches[i]] = guess;
      }

      if (holder.filter((letter) => letter === "-").length === 0) {
        alert(`Gratulujem! Vyhrali ste... Slovo je ${word.join("")}`);
        playable = false;
      }

    } else {
      lives -= 1;

      if (lives === 0) {
        alert(`„Prehral si... Slovo je ${word.join("")}`);
        playable = false;
      }
    }

    e.target.value = "";
  };
</script>

<main class='game'>
<Hangman lives={lives}/>

  <section class='holder'>
    {holder.join(" ")}
  </section>

  <input type="text" on:input={handleInput} disabled='{!playable}' placeholder="Stlačením tu začnite hádať písmená" class='guess-input' autocomplete="off"/>
  <button on:click={()=>location.reload()} class="reset">Resetovať</button>
</main>

<style>
  .game {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    width: clamp(10rem, 90vw, 30rem);
  }

  .guess-input {
    padding: 0.5rem 1rem;
    border-radius: 25rem;
    border: 2px solid #9AEBA3;
    outline: none;
    margin: 1rem 0;
    transition: .3s;
    width: 100%;
  }

  .guess-input:focus {
    border-color: #45C4B0;
  }

  .reset {
    padding: 0.5rem 1rem;
    border-radius: 25rem;
    background: #45C4B0;
    transition: .3s;
    border: none;
    width: 100%;
  }

  .reset:focus, .reset:hover {
    background-color: #9AEBA3;
  }
</style>
