<script lang="ts">
    interface Post {
        default: unknown;
        metadata: {
            title: string;
            published: string;
            modified?: string;
        };
    }

    const files = import.meta.glob<Post>(
        '../../content/blog/*.md',
        {
            eager: true
        }
    );

    const posts = Object.entries(files)
        .map(([path, post]) => ({
            slug: path.split('/').pop()?.replace('.md', ''),
            ...post
        }))
        .sort(
            (a, b) =>
                new Date(b.metadata.published).getTime() -
                new Date(a.metadata.published).getTime()
        );
</script>

<svelte:head>
    <title>Blog</title>
</svelte:head>

<main>
    <h1>Blog</h1>

    <div class="posts">
        {#each posts as post}
            <a href={`/blog/${post.slug}`} class="post">
                <h2>{post.metadata.title}</h2>

                <div class="dates">
                    <span>Published: {post.metadata.published}</span>

                    {#if post.metadata.modified}
                        <span>Modified: {post.metadata.modified}</span>
                    {/if}
                </div>
            </a>
        {/each}
    </div>
</main>

<style>
    .posts {
        display: flex;
        flex-direction: column;
    }

    .post {
        display: block;
        text-decoration: none;
    }
</style>
