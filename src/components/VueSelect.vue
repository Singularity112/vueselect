<template>
  <div class="vueSelect">
    <input
      v-model="query"
      @input="search"
      @keyup.up="paginateUp()"
      @keyup.down="paginateDown()"
      @keyup.enter="selectCurrent"
    />

    <div class="close" @click="removeSelection" v-if="selectedItem">
      <img src="@/assets/close.svg">
    </div>

    <div class="items" :class="{ active: searching }" ref="items">
      <div v-if="filtered.length">
        <div 
          class="item"
          v-for="(item, index) in filtered"
          :key="index"
          @click="setItem(item)"
          :class="{active: currentPag === index}"
        >
          {{ item }}
        </div>
      </div>
      <div v-else>
        no matching options
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      query: '',
      searching: false,
      filtered: [],
      currentPag: -1,
      selectedItem: ''
    }
  },
  props: {
    items: {
      required: true
    },
    value: {
      required: true
    }
  },
  methods: {
    search () {
      this.searching = this.query.length > 0;
      this.filterItems();
      this.currentPag = -1;
    },
    filterItems () {
      this.filtered = this.items.filter(item => item.includes(this.query));
    },
    paginateUp () {
      this.currentPag = this.currentPag < 1 ? this.filtered.length - 1 : this.currentPag - 1;
      this.scrollContainer();
    },
    paginateDown () {
      this.currentPag = this.currentPag === this.filtered.length - 1 ? 0 : this.currentPag + 1;
      this.scrollContainer();
    },
    scrollContainer () {
      this.$refs.items.scrollTop = (this.currentPag * 30) - 30;
    },
    selectCurrent () {
      if (this.currentPag === -1) {
        return;
      }

      const item = this.filtered[this.currentPag];
      this.setItem(item);
    },
    removeSelection () {
      this.query = '';
      this.$emit('input', '');
      this.selectedItem = '';
    },
    foucsOut () {
      this.query = this.selectedItem;
      this.searching = false;
    },
    setItem (item) {
      this.query = item;
      this.searching = false;
      this.$emit('input', item);
      this.selectedItem = item;
    },
  },
}
</script>
<style scoped lang="scss">
  .vueSelect {
    position: relative;

    .close {
      position: absolute;
      top: 50%;
      right: 15px;
      transform: translateY(-50%);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    input {
      width: 100%;
      box-sizing: border-box;
      height: 44px;
      padding-left: 15px;
      padding-right: 15px;
    }

    .items {
      display: none;
      position: absolute;
      max-height: 120px;
      overflow-y: auto;
      top: 100%;
      left: 0;
      width: 100%;
      box-shadow: 0 1px 3px rgba(0,0,0,.25);

      &.active {
        display: block;
      }

      .item {
        padding: 5px 15px;
        height: 30px;
        box-sizing: border-box;
        border-bottom: 1px solid lightgray;
        cursor: pointer;

        &.active {
          background-color: green;
        }

        &:hover {
          background-color: lightblue;
        }
      }
    }
  }
</style>