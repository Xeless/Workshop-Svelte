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
{#if modalIsActived !== true}
<Test {actived}/>
{/if}
 
	
<style lang="scss">
	
</style>
	