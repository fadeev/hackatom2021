<template>
  <main>
    <kinesis-container :perspective="1000">
      <section-hero />
      <section-competition />
      <section-prize-pool />
    </kinesis-container>
    <section-prize-categories />
    <section-chat />
    <section-sponsored />
    <section-judging-criteria />
    <section-judges />
    <section-schedule />
    <section-incoming-transmissions />
    <section-participate />
    <!-- <section-faq /> -->
  </main>
</template>

<script>
import SectionPrizePool from '../components/SectionPrizePool.vue'
export default {
  components: { SectionPrizePool },
}
</script>

<style lang="stylus" scoped></style>
