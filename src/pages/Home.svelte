<script>

    import { onMount } from 'svelte';
    import PostForm from '../components/PostForm.svelte';

    const apiBaseUrl = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";

    let postLimit = 6;
    let posts = [];
    let editingPost = {
        body: '',
        title: '',
        id: null
    }

    onMount(async () => {
        const res = await fetch(apiBaseUrl + '/posts');
        posts = await res.json();
    });

    function addPost({ detail: post }){
        if (posts.find(p => p.id === post.id)) {
            const index = posts.findIndex(p => p.id === post.id);
            let postUpdated = posts;
            postUpdated.splice(index, 1, post);
            posts = postUpdated
        } else {
            posts = [post, ...posts];
        }

        editingPost = {
            body: '',
            title: '',
            id: null
        };
    }

    const editPost = (post) => {
        editingPost = post
    }

    const deletePost = (id) => {
        if (confirm("Are you sure ?")) {
            fetch(`${apiBaseUrl}/post/${id}`, {
                method: 'DELETE'
            }).then(res => {
                return res.json()
            }).then(() => {
                posts = posts.filter(p => p.id !== id);
            });
        }
    }

    const setLimit = () => {
        fetch(`${apiBaseUrl}/posts/${postLimit}`)
        .then(res => {
            return res.json();
        })
        .then(postsData => {
            posts = postsData;
        })
    }
</script>


<div class="row">
    <div class="col s6">
        <PostForm on:postCreated={addPost} {editingPost}/>
    </div>

    <div class="col s3">
        <p>Limit number of posts</p>
        <input type="number" bind:value={postLimit}>
        <button on:click={setLimit} class="waves-effect waves-light btn">
            Set
        </button>
    </div>
</div>

<div class="row">
    {#if posts.length === 0}
        <h3>Loading companies data ...</h3>
    {:else}
        {#each posts as post}
            <div class="col s6">
                <div class="card">
                    <div class="card-content">
                        <p class="card-title">{post.title}</p>
                        <p>{post.body}</p>
                        <p class="timestamp">{post.createdAt}</p>
                    </div>

                    <!-- svelte-ignore a11y-invalid-attribute -->
                    <div class="card-action">
                        <a href="#" on:click={() => editPost(post)}>Edit</a>
                        <a href="#" class="delete-btn" on:click={() => deletePost(post.id)}>
                            Delete</a>
                    </div>
                </div>
            </div>
        {/each}
    {/if}
</div>


<style>
    .delete-btn {
        color: red !important;
    }
    .card .card-content .card-title {
        margin-bottom: 0;
    }
    .card .card-content p.timestamp {
       color: #999;
       margin-bottom: 10px;
    }
</style>
