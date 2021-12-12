<script>
    import { onMount } from "svelte";

    let characters = [];
    let baseURLAPI = "";

    const fetchCharacters = async () => {
        try {
            const res = await fetch("http://localhost:5000/api/characters");
            const data = await res.json();
            return { res, data };
        } catch (error) {
            console.log(error);
        }
    };

    const getCharacters = async () => {
        const { res, data } = await fetchCharacters();
        characters = data.data;
        baseURLAPI = data.url + "/";
    };

    onMount(async () => {
        await getCharacters();
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
                            ? baseURLAPI + character.picture
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
            {/each}
        </div>
    </div>
</main>

<style>
</style>
