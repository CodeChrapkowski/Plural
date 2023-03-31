<template>
  <div
      class="alert alert-danger"
      v-if="error">{{ error }}
  </div>
</template>

<script>
export default {
  name: "ErrorComponent",

  props: {
    message: {
      type:String,
      required:true
    }
  },

setup(props){
    console.log(props.message);
    return{

    };
}
}
</script>

<style scoped>

</style>