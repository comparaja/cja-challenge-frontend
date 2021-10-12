<template>
  <div class="result">
    <div class="header">
      <span class="description">
        {{ result.description }}
      </span>
    </div>
    <div class="body">
      <div class="provider">
        {{ result.provider }}
      </div>
      <div class="connection">
        {{ result.connectionType }}
      </div>
      <div class="channels">
        {{ result.numberOfChannels }} Canais
      </div>
      <div class="speed">
        {{ result.internetSpeed }} Mbps
      </div>
      <div class="cost">
        {{ result.monthlyPrice }} €
      </div>
      <div class="actions">
        <button class="cta">Aderir</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      result: {
        required: true,
        type: Object
      }
    }
  }
</script>

<style scoped>
.result {
  border: solid 1px var(--grey6);
  border-radius: 10px;
  background-color: var(--white)
}

.result .header {
  background-color: var(--grey6);
  padding: 10px 20px;
}

.result .body  {
  display: flex
}

.result .body > * {
  width: calc(100% / 6);
  text-align: center;
  padding: 20px;
  font-size: 18px;
  line-height: 38px;
}

.result .body .provider {
  font-weight: bold;
}

.result .body .actions .cta {
  padding: 10px 20px;
  color: white;
  background-color: var(--orange1);
  font-weight: bold;
  font-size: 18px;
  border: solid 1px var(--orange2);
  border-radius: 5px;
  cursor: pointer;
}

@media (max-width: 992px)  {

  .result .body  {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: auto;
  }
  .result .body > * {
    width: auto;
  }
}

</style>
