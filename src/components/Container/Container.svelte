<script>
  import Render from '../Render/Render.svelte';
  import Arrow from '../Arrow/Arrow.svelte';
  import Info from '../Info/Info.svelte';
  import Loader from '../Loader/Loader.svelte';
  import Footer from '../Footer/Footer.svelte';
  import KnowMore from '../KnowMore/KnowMore.svelte';
  import { count, isLoading, color, dataResponse, date } from '../../context/store.js';
  import { onMount, onDestroy } from 'svelte';
  import { getColor } from './utils.js';
  import soundNamiki from '/assets/sound/respiroNamikiSound.mp3';
  import { DISPOSITIVOS } from '../../constants/constants.js';


  // const URL = 'https://respiro-namiki.netlify.app/api/data.json'
  const URL = 'https://respir0-namiki-default-rtdb.firebaseio.com/data.json'
  // const URL = 'http://localhost:5173/api/data.json'
  let sensorValue = 400;

  dataResponse.subscribe(value => {
    sensorValue = value[$count];
    color.update(() => Number(sensorValue))
  })

  count.subscribe(value => {
    sensorValue = $dataResponse[value];
    color.update(() => Number(sensorValue))
  })

  async function fetchDataSensor () {
		const response = await fetch(URL)
		const data = await response.json()
    const dataMap = DISPOSITIVOS.map((dispositivo) => {
      const key = data[dispositivo];
      return key.co2;
    })
    const getDate = new Date().toLocaleString();
    date.update(() => getDate)
    dataResponse.update(n => n = dataMap)
		return await dataMap
	}

  const interval = setInterval(async () => {
    fetchDataSensor();
  }, 5000);

  onMount(async () => {
    fetchDataSensor();
  });

  onDestroy(() => clearInterval(interval));

</script>

<div class={getColor($color)}>
  {#if $isLoading}
    <Loader />
  {/if }
  {#if !$isLoading}
    <Info dataSensor={sensorValue} color={getColor($color)}/>
    <Arrow />
    <KnowMore />
    <Footer />
  {/if }
    <audio autoplay>
      <source src={soundNamiki} type="audio/mpeg">
    </audio>
    <Render colorValue={getColor($color)}/>
</div>

<style>
  div {
    width: 100%;
    overflow: hidden;
    background-color: #070710;
  }

  .red {
    background: radial-gradient(at 50% 50%, #873740 0%, #272730 40%, #171720 80%, #070710 100%);
  }

  .orange {
    background: radial-gradient(at 50% 50%, #c06034 0%, #272730 40%, #171720 80%, #070710 100%);
  }

  .yellow {
    background: radial-gradient(at 50% 50%, #7c7235 0%, #272730 40%, #171720 80%, #070710 100%);
  }

  .limeGreen {
    background: radial-gradient(at 50% 50%, #5d7c35 0%, #272730 40%, #171720 80%, #070710 100%);
  }

  .green {
    background: radial-gradient(at 50% 50%, #3a6426 0%, #272730 40%, #171720 80%, #070710 100%);
  }
</style>