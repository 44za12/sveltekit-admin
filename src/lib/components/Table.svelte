<script>
  export let tableData = [];
  export const title = "Datatable";
  $: usedData = tableData;
  $: startPos = 0;
  $: endPos = 10;
  $: currentData = usedData.slice(startPos,endPos);
  $: currentPage = 1;
  // @ts-ignore
  const totalPages = parseInt(tableData.length / 2) + 1
  const pagesArray = [];
  for (let i = 1; i<=totalPages; i++){
    pagesArray.push(i);
  }
  const gotoNextPage = async () => {
    startPos += 10;
    endPos += 10;
    currentData = usedData.slice(startPos,endPos);
    currentPage += 1;
  }
  const gotoPrevPage = async () => {
    startPos -= 10;
    endPos -= 10;
    currentData = usedData.slice(startPos,endPos);
    currentPage -= 1;
  }
  const filterData = async () => {
    // @ts-ignore
    let searchedKey = document.getElementById("searchInput").value;
    if (searchedKey === "" || searchedKey.trim() === "") {
      usedData = tableData;
      return
    };
    usedData = [];
    for (let i = 0; i<=tableData.length; i++) {
      Object.values(tableData[i]).forEach(val => {
        if (val.indexOf(searchedKey) > -1) {
          usedData.push(tableData[i])
        }
      });
    }
  }
</script>
<div class="w-full mb-8 overflow-hidden rounded-lg shadow-xs">
  <div class="w-full overflow-x-auto">
    <div class="w-full">
      <h3 class="text-center text-lg font-semibold text-gray-600 dark:text-gray-300">{title.toUpperCase()}</h3>
      <div class="relative my-3 focus-within:text-purple-500">
        <div class="absolute inset-y-0 flex items-center pl-2">
          <svg class="w-4 h-4" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20">
            <path
            fill-rule="evenodd"
            d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
            clip-rule="evenodd"
            />
          </svg>
        </div>
        <input
        class="w-full pl-8 pr-2 py-2 text-sm text-gray-700 placeholder-gray-600 bg-gray-100 border-0 rounded-md dark:placeholder-gray-500 dark:focus:shadow-outline-gray dark:focus:placeholder-gray-600 dark:bg-gray-700 dark:text-gray-200 focus:placeholder-gray-500 focus:bg-white focus:border-purple-300 focus:outline-none focus:shadow-outline-purple form-input"
        type="text"
        placeholder="Search across columns and rows"
        aria-label="Search"
        id = "searchInput"
        on:keydown={() => {filterData()}}
        />
      </div>
    </div>
    <table class="w-full whitespace-no-wrap">
      <thead>
        <tr
        class="text-xs text-center font-semibold tracking-wide text-gray-500 uppercase border-b dark:border-gray-700 bg-gray-50 dark:text-gray-400 dark:bg-gray-800"
        >
        {#each Object.keys(tableData[0]) as col}
        <th class="px-4 py-3">{col}</th>
        {/each}
      </tr>
    </thead>
      <tbody class="bg-white divide-y dark:divide-gray-700 dark:bg-gray-800">
        {#if currentData}
        {#each currentData as row}
        <tr class="text-gray-700 dark:text-gray-400">
          {#each Object.values(row) as val}
          <td class="px-4 py-3 text-sm"> {val} </td>
          {/each}
        </tr>
        {/each}
        {:else}
        <tr rowspan="10" class="text-center">
          <td colspan={tableData[0].length} class="py-auto my-auto px-auto mx-auto">
            Loading...
          </td>
        </tr>
        {/if}
      </tbody>
    </table>
  </div>
  <span class="text-sm text-gray-700 dark:text-gray-400 float-left my-4 py-2">
      Showing <span class="font-semibold text-gray-900 dark:text-white">{startPos+1}</span> to <span class="font-semibold text-gray-900 dark:text-white">{endPos < usedData.length ? endPos : usedData.length}</span> of <span class="font-semibold text-gray-900 dark:text-white">{usedData.length}</span> Entries
  </span>
  <div class="flex flex-col float-right my-3">
    <div class="inline-flex mt-2 xs:mt-0">
      {#if startPos != 0}
      <button class="inline-flex items-center py-2 px-4 text-sm font-medium text-gray-500 uppercase border border-gray-300 dark:border-gray-700 bg-gray-200 dark:text-gray-400 dark:bg-gray-800" on:click={() => {gotoPrevPage()}}>
        <svg aria-hidden="true" class="mr-2 w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M7.707 14.707a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l2.293 2.293a1 1 0 010 1.414z" clip-rule="evenodd"></path></svg>
            Prev
        </button>
      {:else}
        <button disabled class="inline-flex items-center py-2 px-4 text-sm font-medium disabled text-gray-600 border-gray-300 uppercase border dark:border-gray-700 bg-gray-300 dark:text-gray-500 dark:bg-gray-700">
          <svg aria-hidden="true" class="mr-2 w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M7.707 14.707a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l2.293 2.293a1 1 0 010 1.414z" clip-rule="evenodd"></path></svg>
          Prev
        </button>
      {/if}
      {#if endPos + 10 <= tableData.length}
        <button class="inline-flex items-center py-2 px-4 text-sm font-medium text-gray-500 uppercase border border-gray-300 dark:border-gray-700 bg-gray-200 dark:text-gray-400 dark:bg-gray-800" on:click={() => {gotoNextPage()}}>
            Next
            <svg aria-hidden="true" class="ml-2 w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
        </button>
      {:else}
        <button disabled class="inline-flex items-center py-2 px-4 text-sm font-medium disabled text-gray-600 uppercase border border-gray-300 dark:border-gray-700 bg-gray-300 dark:text-gray-500 dark:bg-gray-700">
          Next
          <svg aria-hidden="true" class="ml-2 w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
        </button>
      {/if}
    </div>
  </div>
</div>