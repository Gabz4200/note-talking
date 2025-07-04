<script lang="ts">
    import { invoke } from '@tauri-apps/api/core'
    import { getCurrentWindow } from '@tauri-apps/api/window'

    let name = $state('')
    let greetMsg = $state('')

    const appWindow = getCurrentWindow()

    async function greet(event: Event) {
        event.preventDefault()
        // Learn more about Tauri commands at https://tauri.app/develop/calling-rust/
        greetMsg = await invoke('greet', { name })
    }
</script>

<main class="container">
    <div data-tauri-drag-region class="titlebar">
        <button
            type="button"
            class="titlebar-button"
            id="titlebar-minimize"
            onclick={() => appWindow.minimize()}
        >
            <img
                src="https://api.iconify.design/mdi:window-minimize.svg"
                alt="minimize"
            />
        </button>

        <button
            type="button"
            class="titlebar-button"
            id="titlebar-maximize"
            onclick={() => appWindow.toggleMaximize()}
        >
            <img
                src="https://api.iconify.design/mdi:window-maximize.svg"
                alt="maximize"
            />
        </button>

        <button
            type="button"
            class="titlebar-button"
            id="titlebar-close"
            onclick={() => appWindow.close()}
        >
            <img src="https://api.iconify.design/mdi:close.svg" alt="close" />
        </button>
    </div>

    <h1>Welcome to Tauri + Svelte</h1>

    <div class="row">
        <a href="https://vitejs.dev" target="_blank">
            <img src="/vite.svg" class="logo vite" alt="Vite Logo" />
        </a>
        <a href="https://tauri.app" target="_blank">
            <img src="/tauri.svg" class="logo tauri" alt="Tauri Logo" />
        </a>
        <a href="https://kit.svelte.dev" target="_blank">
            <img
                src="/svelte.svg"
                class="logo svelte-kit"
                alt="SvelteKit Logo"
            />
        </a>
    </div>
    <p>Click on the Tauri, Vite, and SvelteKit logos to learn more.</p>

    <form class="row" onsubmit={greet}>
        <input
            id="greet-input"
            placeholder="Enter a name..."
            bind:value={name}
        />
        <button class="btn-primary" type="submit">Greet</button>
    </form>
    <p>{greetMsg}</p>
</main>

<style>
    .logo.vite:hover {
        filter: drop-shadow(0 0 2em var(--overlay2));
    }

    .logo.svelte-kit:hover {
        filter: drop-shadow(0 0 2em var(--overlay2));
    }

    :root {
        font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
        font-size: 16px;
        line-height: 24px;
        font-weight: 400;

        color: var(--surface0);
        background-color: var(--subtext1);

        font-synthesis: none;
        text-rendering: optimizeLegibility;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        -webkit-text-size-adjust: 100%;
    }

    .container {
        margin: 0;
        padding-top: 10vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align: center;
    }

    .logo {
        height: 6em;
        padding: 1.5em;
        will-change: filter;
        transition: 0.75s;
    }

    .logo.tauri:hover {
        filter: drop-shadow(0 0 2em var(--teal));
    }

    .row {
        display: flex;
        justify-content: center;
    }

    a {
        font-weight: 500;
        color: var(--teal);
        text-decoration: inherit;
    }

    a:hover {
        color: var(--sapphire);
    }

    h1 {
        text-align: center;
    }

    input,
    .btn-primary {
        border-radius: 8px;
        border: 1px solid transparent;
        padding: 0.6em 1.2em;
        font-size: 1em;
        font-weight: 500;
        font-family: inherit;
        color: var(--surface0);
        background-color: var(--text);
        transition: border-color 0.25s;
        box-shadow: 0 2px 2px rgba(0, 0, 0, 0.2);
    }

    .btn-primary {
        cursor: pointer;
    }

    .btn-primary:hover {
        border-color: var(--blue);
        background-color: var(--subtext1);
    }
    .btn-primary:active {
        border-color: var(--blue);
        background-color: var(--subtext0);
    }

    input,
    .btn-primary {
        outline: none;
    }

    #greet-input {
        margin-right: 5px;
    }

    @media (prefers-color-scheme: dark) {
        :root {
            color: var(--subtext1);
            background-color: var(--crust);
        }

        a:hover {
            color: var(--teal);
        }

        img[src*='mdi:window-minimize'],
        img[src*='mdi:window-maximize'],
        img[src*='mdi:close'] {
            filter: invert(1) brightness(0.8) contrast(1);
        }

        input,
        .btn-primary {
            color: var(--text);
            background-color: var(--surface0);
        }
        .btn-primary:active {
            background-color: var(--surface0);
        }
        .btn-primary:hover {
            color: var(--text);
            background-color: var(--surface1);
        }
    }

    .titlebar {
        height: 35px;
        background: var(--base);
        user-select: none;
        display: flex;
        justify-content: flex-end;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
    }

    .titlebar .titlebar-button {
        background: none;
        border: none;
        padding: 0;
        border-radius: 0;
        font: inherit;
        line-height: initial;

        opacity: 1;

        cursor: pointer;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        width: 35px;
        height: 35px;

        user-select: none;
        -webkit-user-select: none;
    }

    .titlebar .titlebar-button:hover {
        background: var(--surface0);
        border-color: transparent;
    }
</style>
