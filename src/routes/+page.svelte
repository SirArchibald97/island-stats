<script>
    import Search from "../svgs/Search.svelte";
    import Star from "../svgs/Star.svelte";
    import LeftArrow from "../svgs/LeftArrow.svelte";
    import { getRankIcon } from "$lib/utils.js";

    export let data;
    export let form;

    let showFavourites = false;
    function toggleFavourites() { showFavourites = !showFavourites; }

    if (form) toggleFavourites();
</script>

<svelte:head>
    <title>MCC Island Stats by SirArchibald</title>
    <link rel="icon" href="%sveltekit.assets%/favicon.png" type="image/png" />
    <meta name="description" content="MCC Island Stats by SirArchibald! Lookup any player to view their game stats, currency, socials and more!" />
</svelte:head>
<main>
    <div class="flex flex-col items-center px-4 sm:px-24 py-32 bg-red-500">
        <p class="text-slate-100 text-5xl sm:text-6xl font-bold text-center">MCC Island Stats</p>
        <p class="text-slate-100 text-xl mt-2">by SirArchibald</p>

        <!-- search -->
        <div class="px-12 py-5">
            <form method="POST" action="?/lookup">
                <div class="flex flex-row bg-slate-100 rounded-lg px-3 py-3">
                    <span class="pr-2"><Search /></span>
                    <input class="bg-slate-100 focus:outline-none" name="username" type="text" placeholder="Username" />
                </div>
            </form>
        </div>
    </div>

    <!-- main -->
    <div class="px-4 sm:px-24 2xl:px-96 py-12 bg-slate-100">

        <!-- featured profiles -->
        <p class="text-slate-700 dark:text-slate-200 text-3xl text-center mb-8 font-semibold">Featured Profiles</p>
        <div class={`${showFavourites && data?.favourites.length === 0 ? "flex flex-row justify-center" : "grid grid-cols-2 sm:grid-cols-3 gap-6"}`}>
            <!-- favourites -->
            {#if showFavourites}
                {#if data?.favourites.length > 0}
                    <button on:click={toggleFavourites} class="flex flex-col justify-center items-center bg-white rounded-lg p-4 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                        <div class="flex flex-row gap-x-1">
                            <span class="self-center w-6 h-6"><LeftArrow /></span>
                            <p class="text-2xl font-semibold">Go back</p>
                        </div>
                    </button>

                    {#each data?.favourites as favourite}
                        <a href={`/player/${favourite.username}`} class="bg-white rounded-lg p-4 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                            <div class="flex flex-col justify-center items-center gap-y-3">
                                <img class="rounded-md" src={`https://crafatar.com/avatars/${favourite.uuid}?overlay`} alt={`SirArchibald97's Profile'`} />
                                <div class="flex flex-row gap-x-2">
                                    <img class="h-8 w-8 bg-slate-400 rounded-md hidden sm:block" src={`https://cdn.islandstats.xyz/ranks/${getRankIcon(favourite.ranks)}`} alt={`SirArchibald97's rank`} />
                                    <p class="text-slate-700 text-center text-lg font-semibold self-center">{favourite.username}</p>
                                </div>
                            </div>
                        </a>
                    {/each}
                {:else}
                    <button on:click={toggleFavourites} class="flex flex-col justify-center items-center bg-white rounded-lg px-24 py-20 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                        <div class="flex flex-row gap-x-1">
                            <span class="self-center w-6 h-6"><Star /></span>
                            <p class="text-2xl font-semibold">No favourites set!</p>
                        </div>
                        <p class="text-xl">Go back</p>
                    </button>
                {/if}
            {:else}
                <button on:click={toggleFavourites} class="flex flex-col justify-center items-center bg-white rounded-lg p-4 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                    <div class="flex flex-row gap-x-1">
                        <span class="self-center w-6 h-6"><Star /></span>
                        <p class="text-2xl font-semibold">Favourites</p>
                    </div>
                </button>

                <!-- SirArchibald97 -->
                <a href={`/player/SirArchibald97`} class="bg-white rounded-lg p-4 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                    <div class="flex flex-col justify-center items-center gap-y-3">
                        <img class="rounded-md" src={`https://crafatar.com/avatars/19f9fd28-558c-4959-98c2-fb1a18bed0a1?overlay`} alt={`SirArchibald97's Profile'`} />
                        <div class="flex flex-row gap-x-2">
                            <img class="h-8 w-8 rounded-md hidden sm:block" src={`https://cdn.islandstats.xyz/ranks/moderator.png`} alt={`SirArchibald97's rank`} />
                            <p class="text-slate-700 text-center text-lg font-semibold self-center">SirArchibald97</p>
                        </div>
                    </div>
                </a>

                <!-- most searched usernames -->
                {#each data.profiles as profile}
                    <a href={`/player/${profile.username}`} class="bg-white rounded-lg p-4 hover:bg-slate-200 hover:scale-105 duration-100 shadow-md">
                        <div class="flex flex-col justify-center items-center gap-y-3">
                            <img class="rounded-md" src={`https://crafatar.com/avatars/${profile.uuid}?overlay`} alt={`${profile.username}'s Profile'`} />
                            <div class="flex flex-row gap-x-2">
                                <img class="h-8 w-8 rounded-md bg-slate-400 hidden sm:block" src={`https://cdn.islandstats.xyz/ranks/${getRankIcon(profile.player.ranks)}`} alt={`${profile.username}'s rank`} />
                                <p class="text-slate-700 text-center text-lg font-semibold self-center">{profile.username}</p>
                            </div>
                        </div>
                    </a>
                {/each}
            {/if}
        </div>
    </div>

    <footer class={`w-full bg-red-500 p-4 relative bottom-0`}>
        <div>
            <p class="text-center text-slate-100">© 2024 <a href="https://sirarchibald.dev" class="underline hover:text-slate-200">SirArchibald</a> • Not affiliated with Minecraft or Noxcrew!</p>
        </div>
    </footer>
</main>