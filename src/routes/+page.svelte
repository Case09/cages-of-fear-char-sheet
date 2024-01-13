<script lang="ts">
  import {onMount} from 'svelte';

  let name = '', 
    clas = '', 
    damageBase = 0, 
    damageMod = 1, 
    intellectBase = 0, 
    intellectMod = 0, 
    healthBase = 4, 
    healthMod = 0, 
    currentHealth = 4,
    currentMonsterHealth = 0,
    exp = 0,
    items = 0,
    gold = 0, 
    potions = 1, 
    lockpicks = 0, 
    spells = 0,
    localStorageKey = 'cages-of-fear-char-sheet';

    onMount(() => {
      const savedValues = localStorage.getItem(localStorageKey);
      if (savedValues) {
        const parsed = JSON.parse(savedValues);
        ({
          name, 
          clas, 
          damageBase, 
          damageMod, 
          intellectBase,
          intellectMod,
          healthBase,
          healthMod,
          currentHealth,
          currentMonsterHealth,
          exp,
          items,
          gold,
          potions,
          lockpicks,
          spells
       } = parsed);
      }
    });

    function saveChanges() {
      localStorage.setItem(localStorageKey, JSON.stringify({
        name, 
        clas, 
        damageBase, 
        damageMod, 
        intellectBase,
        intellectMod,
        healthBase,
        healthMod,
        currentHealth,
        currentMonsterHealth,
        exp,
        items,
        gold,
        potions,
        lockpicks,
        spells
      }));
      alert('Changes saved!');
    }

    function resetSavedChanges() {
      localStorage.clear();
      alert('Cleared!');
    }
</script>

