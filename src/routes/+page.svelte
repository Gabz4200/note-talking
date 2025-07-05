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
        <div class="sidebar-content-wrapper">
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
        </div>
        <footer class="sidebar-footer">
            <div class="user-profile">
                <div class="avatar"></div>
                <div class="user-info">
                    <span class="username">Gabiru</span>
                    <span class="user-status">Online</span>
                </div>
            </div>
            <div class="user-actions">
                <button
                    class="icon-btn"
                    title="Microfone"
                    aria-label="Microfone"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="20"
                        height="20"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        ><path
                            d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"
                        ></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"
                        ></path><line x1="12" y1="19" x2="12" y2="23"
                        ></line></svg
                    >
                </button>
                <button
                    class="icon-btn"
                    title="Configurações"
                    aria-label="Configurações"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="20"
                        height="20"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        ><circle cx="12" cy="12" r="3"></circle><path
                            d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"
                        ></path></svg
                    >
                </button>
            </div>
        </footer>
    </aside>

    <!-- Resizer Handle for Left Sidebar -->
    <div
        class="resizer resizer-left"
        class:hidden={isLeftSidebarCollapsed}
    ></div>

    <!-- Main Content Area -->
    <main class="content-area">
        <header class="content-header">
            <div class="channel-info">
                <span class="channel-icon">#</span>
                <span class="channel-name">geral</span>
                <div class="channel-description">
                    Tópicos gerais e anotações rápidas.
                </div>
            </div>
            <div class="header-actions">
                <button
                    class="icon-btn"
                    title="Notificações"
                    aria-label="Notificações"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        ><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"
                        ></path><path d="M13.73 21a2 2 0 0 1-3.46 0"
                        ></path></svg
                    >
                </button>
                <button class="icon-btn" title="Fixados" aria-label="Fixados">
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        ><path
                            d="M21.44 11.05l-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66l-9.2 9.19a2 2 0 0 1-2.83-2.83l8.49-8.48"
                        ></path></svg
                    >
                </button>
                <div class="search-bar">
                    <input type="text" placeholder="Buscar" />
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="20"
                        height="20"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        class="search-icon"
                        ><circle cx="11" cy="11" r="8"></circle><line
                            x1="21"
                            y1="21"
                            x2="16.65"
                            y2="16.65"
                        ></line></svg
                    >
                </div>
            </div>
        </header>
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
            <div class="input-wrapper">
                <!-- svelte-ignore a11y_consider_explicit_label -->
                <button class="input-action-btn" title="Anexar arquivo">
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        ><circle cx="12" cy="12" r="10"></circle><line
                            x1="12"
                            y1="8"
                            x2="12"
                            y2="16"
                        ></line><line x1="8" y1="12" x2="16" y2="12"
                        ></line></svg
                    >
                </button>
                <input type="text" placeholder="Conversar em #geral..." />
                <div class="quick-actions">
                    <!-- svelte-ignore a11y_consider_explicit_label -->
                    <button class="input-action-btn" title="Presente">
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            ><polyline points="20 12 20 22 4 22 4 12"
                            ></polyline><rect x="2" y="7" width="20" height="5"
                            ></rect><line x1="12" y1="22" x2="12" y2="7"
                            ></line><path
                                d="M12 7H7.5a2.5 2.5 0 0 1 0-5C11 2 12 7 12 7z"
                            ></path><path
                                d="M12 7h4.5a2.5 2.5 0 0 0 0-5C13 2 12 7 12 7z"
                            ></path></svg
                        >
                    </button>
                    <!-- svelte-ignore a11y_consider_explicit_label -->
                    <button class="input-action-btn" title="Emoji">
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            ><circle cx="12" cy="12" r="10"></circle><path
                                d="M8 14s1.5 2 4 2 4-2 4-2"
                            ></path><line x1="9" y1="9" x2="9.01" y2="9"
                            ></line><line x1="15" y1="9" x2="15.01" y2="9"
                            ></line></svg
                        >
                    </button>
                </div>
            </div>
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
        <div class="sidebar-content">
            <div class="context-section">
                <h4 class="context-title">ONLINE — 2</h4>
                <div class="context-item">
                    <div class="avatar online"></div>
                    <div class="context-user-info">
                        <span class="username">Usuário A</span>
                    </div>
                </div>
                <div class="context-item">
                    <div class="avatar online"></div>
                    <div class="context-user-info">
                        <span class="username">Usuário B</span>
                    </div>
                </div>
            </div>
            <div class="context-section">
                <h4 class="context-title">OFFLINE — 1</h4>
                <div class="context-item">
                    <div class="avatar"></div>
                    <div class="context-user-info">
                        <span class="username">Usuário C</span>
                    </div>
                </div>
            </div>
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
        justify-content: space-between;
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
    .sidebar.collapsed .sidebar-header h3,
    .sidebar.collapsed .sidebar-footer {
        display: none; /* Hide content and title when collapsed */
    }

    .sidebar.collapsed .sidebar-header {
        justify-content: center; /* Center the toggle button */
        height: 100%;
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

    /* Content Header */
    .content-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0.75rem var(--padding);
        border-bottom: 1px solid var(--surface0);
        flex-shrink: 0;
    }
    .channel-info {
        display: flex;
        align-items: center;
        gap: 0.75rem;
    }
    .channel-icon {
        color: var(--overlay1);
        font-size: 1.5rem;
        font-weight: 500;
    }
    .channel-name {
        font-weight: 600;
        color: var(--text);
    }
    .channel-description {
        font-size: 0.8rem;
        color: var(--subtext0);
        padding-left: 0.75rem;
        border-left: 1px solid var(--surface1);
    }
    .header-actions {
        display: flex;
        align-items: center;
        gap: 1rem;
    }
    .search-bar {
        position: relative;
        display: flex;
        align-items: center;
    }
    .search-bar input {
        background-color: var(--mantle);
        border: none;
        border-radius: 4px;
        padding: 0.4rem 0.5rem 0.4rem 2rem;
        color: var(--text);
        font-size: 0.9rem;
        width: 140px;
        transition: width 0.3s ease;
    }
    .search-bar input:focus {
        width: 200px;
        outline: none;
    }
    .search-icon {
        position: absolute;
        left: 0.5rem;
        color: var(--overlay2);
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

    /* Sidebar Footer */
    .sidebar-footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0.5rem;
        background-color: var(--base);
        flex-shrink: 0;
    }
    .user-profile {
        display: flex;
        align-items: center;
        gap: 0.75rem;
    }
    .avatar {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        background-color: var(--peach);
        position: relative;
    }
    .avatar.online::after {
        content: '';
        position: absolute;
        bottom: 0;
        right: 0;
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background-color: var(--green);
        border: 2px solid var(--crust);
    }
    .user-info {
        display: flex;
        flex-direction: column;
    }
    .username {
        font-weight: 600;
        font-size: 0.9rem;
    }
    .user-status {
        font-size: 0.75rem;
        color: var(--subtext0);
    }
    .user-actions {
        display: flex;
        gap: 0.25rem;
    }
    .icon-btn {
        background: none;
        border: none;
        color: var(--overlay2);
        cursor: pointer;
        padding: 0.4rem;
        border-radius: 4px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .icon-btn:hover {
        background-color: var(--surface1);
        color: var(--text);
    }

    /* Input Footer Styles */
    .input-footer {
        padding: 0 var(--padding) var(--padding);
    }
    .input-wrapper {
        display: flex;
        align-items: center;
        background-color: var(--surface0);
        border-radius: var(--border-radius);
        padding: 0 0.5rem;
    }
    .input-wrapper input {
        width: 100%;
        padding: 0.8em 0.5em;
        font-size: 1em;
        font-family: inherit;
        background-color: transparent;
        color: var(--text);
        border: none;
        outline: none;
    }
    .input-action-btn {
        background: none;
        border: none;
        color: var(--overlay2);
        cursor: pointer;
        padding: 0.5rem;
        border-radius: 50%;
    }
    .input-action-btn:hover {
        color: var(--text);
        background-color: var(--surface1);
    }
    .quick-actions {
        display: flex;
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

    /* Right Sidebar Content Styles */
    .context-section {
        margin-bottom: 1.5rem;
    }
    .context-title {
        color: var(--subtext0);
        font-weight: bold;
        padding: 0 0.5rem;
        font-size: 0.8rem;
        margin-bottom: 0.75rem;
        text-transform: uppercase;
    }
    .context-item {
        display: flex;
        align-items: center;
        gap: 0.75rem;
        padding: 0.5rem;
        border-radius: 6px;
        cursor: pointer;
    }
    .context-item:hover {
        background-color: var(--surface0);
    }
    .context-item .avatar {
        width: 32px;
        height: 32px;
    }
    .context-user-info {
        display: flex;
        flex-direction: column;
    }
</style>
