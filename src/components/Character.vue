<template>
  <div class="column is-12-mobile is-4-deskto is-4-tablet">
          <div class="card">
            <div class="card-header">
              <img v-bind:src="character.image" v-bind:alt="character.name" >
            </div>

          <div class="card-content">
            <h3 class="title is-size-4">{{character.name}}</h3>
            <button class="button is-success is-rounded is-small" @click="showMore(character.id)">ver mas</button>
          </div>
        </div>
       </div>
</template>


<script>
export default {
  props:['character'],   //variables que pasan depadres a hijos
methods:{
  showMore(id){
    this.$emit('showModal',id);
  }
}
}

</script>
