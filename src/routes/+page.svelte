<script lang="ts">
    import { onMount } from 'svelte'

    // State for controlling the visibility of the sidebars
    let isLeftSidebarCollapsed = $state(false)
    let isRightSidebarCollapsed = $state(false)

    // State to track resizing status for disabling transitions
    let isLeftResizing = $state(false)
    let isRightResizing = $state(false)

    // References to the DOM elements for resizing
    let leftSidebar: HTMLElement
    let rightSidebar: HTMLElement

    // --- Resizing Logic ---
    onMount(() => {
        const leftResizer = document.querySelector(
            '.resizer-left',
        ) as HTMLElement
        const rightResizer = document.querySelector(
            '.resizer-right',
        ) as HTMLElement

        // This function will handle the resizing logic
        const handleMouseMove = (e: MouseEvent) => {
            if (isLeftResizing) {
                // Calculate new width, constrained
                const newWidth = Math.max(180, Math.min(e.clientX, 1080))
                leftSidebar.style.width = `${newWidth}px`
            }
            if (isRightResizing) {
                // Calculate new width, constrained
                const newWidth = Math.max(
                    180,
                    Math.min(window.innerWidth - e.clientX, 1080),
                )
                rightSidebar.style.width = `${newWidth}px`
            }
        }

        // This function will be called when the mouse button is released
        const handleMouseUp = () => {
            // Update resizing state to re-enable transitions
            isLeftResizing = false
            isRightResizing = false

            // Remove the listeners from the window object
            window.removeEventListener('mousemove', handleMouseMove)
            window.removeEventListener('mouseup', handleMouseUp)
            // Reset body styles
            document.body.style.cursor = 'default'
            document.body.style.userSelect = 'auto'
        }

        // Add listener for the left resizer
        leftResizer.addEventListener('mousedown', () => {
            // Update resizing state to disable transitions
            isLeftResizing = true
            // Apply global styles to prevent text selection and set cursor
            document.body.style.cursor = 'col-resize'
            document.body.style.userSelect = 'none'
            // Re-Add listeners to the window to capture mouse movement anywhere on the screen
            window.addEventListener('mousemove', handleMouseMove)
            window.addEventListener('mouseup', handleMouseUp)
        })

        // Add listener for the right resizer
        rightResizer.addEventListener('mousedown', () => {
            // Update resizing state to disable transitions
            isRightResizing = true
            // Apply global styles to prevent text selection and set cursor
            document.body.style.cursor = 'col-resize'
            document.body.style.userSelect = 'none'
            // Re-Add listeners to the window to capture mouse movement anywhere on the screen
            window.addEventListener('mousemove', handleMouseMove)
            window.addEventListener('mouseup', handleMouseUp)
        })
    })

    // --- Toggle Functions ---
    function toggleLeftSidebar() {
        isLeftSidebarCollapsed = !isLeftSidebarCollapsed
        // Reset inline width when collapsing to prevent resize bug
        if (isLeftSidebarCollapsed) {
            leftSidebar.style.width = ''
        }
    }

    function toggleRightSidebar() {
        isRightSidebarCollapsed = !isRightSidebarCollapsed
        // Reset inline width when collapsing to prevent resize bug
        if (isRightSidebarCollapsed) {
            rightSidebar.style.width = ''
        }
    }
</script>

