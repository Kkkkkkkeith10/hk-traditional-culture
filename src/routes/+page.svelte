<script lang="ts">
    import { onDestroy } from "svelte";

    type Tile = {
        id: number;
        word: string;
        en: string;
        note: string;
        related: number[];
    };

    // Each tile is a piece of Hong Kong traditional culture. Tapping a tile reads
    // it aloud in Cantonese (browser speech synthesis, zh-HK) and lights up
    // related tiles for a moment, so you can follow one idea to the next.
    const tiles: Tile[] = [
        { id: 1, word: "舞獅", en: "Lion dance", note: "Performed at openings and festivals to bring good luck.", related: [2, 12] },
        { id: 2, word: "農曆新年", en: "Lunar New Year", note: "Family visits, red packets and flower markets.", related: [3, 4, 5] },
        { id: 3, word: "利是", en: "Red packets", note: "Lucky money given by married relatives to the young and unmarried.", related: [2] },
        { id: 4, word: "年宵花市", en: "New Year flower market", note: "Night markets selling peach blossom and tangerine plants.", related: [2] },
        { id: 5, word: "盆菜", en: "Poon choi", note: "A layered village feast shared from one basin.", related: [6, 13] },
        { id: 6, word: "圍村", en: "Walled villages", note: "Fortified villages in the New Territories.", related: [5, 13] },
        { id: 7, word: "端午節", en: "Tuen Ng Festival", note: "Dragon boat races and rice dumplings.", related: [8, 9] },
        { id: 8, word: "龍舟", en: "Dragon boats", note: "Team boat races held on the fifth day of the fifth month.", related: [7] },
        { id: 9, word: "糉", en: "Rice dumplings", note: "Glutinous rice wrapped in bamboo leaves.", related: [7] },
        { id: 10, word: "中秋節", en: "Mid-Autumn Festival", note: "Mooncakes, lanterns and the fire dragon dance.", related: [11, 14] },
        { id: 11, word: "月餅", en: "Mooncakes", note: "Pastries shared under the full moon.", related: [10] },
        { id: 12, word: "大坑舞火龍", en: "Tai Hang fire dragon", note: "A dragon of incense sticks danced through Tai Hang.", related: [10, 1] },
        { id: 13, word: "太平清醮", en: "Cheung Chau Bun Festival", note: "Bun towers and a parade on Cheung Chau.", related: [6, 5] },
        { id: 14, word: "花燈", en: "Lanterns", note: "Paper lanterns lit for festivals.", related: [10] },
        { id: 15, word: "粵劇", en: "Cantonese opera", note: "Traditional opera on UNESCO's intangible heritage list.", related: [16] },
        { id: 16, word: "神功戲", en: "Deity-worship opera", note: "Bamboo-theatre opera staged for local deities.", related: [15, 17] },
        { id: 17, word: "天后誕", en: "Tin Hau Festival", note: "Celebrating the goddess of the sea.", related: [16, 18] },
        { id: 18, word: "漁村", en: "Fishing villages", note: "Tai O stilt houses and harbour life.", related: [17] },
        { id: 19, word: "茶樓", en: "Yum cha", note: "Tea and dim sum with family.", related: [20] },
        { id: 20, word: "冰室", en: "Bing sutt", note: "Old-style cafés serving milk tea and pineapple buns.", related: [19] },
    ];

    let active: Tile | null = null;
    let lit = new Set<number>();
    let timer: ReturnType<typeof setTimeout> | null = null;
    const LIGHT_MS = 1500;

    function speak(text: string) {
        if (typeof window === "undefined" || !("speechSynthesis" in window)) return;
        const u = new SpeechSynthesisUtterance(text);
        u.lang = "zh-HK";
        window.speechSynthesis.cancel();
        window.speechSynthesis.speak(u);
    }

    function tap(tile: Tile) {
        active = tile;
        speak(tile.word);
        lit = new Set(tile.related);
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => (lit = new Set()), LIGHT_MS);
    }

    onDestroy(() => {
        if (timer) clearTimeout(timer);
    });
</script>

<main class="min-h-screen bg-gray-800 flex items-center justify-center">
    <div class="max-w-xl w-full p-6">
        <h1 class="text-3xl font-bold text-center mb-2 text-white">香港傳統文化</h1>
        <p class="text-center text-gray-300 mb-6">Tap a tile to hear it in Cantonese. Related tiles light up.</p>

        <div class="grid grid-cols-4 gap-4">
            {#each tiles as tile (tile.id)}
                <button
                    class="aspect-square shadow-md p-2 transition-all duration-200 focus:outline-none text-xl font-bold flex items-center justify-center text-center"
                    class:bg-white={!lit.has(tile.id) && active?.id !== tile.id}
                    class:bg-red-300={lit.has(tile.id)}
                    class:bg-gray-200={active?.id === tile.id}
                    on:click={() => tap(tile)}
                >
                    {tile.word}
                </button>
            {/each}
        </div>

        {#if active}
            <div class="mt-6 bg-white p-4 rounded">
                <p class="text-2xl font-bold">{active.word} <span class="text-base font-normal text-gray-600">{active.en}</span></p>
                <p class="mt-2 text-gray-800">{active.note}</p>
            </div>
        {/if}
    </div>
</main>
