<script lang="ts">
  export let title: string;
  export let mal_id: number;
  export let image: string;
</script>

<div class="anime-card  h-full bg-gray-800 rounded-md shadow-lg p-4"   >
  <h1 class="text-lg font-bold text-white mb-2">{title}</h1>
  <a href="/{mal_id}">
    <img
      class="hover:opacity-80 hover:cursor-pointer rounded transition-all duration-300 ease-in-out"
      src={image}
      alt={title}
      height="200px"
      width="200px"
    />
  </a>
</div>