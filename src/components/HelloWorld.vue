<template>
  <div class="Overall__Actions">
      <button class="box" @click="deleteAll">
        <img :src="trashCan" @click="removeTodo(index)"/>
        <span>Delete All</span>
      </button>
  </div>
</template>

<script>
// Import Images
import trashCan from '../assets/trash.svg'
export default {
  name: 'DeleteAll',
  data () {
    return {
      trashCan: trashCan
    }
  },
  methods: {
    deleteAll() {
        this.$emit('deleteAll')
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.Overall__Actions {
  display: flex;
  justify-content: center;
  button {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    margin-top: 8px;
    padding: 8px 16px;
    background-color: #dc3545;
    border: none;
    border-radius: 4px;
    color: #fff;
    margin-bottom: 16px;
    cursor: pointer;
    img {
      width: 20px;
      height: 20px;
      gap: 4px;
      filter: invert(1);
    }
  }

}
</style>
