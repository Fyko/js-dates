<script lang="ts">
  // Create a reactive date that updates every second
  let currentDate = $state(new Date());
  
  // Update the date every second
  const updateInterval = setInterval(() => {
    currentDate = new Date();
  }, 1000);
  
  // Clean up interval on component destruction
  $effect(() => {
    return () => {
      clearInterval(updateInterval);
    };
  });
  
  // Copy to clipboard function
  function copyToClipboard(text: string) {
    navigator.clipboard.writeText(text);
  }
  
  // Track which method was recently copied for feedback
  let recentlyCopied = $state('');
  
  function handleCopy(name: string, value: string) {
    copyToClipboard(value);
    recentlyCopied = name;
    
    // Reset the copied state after 2 seconds
    setTimeout(() => {
      if (recentlyCopied === name) {
        recentlyCopied = '';
      }
    }, 2000);
  }
  
  // Array of formatter methods to showcase
  const formatters = [
    { name: 'toString', method: (date: Date) => date.toString() },
    { name: 'toDateString', method: (date: Date) => date.toDateString() },
    { name: 'toISOString', method: (date: Date) => date.toISOString() },
    { name: 'toLocaleDateString', method: (date: Date) => date.toLocaleDateString() },
    { name: 'toLocaleString', method: (date: Date) => date.toLocaleString() },
    { name: 'toLocaleTimeString', method: (date: Date) => date.toLocaleTimeString() },
    { name: 'toTimeString', method: (date: Date) => date.toTimeString() },
    { name: 'toUTCString', method: (date: Date) => date.toUTCString() }
  ];
</script>

<div class="min-h-screen bg-gradient-to-b from-indigo-50 via-slate-50 to-slate-100 dark:from-slate-900 dark:via-slate-800 dark:to-slate-900 text-slate-800 dark:text-slate-200 p-6 transition-colors duration-300">
  <header class="max-w-3xl mx-auto mb-12">
    <h1 class="text-4xl font-bold text-center mb-3 text-indigo-800 dark:text-indigo-300">js date formatters</h1>
    <div class="w-16 h-1 bg-indigo-400 dark:bg-indigo-500 mx-auto mb-4 rounded-full"></div>
    <p class="text-center text-slate-600 dark:text-slate-400 max-w-lg mx-auto">live showcase of javascript's built-in date formatting methods, updating in real-time</p>
  </header>
  
  <main class="max-w-3xl mx-auto">
    <div class="bg-white/50 dark:bg-slate-800/50 backdrop-blur-sm p-4 rounded-lg mb-8 text-center border border-indigo-100 dark:border-slate-700 shadow-sm">
      <div class="text-2xl font-mono text-indigo-700 dark:text-indigo-300 tabular-nums">{currentDate.toISOString()}</div>
      <div class="text-xs text-slate-500 dark:text-slate-400 mt-1">current time (ISO format)</div>
    </div>
    
    <div class="grid gap-5">
      {#each formatters as formatter}
        <div class="bg-white dark:bg-slate-800 rounded-xl shadow-sm p-5 border border-slate-200 dark:border-slate-700 hover:shadow-md hover:border-indigo-200 dark:hover:border-indigo-700 transition-all duration-300">
          <div class="flex flex-col md:flex-row md:justify-between md:items-center gap-3">
            <div class="font-mono text-sm bg-indigo-50 dark:bg-indigo-900/40 px-3 py-1.5 rounded-md text-indigo-700 dark:text-indigo-300 inline-block border border-indigo-100 dark:border-indigo-800">
              Date.prototype.{formatter.name}()
            </div>
            <button 
              on:click={() => handleCopy(formatter.name, formatter.method(currentDate))}
              class="text-right text-xs px-3 py-1.5 rounded-md border border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-700 transition-colors flex items-center gap-1 self-start md:self-auto"
            >
              {#if recentlyCopied === formatter.name}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 text-green-500 dark:text-green-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
                </svg>
                <span class="text-green-600 dark:text-green-400">copied!</span>
              {:else}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 text-slate-400 dark:text-slate-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3" />
                </svg>
                <span class="dark:text-slate-400">copy</span>
              {/if}
            </button>
          </div>
          <div class="mt-4 font-mono text-sm break-all p-3 bg-slate-50 dark:bg-slate-900/50 rounded-md border border-slate-200 dark:border-slate-700 text-slate-700 dark:text-slate-300 cursor-pointer hover:bg-slate-100 dark:hover:bg-slate-900 transition-colors" on:click={() => handleCopy(formatter.name, formatter.method(currentDate))}>
            {formatter.method(currentDate)}
          </div>
        </div>
      {/each}
    </div>
  </main>
  
  <footer class="max-w-3xl mx-auto mt-16 pb-8 text-center text-slate-500 dark:text-slate-400 text-sm">
    <div class="w-12 h-0.5 bg-slate-200 dark:bg-slate-700 mx-auto mb-4"></div>
    <p>built with svelte and tailwind • {new Date().getFullYear()}</p>
  </footer>
</div>
