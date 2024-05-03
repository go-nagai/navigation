<script>
    import { onMount } from 'svelte';
    import { goto, afterNavigate } from '$app/navigation';

    export let data;
    let formdata = {};

    export const snapshot = {
		capture: () => formdata,
		restore: (value) => formdata = value
	};

    const getState = () => {
        const params = new URLSearchParams(window.location.search);
        if (params.has('state')) {
            return params.get('state');
        } else {
            return 'default';
        }
    }

    onMount(() => {
        data.state = getState();
    });

    afterNavigate(() => {
        data.state = getState();
    })

    const changeState = (newState) => {
        goto(`?state=${newState}`);
    };
</script>

<h1>Page {(data.state === 'results') ? 2 : 1}</h1>

<div>
    <p>Current State: {data?.state}</p>
    {#if data.state === 'results'}
         {JSON.stringify(formdata)}
         <button on:click={() => changeState('form')}>Back</button>
    {:else}
         <form>
            <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input
                    type="text"
                    class="form-control"
                    name="name"
                    id="name"
                    aria-describedby="helpId"
                    placeholder="Type you name"
                    bind:value={formdata.name}
                />
                <small id="helpId" class="form-text text-muted">Help text</small>
             </div>
             <button on:click={(e) => {e.preventDefault();changeState('results');}}>Compute</button>
         </form>
         
    {/if}
</div>