<div class="h-screen flex flex-col items-center justify-center">
  <div class="char-sheet p-4 bg-yellow-50">
    <h1 class="text-xl font-bold tracking-wide">Cages of Fear character sheet</h1>
    <div class="flex gap-20 items-center">
      <div class="mt-2 space-y-2">
        <label for="name" class="flex flex-col">
          <span class="text-xs pb-1 text-orange-600">Name</span>
          <input class="h-8 w-[250px] border border-black p-2" type="text" bind:value={name} name="name">
        </label>
        <label for="class" class="flex flex-col">
          <span class="text-xs pb-1 text-orange-600">Class <span class="text-black">(Footman, Magister or Brigand)</span></span>
          <input class="h-8 w-1/2 border border-black p-2" type="text" bind:value={clas} name="class">
        </label>
      </div>
      <div class="flex gap-4">
        <label for="name" class="flex flex-col justify-center">
          <span class="text-xs pb-1 text-orange-600">Current health</span>
          <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" bind:value={currentHealth} name="name">
        </label>
        <label for="class" class="flex flex-col justify-center">
          <span class="text-xs pb-1 text-orange-600">Current monster health</span>
          <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" bind:value={currentMonsterHealth} name="class">
        </label>
      </div>
      <div class="justify-self-start flex gap-2">
        <button on:click={saveChanges} class="bg-orange-500 text-white px-2 py-1 hover:bg-orange-800">Save Changes</button>
        <button on:click={resetSavedChanges} class="bg-orange-500 text-white px-2 py-1 hover:bg-orange-800">Reset saved changes</button>
      </div>
    </div>
    <div class="mt-6 border-t border-gray-400"></div>

    <div class="flex gap-4">
      <div class="mt-6 p-2 w-80 border border-black">
        <span class="text-xs"><span class="text-orange-600">Damage</span> (base + modifier)</span>
        <label for="damage-base" class="flex items-center">
          <input class="h-8 w-[60px] border border-black p-2" min="0" type="number" bind:value={damageBase} name="damage-base">
          <input class="h-8 w-1/2 border border-l-0 border-spacing-0 border-black p-2" min="0" type="number" bind:value={damageMod} name="damage-mod">
          <span class="pl-2 text-orange-600">Total: {damageBase + damageMod}</span>
        </label>

        <span class="text-xs"><span class="text-orange-600">Intellect</span> (base + modifier)</span>
        <label for="intellect-base" class="flex items-center">
          <input class="h-8 w-[60px] border border-black p-2" min="0" type="number" bind:value={intellectBase} name="intellect-base">
          <input class="h-8 w-1/2 border border-l-0 border-spacing-0 border-black p-2" min="0" type="number" bind:value={intellectMod} name="intellect-mod">
          <span class="pl-2 text-orange-600">Total: {intellectBase + intellectMod}</span>
        </label>

        <span class="text-xs"><span class="text-orange-600">Health</span> (base + modifier)</span>
        <label for="health-base" class="flex items-center">
          <input class="h-8 w-[60px] border border-black p-2" min="0" type="number" bind:value={healthBase} name="health-base">
          <input class="h-8 w-1/2 border border-l-0 border-spacing-0 border-black p-2" min="0" type="number" bind:value={healthMod} name="health-mod">
          <span class="pl-2 text-orange-600">Total: {healthBase + healthMod}</span>
        </label>
      </div>
      <div class="mt-4">
        <span class="text-xs text-orange-600">Experience</span>
        <div class="text-xs flex gap-1">At each level<div class="w-[20px] h-[20px] border-4 border-black"></div> choose either: +1 base damage, +1 base intellect or +2 base health</div>
        <div class="text-xs flex gap-1">A Magister also gains one spell per <div class="w-[20px] h-[20px] border border-l-0 border-r-0 border-white relative with-dot before:!top-[-6px]"></div></div>
        <div class="text-xs flex gap-1">A Brigand also gains one lockpick per <div class="w-[20px] h-[20px] border-4 border-black"></div></div>

        <div class="flex gap-4 mt-4 items-end">
          <div>
            <div class="exp-grid flex">
              <div class="w-[20px] h-[20px] border border-r-0 border-black" class:bg-orange-200={exp >= 1}></div>
              <div class="w-[20px] h-[20px] border-4 border-black relative with-dot-bordered" class:bg-orange-200={exp >= 2}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black" class:bg-orange-200={exp >= 3}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-r-0 border-black relative with-dot" class:bg-orange-200={exp >= 4}></div>
              <div class="w-[20px] h-[20px] border-4 border-black relative" class:bg-orange-200={exp >= 5}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black" class:bg-orange-200={exp >= 6}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black relative with-dot" class:bg-orange-200={exp >= 7}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-r-0 border-black" class:bg-orange-200={exp >= 8}></div>
              <div class="w-[20px] h-[20px] border-4 border-black" class:bg-orange-200={exp >= 9}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black relative with-dot" class:bg-orange-200={exp >= 10}></div>
            </div>
            <div class="exp-grid flex pt-[1px]">
              <div class="w-[20px] h-[20px] border border-r-0 border-black" class:bg-orange-200={exp >= 11}></div>
              <div class="w-[20px] h-[20px] border border-black" class:bg-orange-200={exp >= 12}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-r-0 border-black relative with-dot" class:bg-orange-200={exp >= 13}></div>
              <div class="w-[20px] h-[20px] border-4 border-black" class:bg-orange-200={exp >= 14}></div>
              <div class="w-[20px] h-[20px] border border-black" class:bg-orange-200={exp >= 15}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black relative with-dot" class:bg-orange-200={exp >= 16}></div>
              <div class="w-[20px] h-[20px] border border-black border-l-0" class:bg-orange-200={exp >= 17}></div>
              <div class="w-[20px] h-[20px] border border-black border-l-0" class:bg-orange-200={exp >= 18}></div>
              <div class="w-[20px] h-[20px] border border-l-0 border-black relative with-dot" class:bg-orange-200={exp >= 19}></div>
              <div class="w-[20px] h-[20px] border-4 border-black" class:bg-orange-200={exp === 20}></div>
            </div>
          </div>

          <label for="name" class="flex flex-col justify-center">
            <span class="text-xs text-orange-600">Experience</span>
            <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="20" bind:value={exp} name="exp">
          </label>
        </div>

        <div class="mt-4 space-y-1">
          <span class="text-xs text-orange-600">Abilities</span>
          <div class="text-xs">A Magister can re-roll potions</div>
          <div class="text-xs">A Brigand can re-roll chests. Any failed attempt to flee can also be re-rolled once.</div>
        </div>
      </div>
    </div>

    <div class="flex flex-col w-full gap-2 mt-4">
      <label for="name" class="flex flex-col justify-center">
        <span class="text-xs pb-1 text-orange-600">Gold</span>
        <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="12" bind:value={gold} name="gold">
      </label>
      <label for="class" class="flex flex-col justify-center">
        <span class="text-xs pb-1 text-orange-600">Potions <span class="text-black">(Consume to restore health by d6 + intellect)</span></span>
        <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="6" bind:value={potions} name="potions">
      </label>
      <label for="class" class="flex flex-col justify-center">
        <span class="text-xs pb-1 text-orange-600">Lockpicks <span class="text-black">(Consume to open a locked chest)</span></span>
        <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="6" bind:value={lockpicks} name="lockpicks">
      </label>
      <label for="class" class="flex flex-col justify-center">
        <span class="text-xs pb-1 text-orange-600">Spells <span class="text-black">(Consume to damage an enemy by d6 + intellect)</span></span>
        <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="6" bind:value={spells} name="spells">
      </label>
      <label for="class" class="flex flex-col justify-center">
        <span class="text-xs pb-1 text-orange-600">Pack <span class="text-black">(You may sell items to a merchant for 1 gold each)</span></span>
        <input class="h-8 w-[60px] border border-black p-2" type="number" min="0" max="2" bind:value={items} name="items">
      </label>
    </div>
  </div>
  <span class="text-xs">Check out Cages of Fear website: <a class="text-blue-600 hover:underline" href="https://www.highfellgames.com/cagesoffear" target="_blank">https://www.highfellgames.com/cagesoffear</a></span>
</div>

<style>
  .char-sheet {
    color: black;
    border: 1px solid black;
    min-width: 700px;
    min-height: 500px;
  }

  .with-dot-bordered:before {
    content: '.';
    font-weight: bold;
    font-size: 20px;
    position: absolute;
    top: -14px;
    left: 3px;
  }
  
  .with-dot:before {
    content: '.';
    font-weight: bold;
    font-size: 20px;
    position: absolute;
    top: -11px;
    left: 6px;
  }
</style>
