<script lang="ts">
    export let title: string;
    export let mal_id: number;
    export let image: string;
</script>

<div class="anime-card h-full w-[200px] rounded-md bg-gray-800 p-4 shadow-lg">
    <a href="/{mal_id}">
        <img
            class="rounded transition-all duration-300 ease-in-out hover:cursor-pointer hover:opacity-80"
            src={image}
            alt={title}
            height="200px"
            width="200px"
        />
    </a>
    <h1 class="mt-4 text-lg font-bold text-white">{title}</h1>
</div>
