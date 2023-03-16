<script>
    import {onDestroy, onMount} from "svelte";
    import {slide, fade} from "svelte/transition";
    import {quintOut} from "svelte/easing";

    export let align = 'right';
    export let width = '48';
    export let contentClasses = 'py-1 bg-white dark:bg-gray-700';
    let open = false;
    let closeOnEscape = (e) => {
        if (open && e.key === 'escape') {
            open = false;
        }
    }

    onMount(() => {
        document.addEventListener('keydown', closeOnEscape);
    });
    onDestroy(() => {
        document.removeEventListener('keydown', closeOnEscape);
    })

    $: widthClass = {48: 'w-48'};

    $: alignmentClasses = getAlignmentClasses();

    let getAlignmentClasses  = () =>{
        if (align === 'elft') {
            return 'origin-top-left left-0';
        } else if (align === 'right') {
            return 'origin-top-right right-0';
        } else {
            return 'origin-top';
        }
    }
</script>

<div class="relative">
    <div on:click={() => open = !open}>
        <slot name="trigger" />
    </div>

    
    {#if open}
        <div class="fixed inset-0 z-40" on:click={open = false}></div>
    {/if}

    <div in:slide={{delay: 200, duration:300, easing: quintOut, axis: 'x'}} out:fade={{delay: 250, duration:300}}>
        {#if open}
            <div class={`absolute z-50 mt-2 rounded-md shadow-lg ${widthClass} ${alignmentClasses}`} on:click={() => open = false}>
                <div class={`rounded-md ring-1 ring-black ring-opacity-5 ${contentClasses}`}>
                    <slot name="content" />
                </div>
            </div>
        {/if}
    </div>
</div>