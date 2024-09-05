<script lang="ts">
    export let currentPage: number;
    export let totalPages: number;
    export let onPageChange: (page: number) => void;

    function handlePrevious() {
        if (currentPage > 1) {
            onPageChange(currentPage - 1);
        }
    }

    function handleNext() {
        if (currentPage < totalPages) {
            onPageChange(currentPage + 1);
        }
    }
</script>

<div class="mt-4 flex justify-center">
    {#if currentPage > 1}
        <button
            class="mx-2 rounded bg-gray-600 px-4 py-2 font-bold text-white hover:bg-gray-700"
            on:click={handlePrevious}
        >
            Previous
        </button>
    {/if}

    {#if currentPage < totalPages}
        <button
            class="mx-2 rounded bg-gray-600 px-4 py-2 font-bold text-white hover:bg-gray-700"
            on:click={handleNext}
        >
            Next
        </button>
    {/if}
</div>
