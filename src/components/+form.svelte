<script>

</script>

<form>
  <p class="form-text">
    <label for="source">Video from URL:</label> 
    <input type="text" id="source" name="source" placeholder="YouTube link or direct link to video file"><br/>
    <label for="second">Should play exact second:</label>
    <input type="number" id="second" name="second" min="0" max="" step="1" value="0"><br/>
    <label for="time">When current time is: </label>
    <input type="time" id="time" name="time" step="10" value="00:00:00"><br/>
  <p>
</form> 

<style>
  input {
    background-color: unset;
    border: none;
    font-size: 18px;
    border: 1px var(--border-color) solid;
    outline: none;
    color: var(--font-color);
  }

  label {
    display: inline-block;
    min-width: 232px;
  }

  .form-text {
    white-space: nowrap;
    line-height: 25px;

    font-size: 18px;
  }

  input#source {
    width: calc(100% - 250px);
  }

  input#second {
    width: 50px;
  }

  input#time {
    width: 200px;
  }

  @media(max-width: 768px) {
    .form-text {
      white-space: normal;
    }

    label, input#source {
      width: 100%;
      max-width: 100%;
    }

    input {
      margin-bottom: 16px;
    }
  }
</style>