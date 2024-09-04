<script>
	export let actived;

function close() {
    if (typeof actived === 'function') {
        actived(); 
    }
}

</script>

<button on:click={close}>Check</button>
<style scoped></style>