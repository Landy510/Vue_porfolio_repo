<template>
  <div>
    <AlertMessage/>
    <router-view/>
  </div>
</template>

<script>
import AlertMessage from '../../components/AlertMessage.vue'
export default {
  data () {
    return {

    }
  },
  components: {
    AlertMessage
  }
}
</script>
