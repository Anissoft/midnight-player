<script lang="ts">
	import Form from '../components/+form.svelte';
	import Player from '../components/+player.svelte';

  let videoUrl: string;
  let videoTimestamp: number;
  let desiredTime: Date;

  const onChange = ({ source, second, time }: { second: number, time: Date, source: string }) => {
    videoUrl = source;
    videoTimestamp = second;
    desiredTime = time;
  }
</script>

<div class="root">
  <div class="container">
    <Form onChange={onChange}/>
    <Player videoUrl={videoUrl} videoTimestamp={videoTimestamp} desiredTime={desiredTime} />
  </div>
</div>

<style>
  .root {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    position: relative;

    padding: 64px 32px;

    display: flex;
    flex-direction: column;
    align-items: center;
    box-sizing: border-box;
  }
  .container {
    width: 100%;
    height: 100%;
    max-width: 1024px;

    background-color: var(--surface-color);
    border: 1px var(--border-color) solid;
    border-radius: 10px;

    padding: 32px;

    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    overflow-x: hidden;
    overflow-y: scroll;
  }

  @media(max-width: 768px) {
    .root {
      padding: 0;
    }

    .container {
      padding: 16px;

      border: none;
      border-radius: 0;
    }
  }
</style>