<div class="main-container">
    <!-- Left Sidebar -->
    <aside
        bind:this={leftSidebar}
        class="sidebar left-sidebar"
        class:collapsed={isLeftSidebarCollapsed}
        class:is-resizing={isLeftResizing}
    >
        <div class="sidebar-header">
            <h3>Notas</h3>
            <button class="toggle-btn" onclick={toggleLeftSidebar}>
                {isLeftSidebarCollapsed ? '→' : '←'}
            </button>
        </div>
        <div class="sidebar-content">
            <!-- Placeholder for your notes list -->
            <a href="#/" class="note-item active"># geral</a>
            <a href="#/" class="note-item"># ideias-projeto</a>
            <a href="#/" class="note-item"># rascunhos</a>
            <a href="#/" class="note-item"># links-uteis</a>
        </div>
    </aside>

    <!-- Resizer Handle for Left Sidebar -->
    <div
        class="resizer resizer-left"
        class:hidden={isLeftSidebarCollapsed}
    ></div>

    <!-- Main Content Area -->
    <main class="content-area">
        <div class="messages-container">
            <!-- Placeholder for chat/note content -->
            <div class="message">
                <p class="author">Sistema</p>
                <p>Bem-vindo ao seu novo app de anotações!</p>
                <p class="timestamp">Agora mesmo</p>
            </div>
            <div class="message">
                <p class="author">Você</p>
                <p>
                    Esta é a primeira anotação. O layout parece ótimo. Os cantos
                    arredondados e o espaçamento dão uma sensação limpa e
                    moderna.
                </p>
                <p class="timestamp">Há 2 minutos</p>
            </div>
            <div class="message">
                <p class="author">Você</p>
                <p>
                    Posso redimensionar as barras laterais arrastando as
                    divisórias e também recolhê-las usando os botões de seta.
                </p>
                <p class="timestamp">Há 1 minuto</p>
            </div>
        </div>

        <div class="input-footer">
            <input type="text" placeholder="Escreva sua anotação aqui..." />
        </div>
    </main>

    <!-- Resizer Handle for Right Sidebar -->
    <div
        class="resizer resizer-right"
        class:hidden={isRightSidebarCollapsed}
    ></div>

    <!-- Right Sidebar -->
    <aside
        bind:this={rightSidebar}
        class="sidebar right-sidebar"
        class:collapsed={isRightSidebarCollapsed}
        class:is-resizing={isRightResizing}
    >
        <div class="sidebar-header">
            <h3>Contexto</h3>
            <button class="toggle-btn" onclick={toggleRightSidebar}>
                {isRightSidebarCollapsed ? '←' : '→'}
            </button>
        </div>
        <div class="sidebar-content">
            <!-- Placeholder -->
            <p class="context-title">Online - 2</p>
            <div class="context-item">Usuário A</div>
            <div class="context-item">Usuário B</div>
            <p class="context-title">Offline - 1</p>
            <div class="context-item offline">Usuário C</div>
        </div>
    </aside>
</div>

