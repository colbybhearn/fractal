<template>
  <div class="hello">
    <label>size</label><input id='size' type="number" name="" v-model="size"  v-on:change='update'><br/>
		<label>Generation</label><input id='gen' type="number" name="" v-model="gen" v-on:change='update'><br/>
    
		<input id='sizeLrg' type="button" text="Larger" value="Larger" >
		<input id='sizeSml' type="button" text="Smaller" value = "Smaller">
  </div>
</template>

<script>
export default {
  name: 'Controls',
  data: function(){
    return {
    size : 50,
    gen: 3
    }
  },
  props: {
    msg: String
  },
  methods:{
    
    update: function(){
      let input = {
        
      } ;     
      this.$emit('update', input);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
