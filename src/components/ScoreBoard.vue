<template>
  <section class="score">
     Player <span>{{this.playerScore}}</span> x <span>{{this.machineScore}}</span> Machine
  </section>
</template>

<script>
export default {
   props: ['playerScore', 'machineScore'],
}
</script>

<style>

   section.score{
      border-bottom: 1px solid black;
      padding: 24px;
      font-size: 18px;
   }
   section.score span{
      padding: 8px;
      font-weight: bold;
      border: 1px solid black;
   }

</style>