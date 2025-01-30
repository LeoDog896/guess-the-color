<script lang="ts" module>
    interface Message {
        status: 'win' | 'bad';
        message: string;
    }

    /** Normalize an input to make it easy to search for color names. */
    function normalizeColorName(input: string): string {
        const normalized = input.toLowerCase().replace("-", " ")

        if (normalized === 'gray') return 'grey';

        return normalized;
    }

    /** Gets a random element from an array */
    const random = <T,>(arr: T[]) => arr[Math.floor(Math.random() * arr.length)];

    /** Converts a hex color to an RGB object */
    // https://stackoverflow.com/a/5624139/7589775
    function hexToRgb(hex: string): { R: number, G: number, B: number } | null {
        var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
        return result ? {
            R: parseInt(result[1], 16),
            G: parseInt(result[2], 16),
            B: parseInt(result[3], 16)
        } : null;
    }
</script>

<script lang="ts">
    import Button from '$lib/Button.svelte';
    import { colornames } from 'color-name-list';
    import { diff, rgb_to_lab } from 'color-diff'

    let typedColors = colornames as ({ name: string, hex: string })[];

    /** A map from normalized names to hex */
    const map = new Map<string, string>();

    for (const { name, hex } of typedColors) {
        map.set(normalizeColorName(name), hex);
    }

    let color = $state(random(typedColors));

    function regenerateColor() {
        color = random(typedColors);
    }

    /** The player's current guess */
    let guess = $state("");

    let message = $state<Message | null>(null);

    const pairedMessages = {
        5: 'Might be a good time to take the Ishihara test.',
        25: 'So close!',
        50: 'At least you tried.',
        75: "Try doing better.",
        85: 'Getting warmer.',
        90: "I'm a little impressed. By your bad memory.",
        95: 'Almost there.'
    }

    const sortedPairedMessages = Object.entries(pairedMessages)
        .sort((a, b) => parseInt(b[0]) - parseInt(a[0]))

    function submit() {
        const normalizedGuess = normalizeColorName(guess);

        if (!map.has(normalizedGuess)) {
            message = {
                status: 'bad',
                message: 'Color not found.'
            }

            return;
        }

        const hex = map.get(normalizedGuess)!
        
        if (color.hex !== hex) {
            const distance = diff(
                rgb_to_lab(hexToRgb(color.hex)!),
                rgb_to_lab(hexToRgb(hex)!)
            )

            console.log(diff(
                rgb_to_lab(hexToRgb("#ffffff")!),
                rgb_to_lab(hexToRgb("#000000")!)
            ))


            const stylizedColor = `"<span class="guess" style="--color: ${color.hex}">${color.name}</span>."`

            const normalizedDistance = Math.trunc((101 - distance) / (100 / 101))

            const [, taunt] = sortedPairedMessages.find(([threshold]) => normalizedDistance >= parseInt(threshold))!

            message = {
                status: 'bad',
                message: `${taunt} The color was ${stylizedColor} You were ${normalizedDistance}% correct!`
            }

            regenerateColor();

            return;
        }

        message = {
            status: 'win',
            message: 'Correct!'
        }

        regenerateColor();
    }
</script>

<main>
    <div class="color" style:background-color={color.hex}></div>

    <div class="input">
        <input type="text" bind:value={guess} placeholder="Color Name" />
        <Button onclick={submit} theme={{
            top: "#111",
            bottom: "#000",
            color: "white"
        }}>Submit Guess</Button>
    </div>

    {#if message}
        <p>{@html message.message}</p>
    {/if}
</main>

<style>
    main {
        display: flex;
        align-items: center;
        flex-direction: column;
    }

    div.color {
        display: block;
        width: 200px;
        height: 200px;
        border-radius: 2rem;
    }

    input {
        margin-top: 2rem;
        background-color: white;
        padding: 1rem 2rem;
        font-size: 2rem;
        border-radius: 0.5rem;
        border: 4px solid black;
    }

    :global(span.guess) {
        text-decoration: 3px underline var(--color);
    }
</style>
