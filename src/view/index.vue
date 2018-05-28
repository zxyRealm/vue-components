<template>
    <div class="view-wrap">
      <ip-input v-model="ip"></ip-input>
    <free-drag v-model="coordinate"></free-drag>
    </div>
</template>

<script>
  import ipInput from '@/components/IpInput'
  import FreeDrag from '@/components/FreeDrag'
    export default {
      components:{
        ipInput,
        FreeDrag
      },
        name: "index",
        data(){
          return {
            ip:'3.5. .6',
            coordinate:[]
          }
        }
    }
</script>

<style scoped>

</style>
