<script lang="ts">
    import { enhance } from "$app/forms";
    import Anime from "../anime.svelte";
    import type { PageData } from "./$types";

    export let data: PageData;
</script>

<Anime
    title={data.anime.title}
    mal_id={data.anime.mal_id}
    image={data.anime.images.webp.image_url}
/>

<p class="mb-4 text-gray-300">
    {data.anime.synopsis}
</p>

<form action="?/addToFavorites" method="post" use:enhance>
    <input type="hidden" name="mal_id" value={data.anime.mal_id} />
    <input type="hidden" name="title" value={data.anime.title} />
    <input
        type="hidden"
        name="image"
        value={data.anime.images.webp.image_url}
    />
    <button
        class="mt-4 rounded bg-blue-500 px-4 py-2 font-bold text-white transition-colors duration-300 hover:bg-blue-600"
    >
        Add to favorites
    </button>
</form>

<div class="my-10">
    <a
        class="rounded bg-gray-600 px-4 py-2 font-bold text-white transition-colors duration-300 hover:bg-gray-700"
        href="/"
    >
        Go back to list
    </a>
</div>
