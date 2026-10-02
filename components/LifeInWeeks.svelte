<script context="module">
export let weeksSinceBirth = 2012;
export let totalWeeks = 83 * 52;
export let tab = Array(totalWeeks)
</script>
<script>
    export let data;
    // Remove any undefined props (e.g. w/o defaults) to prevent them from being passed
    $: spreadProps = Object.fromEntries(Object.entries($$props).filter(([, v]) => v !== undefined));
    let queryID = data?.id;
</script>

<h2>% of life lived: 14,087 / 29,000 days (48.6%)</h2>
<div class="progress-container">
  <div class="completed-count-total-count-bar" style="width: 48.6%"></div>
</div>

<h2>Life in Weeks</h2>

<div class="week-grid css-selector">
    {#each tab as _, i}
      <div class="week past" title="🏃🏼‍♂️ Week {i + 1} - Age {Math.floor(i / 52)} years"></div>
    {/each}
</div>

 <style>
    .week-grid {
      display: grid;
      grid-template-columns: repeat(52, 1fr);
      gap: clamp(0.1px, 0.1vw, 0.5px);
      width: 100%;
      max-width: 636px;
      margin: 0 auto;
      padding: clamp(1px, 0.5vw, 5px);
      box-sizing: border-box;
    }

    .week {
      aspect-ratio: 1 / 1;
      width: 100%;
      border-radius: 2px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: clamp(0.1rem, 0.5vw, 0.3rem);
      min-height: 0;
    }

    .week[title]:hover::after {
      content: attr(title);
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      background-color: rgba(0, 0, 0, 0.8);
      color: white;
      padding: 4px 8px;
      border-radius: 4px;
      font-size: 12px;
      white-space: nowrap;
      z-index: 1000;
      pointer-events: none;
    }

    .week[title] {
      position: relative;
    }
  
    .past {
      background-color: #4a90e2;
    }
  
    .future {
      background-color: #f0f0f0;
      border: 1px solid #ddd;
    }

    .progress-container {
      width: 100%;
      max-width: 636px;
      margin: 0 auto;
      box-sizing: border-box;
      height: 24px;
      background-color: #f0f0f0;
      border: 1px solid #ddd;
      border-radius: 2px;
      position: relative;
    }

    .completed-count-total-count-bar {
      height: 100%;
      background-color: #4a90e2;
      border-radius: 1px;
    }
  </style>