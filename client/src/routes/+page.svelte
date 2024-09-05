<script lang="ts">
    import { onMount, afterUpdate, tick } from "svelte";
    import { gsap } from "gsap";
    import Anime from "./anime.svelte";
    import type { PageData } from "./$types";

    let ScrollTrigger: any;

    async function loadGSAPPlugins() {
        const module = await import("gsap/ScrollTrigger");
        ScrollTrigger = module.ScrollTrigger;
        gsap.registerPlugin(ScrollTrigger);
    }

    export let data: PageData;

    type SubRecommendation = {
        title: string;
        mal_id: number;
        images: {
            webp: {
                image_url: string;
            };
        };
    };

    type Recommendation = {
        content: string;
        entry: SubRecommendation[];
    };

    let loading = true;
    let currentPage = 1;
    const itemsPerPage = 5;
    let paginatedData: Recommendation[] = [];

    const totalPages = Math.ceil(data.recommended.data.length / itemsPerPage);

    async function paginate(page: number) {
        currentPage = page;
        const start = (currentPage - 1) * itemsPerPage;
        const end = start + itemsPerPage;
        paginatedData = data.recommended.data.slice(start, end);

        await tick();
        window.scrollTo({
            top: 0,
            behavior: "smooth",
        });
    }

    async function applyGsapAnimations() {
        await tick();

        paginatedData.forEach((_, i) => {
            const animeElement = document.querySelector(`#anime-${i}`);
            if (animeElement) {
                gsap.from(`#anime-${i}`, {
                    x: i % 2 === 0 ? -200 : 200,
                    opacity: 0,
                    duration: 1.5,
                    ease: "power4.out",
                    scrollTrigger: {
                        trigger: `#anime-${i}`,
                        start: "top 80%",
                        toggleActions: "play none none none",
                    },
                });
            }
        });
    }

    onMount(async () => {
        await loadGSAPPlugins();
        paginate(currentPage);
        loading = false;
    });

    afterUpdate(() => {
        applyGsapAnimations();
    });
</script>

{#if loading}
    <div
        class="absolute left-0 top-0 flex h-full w-full items-center justify-center bg-gray-900"
    >
        <svg
            class="mr-3 h-10 w-10 animate-spin text-white"
            xmlns="http://www.w3.org/2000/svg"
            fill="white"
            viewBox="0 0 24 24"
            stroke="currentColor"
        >
            <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6v6m0 0v6m0-6h6m-6 0H6"
            />
        </svg>
    </div>
{/if}

{#each paginatedData as recommendation, i}
    <div
        class="mb-4 grid grid-cols-2 rounded border border-gray-800 p-4"
        id={`anime-${i}`}
    >
        <h1
            class="col-span-2 border-b border-gray-800 p-2 text-xl text-gray-300"
        >
            {recommendation.content}
        </h1>

        {#each recommendation.entry as subRecommendation}
            <div class="mb-2 flex justify-around p-2">
                <Anime
                    title={subRecommendation.title}
                    mal_id={subRecommendation.mal_id}
                    image={subRecommendation.images.webp.image_url}
                />
            </div>
        {/each}
    </div>
{/each}

<div class="mt-4 flex justify-center">
    {#if currentPage > 1}
        <button
            class="mx-2 rounded bg-gray-600 px-4 py-2 font-bold text-white hover:bg-gray-700"
            on:click={() => paginate(currentPage - 1)}
        >
            Previous
        </button>
    {/if}

    {#if currentPage < totalPages}
        <button
            class="mx-2 rounded bg-gray-600 px-4 py-2 font-bold text-white hover:bg-gray-700"
            on:click={() => paginate(currentPage + 1)}
        >
            Next
        </button>
    {/if}
</div>
