<script>
  import { onMount } from 'svelte';

  let consoleMessages = [];

  function consoleLog(message, type) {
    consoleMessages = [...consoleMessages, { message, type }];
  }

  // 1. Alapértelmezett Promise
  const checkIngredients = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Minden alapanyag megvan');
    }, 1000);
  });

  checkIngredients.then(result => {
    consoleLog(`Alapértelmezett Promise: ${result}`, 'default');
  });

  // 2. Promise láncolás
  const startCooking = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Főzni szeretnék');
    }, 500);
  });

  startCooking
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('Megnézem megvan-e minden');
        }, 1000);
      });
    })
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('Elmentem a boltba');
        }, 2000);
      });
    })
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('Visszajöttem a boltból');
        }, 2000);
      });
    })
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('Elkezdtem főzni');
        }, 1000);
      });
    })
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('Elkészült az étel');
        }, 2000);
      });
    })
    .then(result => {
      consoleLog(`Promise láncolás: ${result}`, 'chain');
    });

  // 3. Promise.all
  const checkAndBuy = Promise.all([
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Minden alapanyag megvan');
      }, 1000);
    }),
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Elmentem a boltba és visszajöttem');
      }, 4000);
    })
  ]);

  checkAndBuy.then(results => {
    consoleLog(`Promise.all: ${results.join(' és ')}`, 'all');
  });

  // 4. Async/Await
  async function asyncCook() {
    consoleLog('Async/Await: Főzni szeretnék', 'async');

    const check = await new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Megnézem megvan-e minden');
      }, 1000);
    });

    consoleLog(`Async/Await: ${check}`, 'async');

    const buy = await new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Elmentem a boltba és visszajöttem');
      }, 4000);
    });

    consoleLog(`Async/Await: ${buy}`, 'async');

    const cook = await new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Elkészült az étel');
      }, 2000);
    });

    consoleLog(`Async/Await: ${cook}`, 'async');
  }

  onMount(() => {
    asyncCook();
  });
</script>

<main>
  <h1>Promise Bemutató</h1>
  <div class="console">
    {#each consoleMessages as { message, type }}
      <div class={type}>- {message}</div>
    {/each}
  </div>
</main>

<style>
  :root {
    --primary-color: #646cff;
    --secondary-color: #535bf2;
    --background-color: #242424;
    --text-color: rgba(255, 255, 255, 0.87);
    --card-background: #1a1a1a;
    --default-color: #ff6f61;
    --chain-color: #ffcc00;
    --all-color: #66ff66;
    --async-color: #66ccff;
  }

  body {
    margin: 0;
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    background-color: var(--background-color);
    color: var(--text-color);
  }

  main {
    padding: 2rem;
    text-align: center;
  }

  .console {
    background-color: var(--card-background);
    padding: 1rem;
    border-radius: 8px;
    text-align: left;
    max-height: 300px;
    overflow-y: auto;
  }

  .default {
    color: var(--default-color);
  }

  .chain {
    color: var(--chain-color);
  }

  .all {
    color: var(--all-color);
  }

  .async {
    color: var(--async-color);
  }

  h1 {
    color: var(--primary-color);
  }
</style>