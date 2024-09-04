<script>
import Modal from './Modal.svelte'
import Test from './Test.svelte'
// variables

let modalIsActived = false;
console.log(modalIsActived);
function actived() {
    modalIsActived === false ? modalIsActived = true : modalIsActived = false;
    console.log(modalIsActived);
    
    
}


</script>
{#if modalIsActived === true}
<Modal {actived}/>
{/if}

<Test {actived}/>
	
<style lang="scss">
	
</style>
	