<script lang="ts">
    import { fly, fade } from 'svelte/transition';
    import { cubicOut, cubicIn } from 'svelte/easing';

    export let open: boolean;
    export let currentPath: string;

    function close() {
        open = false;
    }

    function onPage(path: string) {
        return currentPath === path;
    }

    function handleNav(path: string) {
        if (onPage(path)) {
            close();
        } else {
            window.location.href = path;
        }
    }
</script>

{#if open}
    <!-- Overlay -->
    <button
        type="button"
        aria-label="Close sidebar"
        class="overlay"
        onclick={close}
        in:fade={{ duration: 200 }}
        out:fade={{ duration: 200 }}
    />

    <!-- Sidebar -->
    <nav
        class="w3-sidebar w3-bar-block w3-border-right sidebar"
        style="width:250px"
        in:fly={{ x: -250, duration: 250, easing: cubicOut }}
        out:fly={{ x: -250, duration: 200, easing: cubicIn }}
    >
        <button class="w3-bar-item w3-large" onclick={close}>
            Close &times;
        </button>

        <button
            class="w3-bar-item w3-button {onPage('/') ? 'active' : ''}"
            onclick={() => handleNav('/')}
        >
            Home
        </button>

        <button
            class="w3-bar-item w3-button {onPage('/about') ? 'active' : ''}"
            onclick={() => handleNav('/about')}
        >
            About
        </button>

        <button
            class="w3-bar-item w3-button {onPage('/projects') ? 'active' : ''}"
            onclick={() => handleNav('/projects')}
        >
            Projects
        </button>
    </nav>
{/if}

<style>
    .overlay {
        position: fixed;
        inset: 0;
        z-index: 40;
        background: rgba(0, 0, 0, 0.4);
        backdrop-filter: blur(6px);
    }

    .sidebar {
        z-index: 50;
    }

    .active {
        color: #009688 !important;
    }

    .active:hover {
        background-color: #009688 !important;
        color: white !important;
    }
</style>

