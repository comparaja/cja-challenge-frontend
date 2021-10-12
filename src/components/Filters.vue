<template>
  <header>
    <div class="filters desktop">
      <div class="group">
        <label for="provider">Provider</label>
        <select class="input" v-model="value.provider" id="provider">
          <option value="">Todos</option>
          <option v-for="provider in providers" :value="provider.id" :key="'provider' + provider.id">{{ provider.name }}</option>
        </select>
      </div>
      <div class="group">
        <label for="connection">Tipo</label>
        <select class="input" v-model="value.connectionType" id="connection">
          <option value="">Todos</option>
          <option v-for="connType in connectionTypes" :value="connType.id" :key="'conn-type' + connType.id">{{ connType.description }}</option>
        </select>
      </div>
      <div class="group">
        <label for="canais">Canais</label>
        <input id="canais" v-model="value.channels" class="input" inputmode="numeric" type="text" placeholder="Canais">
      </div>
      <div class="group">
        <label for="internet">Velocidade</label>
        <input id="internet" v-model="value.internetSpeed" class="input" inputmode="numeric" type="text" placeholder="Internet">
      </div>
      <div class="group">
        <label for="preco">Preço</label>
        <input id="preco" v-model="value.price" class="input" inputmode="numeric" type="text" placeholder="Preço">
      </div>
      <div class="group">
        <label for="order">Ordenação</label>
        <select v-model="value.sortBy" class="input" name="order" id="order">
          <option value="monthlyPrice">Preço</option>
          <option value="numberOfChannels">Canais</option>
          <option value="internetSpeed">Velocidade</option>
        </select>
      </div>
    </div>
    <div class="filters mobile">
      <div class="group">
        <button @click="showFilters = true" class="input"><em class="fa fa-filter"></em> Filtros</button>
      </div>
      <div class="group right">
        <label for="order"><em class="fa fa-sort"></em> Ordenação</label>
        <select v-model="value.sortBy" class="input" name="order" id="order">
          <option value="monthlyPrice">Preço</option>
          <option value="numberOfChannels">Canais</option>
          <option value="internetSpeed">Velocidade</option>
        </select>
      </div>
      <div v-if="showFilters" class="modal">
        <div class="modal-title">
          <h2>Filtros</h2>
          <em @click="showFilters = false" class="fas fa-times"></em>
        </div>
        <hr>
        <div class="group">
          <label for="provider">Provider</label>
          <select class="input" v-model="value.provider" id="provider">
            <option value="">Todos</option>
            <option v-for="provider in providers" :value="provider.id" :key="'provider' + provider.id">{{ provider.name }}</option>
          </select>
        </div>
        <div class="group">
          <label for="connection">Tipo</label>
          <select class="input" v-model="value.connectionType" id="connection">
            <option value="">Todos</option>
            <option v-for="connType in connectionTypes" :value="connType.id" :key="'conn-type' + connType.id">{{ connType.description }}</option>
          </select>
        </div>
        <div class="group">
          <label for="canais">Canais</label>
          <input id="canais" v-model="value.channels" class="input" inputmode="numeric" type="text" placeholder="Canais">
        </div>
        <div class="group">
          <label for="internet">Velocidade</label>
          <input id="internet" v-model="value.internetSpeed" class="input" inputmode="numeric" type="text" placeholder="Internet">
        </div>
        <div class="group">
          <label for="preco">Preço</label>
          <input id="preco" v-model="value.price" class="input" inputmode="numeric" type="text" placeholder="Preço">
        </div>
      </div>
    </div>
  </header>
</template>

<script>
  export default {
    props: {
      value: {
        required: true,
        type: Object
      },
      providers: {
        required: true,
        default: [],
        type: Array
      },
      connectionTypes: {
        required: true,
        default: [],
        type: Array
      }
    },
    watch: {
      showFilters: (val) => {
        if(val) {
          document.body.style.overflow = "hidden";
        } else {
          document.body.style.overflow = "auto";
        }
      }
    },
    data() {
      return {
        showFilters: false,
      }
    },
  }
</script>

<style scoped>
  header {
    position: sticky;
    z-index: 2;
    top: 85px;
    box-shadow: 0 1px 3px rgb(0 0 0 / 20%);
    padding: 20px calc((100vw - 1020px) / 2);
    background-color: var(--blue10);
  }

  header .filters {
    display: flex;
  }

  header .filters.mobile {
    display: none;
  }

  header .filters .group {
    width: calc((100% / 6) - 20px);
    padding: 0 10px; 
    display: flex;
    flex-direction: column;
  }

  header .filters .group label {
    font-weight: bold;
    font-size: 14px;
    color: var(--green3);
  }

  header .filters .group .input {
    padding: 10px;
    font-size: 16px;
    border: solid 2px var(--grey4);
    border-radius:5px;
    background-color: var(--white);
  }
  header .filters .group .input:active, header .filters .group .input:focus{
    border: solid 2px var(--blue1);
    outline: none;
  }

  .modal {
    display: none;
  }

  @media (max-width: 1200px) {
    header {
      padding: 20px;
    }
  }

  @media (max-width: 992px) {
    header .filters.desktop {
      display: none;
    }
    header .filters.mobile {
      display: flex;
    }

    header .filters .group {
      flex-direction: row;
      width: auto;
    }

    header .filters .group button.input {
      cursor: pointer;
      font-weight: bold;
      padding: 10px 20px;
    }

    header .filters .group.right {
      margin-left: auto;
    }

    header .filters .group label {
      font-weight: bold;
      font-size: 16px;
      line-height: 42px;
      margin-right: 5px;
    }

    .modal {
      display: block;
      position: fixed;
      inset: 0;
      width: calc(100vw - 80px);
      max-width: calc(100vw - 80px);
      height: calc(100vh - 40px);
      max-height: calc(100vh - 40px);
      overflow-x: hidden;
      overflow-y: auto;
      background-color: var(--white3);
      padding: 20px 40px;
    }

    .modal .modal-title {
      display: flex
    }

    .modal .modal-title h2 {
      margin: 0;
      padding: 0;
    }

    .modal .modal-title em {
      margin-left: auto;
      font-size: 24px;
      cursor: pointer;
    }

    .modal hr {
      margin: 20px 0 40px;
    }

    .modal .group {
      margin: 20px 0;
    }

    .modal .group label, .modal .group .input {
      width: 50% 
    }

    .modal .group select.input {
      width: calc(50% + 20px) 
    }
  }
</style>
