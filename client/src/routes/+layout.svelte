<script lang="ts">
    import "../app.css";
    import type { LayoutData } from "./$types";
    import Anime from "./anime.svelte";

    export let data: LayoutData;
</script>

<div class="mx-auto flex w-3/4 flex-col p-4">
    <div>
        <slot />
    </div>
    <div>
        <h1 class="mb-4 text-xl font-bold text-gray-300">Favorites:</h1>
        {#each [...data.favorites] as [key, value]}
            <Anime title={value.title} mal_id={key} image={value.image} />
        {/each}
    </div>
</div>
