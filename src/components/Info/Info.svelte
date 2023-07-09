<script>
  export let dataSensor;
  export let color;
  import { count, dataInfo, date, openModal } from '../../context/store.js';
  import Modal from '../Modal/Modal.svelte';

  function isOpen () {
    openModal.update(() => true)
  }
</script>

<div class="absolute left-8 top-28 md:top-40 md:left-16">
  <h1 class="text-7xl font-bold lg:text-8xl">{dataInfo[$count].name}</h1>
  <div class="flex flex-col text-4xl lg:text-4xl md:mt-7">
    <p class="{color}">___</p>
    <p class="mt-2 font-bold">{dataInfo[$count].userName}</p>
    <p class="text-lg mb-6">[ {dataInfo[$count].fetchId} ]</p>
    <p class="text-3xl md:text-3xl">{dataInfo[$count].city}</p>
    <p class="text-5xl font-semibold md:text-5xl">{dataInfo[$count].country}</p>
    <div class="flex flex-col mt-24 md:mt-10">
      <p class="text-2xl md:text-2xl">SENSOR CO2</p>
      <p class="font-bold text-8xl md:text-7xl {color}">{dataSensor}<span class="text-2xl md:text-2xl ml-1">ppm</span></p>
    </div>
    <div class="text-lg mt-3">
      <p>{$date}</p>
    </div>
    <p class="hidden lg:block">_</p>
    <div class="flex gap-1 items-center mt-5">
      <button class="w-8 lg:hidden" on:click={isOpen}>
        <img src="/assets/images/info-2.svg" alt="">
      </button>
      <button class="text-base lg:hidden" on:click={isOpen}>
        Más información
      </button>
    </div>
  </div>
</div>

{#if $openModal}
  <Modal />
{/if }

<style>
  .green {
    color: #339900;
  }

  .limeGreen {
    color: #99cc00;
  }

  .yellow {
    color: #ffcc00;
  }

  .orange {
    color: #ff6600;
  }

  .red {
    color: #ff0000;
  }
</style>