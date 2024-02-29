<script lang="ts">
    import { onMount } from 'svelte';
    let items: any[] = [];
    let showToast = false;
    let toastMessage = "";
    let toastTimeout: number;
    let searchItem: string = '';
    let filteredItems: any[] = [];
    
    onMount(() => {
      getItems();
    });
  
    function Toast(message: string, shownFor: number) {
      toastMessage = message;
      toastTimeout = shownFor * 1000;
      showToast = true;
      setTimeout(() => {
        showToast = false;
      }, 1000);
    }
  
    async function getItems() {
      try {
        const res = await fetch("https://api.hypixel.net/v2/resources/skyblock/items", {
          method: "GET",
        });
        if (!res.ok) {
          Toast(`Error ${res.status}`, 5);
        } else {
          const data = await res.json();
          items = data.items;
          Toast(`Items loaded`, 5);
        }
      } catch (error) {
        console.error("Error fetching items:", error);
      }
    }
  
    function filterItems(event: any) {
      const value = event.target.value.toLowerCase();
      if (value.trim() !== '') {
        filteredItems = items.filter(item => item.name.toLowerCase().includes(value)).slice(0, 5); // Limit to the first 5 items
      } else {
        filteredItems = [];
      }
    }
  
    function selectItem(item: any) {
      searchItem = item.name;
      filteredItems = []; // Hide dropdown after selection
    }
  </script>
  
  <div class="relative">
    <div class="navbar bg-base-300">
      <div class="flex-1">
        <a class="btn btn-ghost text-xl">Skyblock invests</a>
      </div>
      <div class="flex-none gap-2">
        <div class="relative" data-te-input-wrapper-init id="displayValue">
          <input
            type="text"
            class="input-bordered peer block min-h-[auto] w-full rounded border-0 bg-transparent px-3 py-[0.32rem] leading-[1.6] outline-none transition-all duration-200 ease-linear placeholder:opacity-0 focus:placeholder:opacity-100 peer-focus:text-primary motion-reduce:transition-none dark:text-neutral-200 dark:placeholder:text-neutral-200 dark:peer-focus:text-primary"
            id="exampleFormControlInput2"
            bind:value={searchItem}
            on:input={filterItems}
          />
          <label for="exampleFormControlInput2"
            class="pointer-events-none absolute left-3 top-0 mb-0 max-w-[80%] origin-[0_0] truncate pt-[0.37rem] leading-[1.6] text-neutral-500 transition-all duration-200 ease-out peer-focus:-translate-y-[0.9rem] peer-focus:scale-[0.8] peer-focus:text-primary peer-data-[te-input-focused]:-translate-y-[0.9rem] peer-data-[te-input-state-active]:-translate-y-[0.9rem] peer-data-[te-input-focused]:scale-[0.8] peer-data-[te-input-state-active]:scale-[0.8] motion-reduce:transition-none dark:text-neutral-200 dark:peer-focus:text-primary">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor"
              class="w-4 h-4 opacity-70 inline-block align-middle mr-1">
              <path fill-rule="evenodd"
                d="M9.965 11.026a5 5 0 1 1 1.06-1.06l2.755 2.754a.75.75 0 1 1-1.06 1.06l-2.755-2.754ZM10.5 7a3.5 3.5 0 1 1-7 0 3.5 3.5 0 0 1 7 0Z"
                clip-rule="evenodd" />
            </svg>
            Search
          </label>
          {#if filteredItems.length > 0}
            <ul class="dropdown-content absolute left-0 mt-1 bg-white shadow-lg rounded-md w-48">
              {#each filteredItems as item}
                <li>
                  <button class="w-full text-left px-2 py-1 hover:bg-gray-100" on:click={() => selectItem(item)}>{item.name}</button>
                </li>
              {/each}
            </ul>
          {/if}
        </div>
      </div>
    </div>
    <button class="btn btn-active btn-secondary" on:click={getItems}>Get Items</button>
    {#if showToast}
      <div class="toast">
        <div class="alert alert-info">
          <span>{toastMessage}</span>
        </div>
      </div>
    {/if}
  </div>
  