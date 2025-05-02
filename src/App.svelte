<script lang="ts">
  import { SvelteSet } from 'svelte/reactivity';
  import Button from './Button.svelte';
  import SolvedCategory from './SolvedCategory.svelte';
  import Toast from './Toast.svelte';

  let groups = [
    { title: "Zařídím v internetovém bankonvnictví", cards: ["převod", "výběr", "zůstatek", "vklad"] },
    { title: "Části jízdního kola", cards: ["kazeta", "vidlice", "duše", "řetěz"] },
    { title: "Analogová záznamová média", cards: ["film", "páska", "fonografický válec", "vinyl"] },
    { title: "Samo-", cards: ["obsluha", "správa", "chvála", "pal"] },
  ];
  let selectedCards = new SvelteSet<string>();
  let mistakes = $state(0);
  let solvedGroups = $state([]);
  let message = $state("");

  let cards = $derived(
    groups.filter(x => !solvedGroups.map(x => x.title).includes(x.title)).map(group => group.cards).flat().sort(() => Math.random() - 0.5).sort(() => Math.random() - 0.5),
  );

  let triedAlready = $state<string[][]>([]);

  function selectCard(word: string) {
    if (selectedCards.has(word)) {
      selectedCards.delete(word);
    } else if (selectedCards.size < 4) {
      selectedCards.add(word);
    }
  }

  function checkSolved() {
    if (selectedCards.size === 4) {
      const selectedArray = Array.from(selectedCards);
      const groupIndex = groups.findIndex(group => group.cards.every(card => selectedArray.includes(card)));
      if (groupIndex !== -1) {
        solvedGroups.push({...groups[groupIndex], index: groupIndex}); 

        if (groups.length === solvedGroups.length) {
          message = "Vyhráli jste!";
          setTimeout(() => {
            message = "";
          }, 2000);
        }

        selectedCards.clear();
      } else {
        if (triedAlready.some(tried => tried.every(card => selectedArray.includes(card)))) {
          message = "Tato slova už jste zkoušeli!";
          setTimeout(() => {
            message = "";
          }, 2000);
          return;
        }

        // if the selection differs from any group by 1 card, show message
        const isSimilar = groups.some(group => {
          const diff = group.cards.filter(card => !selectedArray.includes(card));
          return diff.length === 1 && selectedArray.some(card => group.cards.includes(card));
        });
        if (isSimilar) {
          message = "Jedno slovo je špatně!";
          setTimeout(() => {
            message = "";
          }, 2000);
        } else {
          // if the selection is completely wrong, show message
          message = "Nesprávně vybraná slova";
          setTimeout(() => {
            message = "";
          }, 2000);
        }
        mistakes += 1;

        triedAlready.push(selectedArray);
      }

      if (mistakes >= 4) {
        message = "Prohráli jste!";
        solvedGroups = groups.map((group, index) => ({ ...group, index }));
        setTimeout(() => {
          message = "";
        }, 2000);
      }
    }
  }

  function getRows(cards: string[]) {
    const rows = [];
    for (let i = 0; i < cards.length; i += 4) {
      rows.push(cards.slice(i, i + 4));
    }
    return rows;
  }


</script>

<style>
  .grid {
    display: flex;
    margin: auto;
    gap: 10px;
    flex-direction: column;
  }

  .row {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 10px;
    align-items: center;
  }

  button {
    margin-top: 20px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding: 10px 20px;
    cursor: pointer;
    border: none;
  }

  .mistakes {
    display: flex;
    flex-direction: row;
    margin: auto;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }

  .health-point {
    width: 10px;
    height: 10px;
    background-color: #383838;
    border-radius: 50%;
    margin: 5px;
  }
</style>

<h4>Tvořte související skupiny po čtyřech slovech</h4>

<div>
  {#each solvedGroups as group}
    <SolvedCategory {...group} />
  {/each}

  {#if mistakes < 4}
    <div class="grid">
      {#each getRows(cards) as row}
        <div class="row">
          {#each row as word}
            <Button
              word={word}
              select={selectCard}
              selected={selectedCards.has(word)}
            />
          {/each}
        </div>
      {/each}
    </div>
  {/if}

  <div class="mistakes">
  {#each { length: 4 - mistakes }}
    <div class="health-point">
    </div>
  {/each}
  </div>
</div>

<Toast
  {message}
/>

{#if solvedGroups.length !== 4}
  <button 
    onclick={checkSolved}
    disabled={selectedCards.size !== 4}
  >Zkontrolovat</button>
{/if}
