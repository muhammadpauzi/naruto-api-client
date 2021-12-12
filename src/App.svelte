<script>
    import { onMount } from "svelte";

    let dataResponse = {};
    let characters = [];
    let currentSkip = 0;

    $: {
        getCharacters(currentSkip);
    }

    const fetchCharacters = async (skip) => {
        try {
            const res = await fetch(
                `http://localhost:5000/api/characters?skip=${skip}`
            );
            const data = await res.json();
            return { res, data };
        } catch (error) {
            console.log(error);
        }
    };

    const getCharacters = async (skip) => {
        const { res, data } = await fetchCharacters(skip);
        characters = data.data;
        dataResponse = data;
        dataResponse.url = data.url + "/";
        delete dataResponse.data;
    };

    onMount(async () => {
        await getCharacters(currentSkip);
    });
</script>

<main>
    <div class="mx-auto max-w-4xl py-20 px-3">
        <div class="text-center mb-20">
            <h1 class="font-bold text-4xl text-gray-800 mb-2">
                Naruto API Client.
            </h1>
            <p class="text-gray-500">
                This is Client of <a
                    class="text-blue-500 hover:underline"
                    href="https://github.com/muhammadpauzi/naruto-api"
                    target="_blank">Naruto API.</a
                >
            </p>
        </div>

        <hr class="block mb-10 border-1 border-gray-200" />

        <div class="grid grid-cols-3 gap-2">
            {#each characters as character (character._id)}
                <div
                    class="bg-gray-100 p-2 border-2 border-gray-200 hover:shadow text-center"
                >
                    <img
                        class="block mx-auto w-full"
                        src={character.picture == "default.jpg"
                            ? dataResponse.url + character.picture
                            : character.picture.split("/revision")[0]}
                        alt=""
                    />
                    <div class="pt-4 pb-2">
                        <h2 class="text-xl font-bold text-gray-800">
                            {character.name}
                        </h2>
                        <p class="text-gray-500">{character.nameInJapan}</p>
                    </div>
                </div>
            {:else}
                <p class="text-red-500 font-bold text-center w-full py-10">
                    Characters not found.
                </p>
            {/each}
        </div>

        <div class="flex items-center justify-between py-5">
            <div>
                {#if dataResponse.previous}
                    <button
                        on:click={() => {
                            currentSkip = dataResponse.previous.skip;
                        }}
                        class="block hover:bg-gray-200 hover:border-gray-300 rounded hover:shadow py-2 px-4 bg-gray-100 text-gray-500 border-2 border-gray-200 font-bold"
                        >Previous</button
                    >
                {/if}
            </div>
            <div>
                {#if dataResponse.next}
                    <button
                        on:click={() => {
                            currentSkip = dataResponse.next.skip;
                        }}
                        class="block hover:bg-gray-200 hover:border-gray-300 rounded hover:shadow py-2 px-4 bg-gray-100 text-gray-500 border-2 border-gray-200 font-bold"
                        >Next</button
                    >
                {/if}
            </div>
        </div>
    </div>
</main>

<style>
</style>
