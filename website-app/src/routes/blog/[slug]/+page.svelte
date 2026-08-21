<script lang="ts">
    import { error } from '@sveltejs/kit';
    import type { Component } from 'svelte';

    let { params } = $props();

    const posts = import.meta.glob(
        '../../../content/blog/*.md',
        { eager: true }
    );

    const path = `../../../content/blog/${params.slug}.md`;
    const post = posts[path] as {
        default: Component;
        metadata: {
            title: string;
            published: string;
            modified?: string;
        };
    };

    if (!post) {
        error(404, 'Post not found');
    }

    const Post = post.default;
    const metadata = post.metadata;
</script>

<svelte:head>
    <title>{metadata.title}</title>
</svelte:head>

<article>
    <h1>{metadata.title}</h1>

    <p>
        Published: {metadata.published}
    </p>

    {#if metadata.modified}
        <p>
            Modified: {metadata.modified}
        </p>
    {/if}

    <Post />
</article>
