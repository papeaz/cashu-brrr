<script lang="ts">
    import { getDecodedToken, getEncodedTokenV4 } from "@cashu/cashu-ts";
  import "./app.css";
    import ComicNote from "./lib/ComicNote.svelte";
  import Main from "./lib/Main.svelte";
    import { getAmountForTokenSet } from "./lib/utils";
  const urlParams = window.location.hash.slice(1).split('/');
  const token = $derived.by(()=>{
    const tokenString = urlParams[0];
    try {
      return getDecodedToken(tokenString??"")
    } catch (error) {
      return undefined
    } })
  const template = $derived.by(()=>{
    const param = urlParams[1];
    if (!param) return 5;
    const design = Number.parseInt(param)
    const d = isNaN(design) ? 5 : design
    return d
  })
  
  let sizeMM = 120;  // tamaño en milímetros
  let heightMM = 70; // alto en mm (valor inicial)
</script>



<div class="mb-4">
  <label for="size" class="font-semibold">Tamaño del billete (ancho en mm):</label>
  <select id="size" bind:value={sizeMM} class="ml-2 p-1 border rounded">
    <option value="100">100 mm</option>
    <option value="120">120 mm</option>
    <option value="150">150 mm</option>
    <option value="180">180 mm</option>
  </select>
</div>

<div class="mb-4 flex flex-wrap gap-2 items-center">
  <span class="font-semibold">Tamaños estándar:</span>
  <button type="button" class="px-2 py-1 bg-gray-200 rounded hover:bg-gray-300" on:click="{() => { sizeMM = 156; heightMM = 66; } }">
    USD (156×66 mm)
  </button>
  <button type="button" class="px-2 py-1 bg-gray-200 rounded hover:bg-gray-300" on:click="{() => { sizeMM = 140; heightMM = 77; } }">
    EUR (140×77 mm)
  </button>
  <button type="button" class="px-2 py-1 bg-gray-200 rounded hover:bg-gray-300" on:click="{() => { sizeMM = 140; heightMM = 70; } }">
    BOB (140×70 mm)
  </button>
  <button type="button" class="px-2 py-1 bg-gray-200 rounded hover:bg-gray-300" on:click="{() => { sizeMM = 140; heightMM = 65; } }">
    BRL (140×65 mm)
  </button>
</div>

{#if token}
<div>
  design={template}
  denomination={getAmountForTokenSet(token.proofs)}
  mintUrl={token.mint}
  token={getEncodedTokenV4(token)}
  unit={token.unit??"sat"}
  />
</div>
{:else}
  
<Main></Main>
{/if}