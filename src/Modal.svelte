<script>
    import {fade, blur, fly, slide} from "svelte/transition";
    import {createEventDispatcher} from "svelte";

    let w;
    const dispatch = createEventDispatcher();


</script>

<style>
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.6);

    & .modal {
      background: white;
      padding: 20px;
      border-radius: 15px;
    }
  }

</style>


<div class="modal-bg" transition:fade bind:clientWidth={w}>
  <div class="modal" trasition:fly={{y:200}}>
    <button on:click={()=>{
        dispatch('close');
    }}>Close
    </button>
    <!--  <slot>optional fallback</slot>-->
    <slot/> <!--Generic slot-->
    <!--  <slot name="modal"/>-->
  </div>
</div>


