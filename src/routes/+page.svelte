<script>
	import Form from '../components/+form.svelte';
</script>

<div class="root">
  <div class="container">
    <Form />
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