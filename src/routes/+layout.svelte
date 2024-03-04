<script lang=ts>
    import { onMount } from 'svelte';
    import { Line } from 'svelte-chartjs';
    import link from '../../static/link.svg'
    import {Chart as ChartJS, Title,Tooltip,Legend,LineElement,LinearScale,PointElement,CategoryScale,} from 'chart.js';
    ChartJS.register(Title,Tooltip,Legend,LineElement,LinearScale,PointElement,CategoryScale);
    let items: any[] = [];
    let showToast = false;
    let toastMessage = "";
    let toastTimeout: number;
    let searchItem: string = '';
    let filteredItems: any[] = [];
    let theme: any = "dark";
    let selectedItem:any;
    let selectedItemData:any;
    let itemAuctionData:any;
    let itemBazaarDataWeek:any;
    let validBazaarItems:any[];
    const today = new Date(); // Get today's date
    let last30DaysData:[] = []; // Create lists to store the dates and data for the last 30 days
    let last30DaysDates:[] = [];
    
    onMount(() => {
      getItems();
      getValidBazaarItems();
    });
    //--------------------------------------------------------------------------------
    //                   Utility functions
    //--------------------------------------------------------------------------------
    
    function Toast(message: string, shownFor: number) {
      toastMessage = message;
      toastTimeout = shownFor * 1000;
      showToast = true;
      setTimeout(() => {
        showToast = false;
      }, 1000);
    }
    
    function themeLight() {
    if (theme !== "light") {
      theme = "light";
      localStorage.setItem("theme", "light");
    } else {
      theme = "dark";
      localStorage.setItem("theme", "dark");
    }
    }
    
    //--------------------------------------------------------------------------------
    //                   API functions
    //--------------------------------------------------------------------------------
    
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

    async function getValidBazaarItems() {
      try {
        const res = await fetch(`https://sky.coflnet.com/api/items/bazaar/tags`, {
          method: "GET",
        });
        if (!res.ok) {
          console.log(`Bazaar Items not found ${res.status}`);     
        } else {
          console.log(`Bazaar Items successfully fetched`);
          const data = await res.json();
          validBazaarItems = data.map(item => item);
        }
      } catch (error) {
        console.error("Error fetching items:", error);
      }
    }

    async function getItemHistoryData(item:any) {
      if (validBazaarItems.includes(item.id)) {
      try {
        const res = await fetch(`https://sky.coflnet.com/api/bazaar/${item.id}/history/week`, {
          method: "GET",
        });
        if (!res.ok) {
          console.log(`No Bazaardata found for ${item.name} with id ${item.id}`);    
        } else {
          console.log(`Bazaardata found for ${item.name} with id ${item.id}`);
          const data = await res.json();
          itemBazaarDataWeek = await data.items;
          console.log(data, "test");
        }
      } catch (error) {
        console.error("Error fetching items:", error);
      }
    } else {
      try {
        const res = await fetch(`https://sky.coflnet.com/api/auctions/tag/${item.id}/sold?page=1&pageSize=1000`, {
          method: "GET",
        });
        if (!res.ok) {
          console.log(`No Auctiondata found for ${item.name} with id ${item.id}`);    
        } else {
          console.log(`Auctiondata found for ${item.name} with id ${item.id}`);
          const data = await res.json();
          itemAuctionData = data.items;
        }
      } catch (error) {
        console.error("Error fetching items:", error);
      }
    }
    }
    
    //--------------------------------------------------------------------------------
    //                   Itemfilter functions
    //--------------------------------------------------------------------------------
    
    function filterItems(event: any) {
      const value = event.target.value.toLowerCase().replace(/\s/g, '');
      if (value.trim() !== '') {
        filteredItems = items.filter(item => 
          item.name.toLowerCase().replace(/\s/g, '').includes(value) || 
          item.id.toString().toLowerCase().replace(/\s/g, '').includes(value)
        ).slice(0, 5); // Limit to the first 5 items
      } else {
        filteredItems = [];
      }
    }
    
    //-----------------------------------------------------------------------------------------
    //                           Chart info
    //-----------------------------------------------------------------------------------------
    function selectItem(item: any) {
      last30DaysData = []; // Clear the array before populating it with new data
      last30DaysDates = [];
      selectedItem = item;
      for (let i = 0; i < 30; i++) {
    // Calculate the date i days ago
    const date = new Date(today);
    date.setDate(today.getDate() - i);
    const randomData = Math.floor(Math.random() * 101);
    // Format the date as mm/dd/yyyy
    const formattedDate = `${(date.getMonth() + 1).toString().padStart(2, '0')}/${date.getDate().toString().padStart(2, '0')}/${date.getFullYear()}`;
    last30DaysData.push(randomData);
    last30DaysDates.unshift(formattedDate); // Add to the beginning of the array
}

console.log("Dates:", last30DaysDates);
console.log("Data:", last30DaysData);

        selectedItemData = {
  labels: last30DaysDates,
  datasets: [
    {
      label: 'Median Price',
      fill: true,
      lineTension: 0.1,
      backgroundColor: 'rgba(225, 204,230, .3)',
      borderColor: 'rgb(239, 159, 118)',
      borderCapStyle: 'butt',
      borderDash: [],
      borderDashOffset: 0.0,
      borderJoinStyle: 'miter',
      pointBorderColor: 'rgb(239, 159, 118)',
      pointBackgroundColor: 'rgb(255, 255, 255)',
      pointBorderWidth: 1,
      pointHoverRadius: 5,
      pointHoverBackgroundColor: 'rgb(255, 255, 255)',
      pointHoverBorderColor: 'rgb(244, 184, 228)',
      pointHoverBorderWidth: 2,
      pointRadius: 1,
      pointHitRadius: 10,
      data: last30DaysData,
    },
    {
      label: 'Higher Price',
      fill: true,
      lineTension: 0.1,
      backgroundColor: 'rgba(225, 204,230, .3)',
      borderColor: 'rgb(231, 130, 132)',
      borderCapStyle: 'butt',
      borderDash: [],
      borderDashOffset: 0.0,
      borderJoinStyle: 'miter',
      pointBorderColor: 'rgb(231, 130, 132)',
      pointBackgroundColor: 'rgb(255, 255, 255)',
      pointBorderWidth: 1,
      pointHoverRadius: 5,
      pointHoverBackgroundColor: 'rgb(255, 255, 255)',
      pointHoverBorderColor: 'rgb(244, 184, 228)',
      pointHoverBorderWidth: 2,
      pointRadius: 1,
      pointHitRadius: 10,
      data: last30DaysData,
    },
    {
      label: 'Lower Price',
      fill: true,
      lineTension: 0.1,
      backgroundColor: 'rgba(225, 204,230, .3)',
      borderColor: 'rgb(166, 209, 137)',
      borderCapStyle: 'butt',
      borderDash: [],
      borderDashOffset: 0.0,
      borderJoinStyle: 'miter',
      pointBorderColor: 'rgb(166, 209, 137)',
      pointBackgroundColor: 'rgb(255, 255, 255)',
      pointBorderWidth: 1,
      pointHoverRadius: 5,
      pointHoverBackgroundColor: 'rgb(255, 255, 255)',
      pointHoverBorderColor: 'rgb(244, 184, 228)',
      pointHoverBorderWidth: 2,
      pointRadius: 1,
      pointHitRadius: 10,
      data: last30DaysData,
    },
  ],
};
      searchItem = item.name;
      filteredItems = []; // Hide dropdown after selection
    }
    
    function formatName(name:string) {
        return name.replace(/(?<=[a-z])(?=[A-Z])/g, ' ');
      }
    //--------------------------------------------------------------------------------
    //                   Modal functions
    //--------------------------------------------------------------------------------
    function openModal(modal:any) {
        getItemHistoryData(selectedItem);
    // This code will only execute in the browser environment
    document.getElementById(modal).showModal();

    }
    //--------------------------------------------------------------------------------
    </script>
    <html lang="en" data-theme={theme}>
    <div class="navbar">
      <div class="flex-1">
        <a class="btn btn-ghost text-xl">Skyblock Invest</a>
        <label class="cursor-pointer grid place-items-center">
            <input type="checkbox" value="synthwave" on:click={()=> themeLight()} class="toggle theme-controller bg-base-content row-start-1 col-start-1 col-span-2"/>
            <svg class="col-start-1 row-start-1 stroke-base-100 fill-base-100" xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path></svg>
            <svg class="col-start-2 row-start-1 stroke-base-100 fill-base-100" xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="5"/><path d="M12 1v2M12 21v2M4.2 4.2l1.4 1.4M18.4 18.4l1.4 1.4M1 12h2M21 12h2M4.2 19.8l1.4-1.4M18.4 5.6l1.4-1.4"/></svg>
          </label>
      </div>
      <div class="flex-none gap-2">
        <div class="relative" data-te-input-wrapper-init id="displayValue">
            <label class="input input-bordered flex items-center gap-2">
                <input type="text" bind:value={searchItem} on:input={filterItems} class="grow" placeholder="Search" />
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70"><path fill-rule="evenodd" d="M9.965 11.026a5 5 0 1 1 1.06-1.06l2.755 2.754a.75.75 0 1 1-1.06 1.06l-2.755-2.754ZM10.5 7a3.5 3.5 0 1 1-7 0 3.5 3.5 0 0 1 7 0Z" clip-rule="evenodd" /></svg>
              </label>
          {#if filteredItems.length > 0}
            <ul class="dropdown-content absolute left-0 mt-1 dark:bg-slate-600 light:bg-slate-100 shadow-lg rounded-md w-48">
              {#each filteredItems as item}
                <li>
                  <button class="w-full text-left px-2 py-1 dark:hover:bg-gray-100 light:hover:bg-slate-800" on:click={() => {selectItem(item); openModal("itemInfo")}}>{formatName(item.name)}</button>
                </li>
              {/each}
            </ul>
          {/if}
        </div>
      </div>
    </div>
    
    <!--Item stats Modal -->
    <dialog id="itemInfo" class="modal">
      <div class="modal-box w-11/12 max-w-5xl">
        {#if selectedItem}
        <div class="flex flex-row">
        <h3 class="font-bold text-lg">Item info: {formatName(selectedItem.name)}</h3>
      </div>
        <p class="py-4">Item ID: {selectedItem.id}</p>
        <Line data={ selectedItemData }/>
        {/if}
        <div class="modal-action">
          <form method="dialog">
            <!-- if there is a button in form, it will close the modal -->
            <button class="btn">Close</button>
          </form>
        </div>
      </div>
    </dialog>
    
    <!--Detailed stats Modal-->
    <dialog id="detailedStats" class="modal">
        <div class="modal-box w-11/12 max-w-5xl">
          {#if selectedItem}
          <h3 class="font-bold text-lg">Detailed stats {formatName(selectedItem.name)}</h3>
          <p class="py-4">Item ID: {selectedItem.id}</p>
          {/if}
          <div class="modal-action">
            <form method="dialog">
              <!-- if there is a button in form, it will close the modal -->
              <button class="btn">Close</button>
            </form>
          </div>
        </div>
      </dialog>
    
    
    {#if showToast}
      <div class="toast">
        <div class="alert alert-info">
          <span>{toastMessage}</span>
        </div>
      </div>
    {/if}
    </html>