<style>
    :root {
        /* Color Palette is in app.css */

        --border-radius: 12px;
        --padding: 1rem;
        --gap: 8px; /* The space between the panels */
        --outer-padding: 16px; /* Padding around the entire layout */
    }

    /* Basic Reset and Global Styles */
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    /* Ensure the app fills the viewport and prevent body scrolling */
    :global(html, body) {
        height: 100%;
        width: 100%;
        overflow: hidden;
    }

    .main-container {
        height: 100vh;
        width: 100%;
        overflow: hidden;
        font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
        background-color: var(--crust);
        color: var(--text);
        display: flex;
        padding: var(--outer-padding);
        gap: var(--gap);
    }

    /* Sidebar Styles */
    .sidebar {
        background-color: var(--mantle);
        border-radius: var(--border-radius);
        display: flex;
        flex-direction: column;
        overflow: hidden;
        flex-shrink: 0;
    }

    /* Apply transition ONLY when NOT resizing */
    .sidebar:not(.is-resizing) {
        transition:
            width 0.3s ease,
            min-width 0.3s ease;
    }

    .left-sidebar {
        width: 240px;
    }

    .right-sidebar {
        width: 240px;
    }

    /* Styles for the collapsed sidebar state */
    .sidebar.collapsed {
        width: 40px; /* A fixed, compact width */
        min-width: 40px;
    }

    .sidebar.collapsed .sidebar-content,
    .sidebar.collapsed .sidebar-header h3 {
        display: none; /* Hide content and title when collapsed */
    }

    .sidebar.collapsed .sidebar-header {
        justify-content: center; /* Center the toggle button */
    }

    .sidebar-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: var(--padding);
        padding-bottom: 0.5rem;
        flex-shrink: 0;
    }

    .sidebar-header h3 {
        color: var(--subtext1);
        font-weight: 500;
        white-space: nowrap;
    }

    .sidebar-content {
        padding: 0 var(--padding) var(--padding) var(--padding);
        overflow-y: auto;
        flex-grow: 1;
    }

    .sidebar-content::-webkit-scrollbar {
        width: 8px;
    }
    .sidebar-content::-webkit-scrollbar-track {
        background: transparent;
    }
    .sidebar-content::-webkit-scrollbar-thumb {
        background-color: var(--surface1);
        border-radius: 20px;
        border: 3px solid transparent;
    }

    .toggle-btn {
        background: none;
        border: none;
        color: var(--overlay2);
        cursor: pointer;
        font-size: 1.5rem;
        padding: 0 0.5rem;
        border-radius: 4px;
    }

    .toggle-btn:hover {
        background-color: var(--surface0);
        color: var(--text);
    }

    /* Resizer Handle Styles */
    .resizer {
        background-color: transparent;
        cursor: col-resize;
        width: var(--gap);
        flex-shrink: 0;
        transition:
            background-color 0.2s ease,
            border-radius 0.2s ease;
        z-index: 10;
    }
    .resizer:hover {
        background-color: var(--overlay1);
        border-radius: 6px;
    }

    .resizer.hidden {
        display: none;
    }

    /* Main Content Area Styles */
    .content-area {
        flex-grow: 1;
        background-color: var(--base);
        border-radius: var(--border-radius);
        display: flex;
        flex-direction: column;
        overflow: hidden;
        min-width: 300px;
    }

    .messages-container {
        flex-grow: 1;
        overflow-y: auto;
        padding: var(--padding);
    }

    .messages-container::-webkit-scrollbar {
        width: 10px;
    }
    .messages-container::-webkit-scrollbar-track {
        background: transparent;
    }
    .messages-container::-webkit-scrollbar-thumb {
        background-color: var(--surface1);
        border-radius: 20px;
        border: 3px solid var(--base);
    }

    .message {
        margin-bottom: 1.5rem;
    }
    .message .author {
        font-weight: bold;
        color: var(--peach);
        margin-bottom: 0.25rem;
    }
    .message .timestamp {
        font-size: 0.75rem;
        color: var(--overlay1);
        margin-top: 0.25rem;
    }

    /* Input Footer Styles */
    .input-footer {
        padding: var(--padding);
        padding-top: 0;
    }

    .input-footer input {
        width: 100%;
        padding: 0.8em 1.2em;
        font-size: 1em;
        font-family: inherit;
        background-color: var(--surface0);
        color: var(--text);
        border: 1px solid var(--surface1);
        border-radius: var(--border-radius);
        outline: none;
        transition:
            border-color 0.25s,
            background-color 0.25s;
    }

    .input-footer input:focus {
        border-color: var(--blue);
        background-color: var(--surface1);
    }

    /* Placeholder Content Styles */
    .note-item {
        display: block;
        padding: 0.5rem 0.75rem;
        margin-bottom: 0.25rem;
        text-decoration: none;
        color: var(--subtext0);
        border-radius: 6px;
        font-weight: 500;
    }
    .note-item:hover {
        background-color: var(--surface0);
        color: var(--text);
    }
    .note-item.active {
        background-color: var(--surface1);
        color: var(--text);
    }

    .context-title {
        color: var(--subtext0);
        font-weight: bold;
        margin-top: 1rem;
        margin-bottom: 0.5rem;
        padding: 0 0.5rem;
        font-size: 0.9em;
    }
    .context-item {
        padding: 0.5rem;
        color: var(--subtext1);
        border-radius: 6px;
    }
    .context-item.offline {
        color: var(--overlay1);
    }
</style>
