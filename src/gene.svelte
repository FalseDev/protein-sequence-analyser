<script lang="ts">
  import genes from "./genes.ts";
  let output = "";
  
  let inputValue: string = "";
  $: {
    let counter = {};
    for (let key in genes){
      counter[key] = 0
    }
    let cleansed = inputValue.replace(/[, ]/g, "")
    output = "";
    for (let i = 0; i < cleansed.length; i++) {
      let key = cleansed[i].toUpperCase();
      if (!(key in counter)){
        output = `Unknown gene ${key}`
        break
      } 
      counter[key] += 1
    }
    let empty = true
    for (let key of Object.keys(counter)){
      if (counter[key] && empty){
        empty = false;
      }
    }
    if (!output && !empty){
      for (let key of Object.keys(counter)){
        if (counter[key]){
          let count = counter[key]
          let name = genes[key].padEnd(10)
          let percent = (count/cleansed.length * 100).toFixed(2);
          output += `${name}: ${count} (${percent})<br />`
        }
      }
    }
    else if(!output && empty){
      output = "Start typing..."
    }
  }
</script>


<main class="flex-container">
  <div>
    <textarea
      bind:value={inputValue}
      placeholder="Enter Genes here"
      id="gene-input"
    />
  </div>
      <div class="output">
        {@html output}
      </div>

</main>


