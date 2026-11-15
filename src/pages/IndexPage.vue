<template>
  <q-page padding>
    <div class="text-h5">
      Vandaag
    </div>
    <div class="text-body1 text-grey-8 q-mt-sm">
      U10
    </div>

    <AgendaCard />
  </q-page>
</template>

<script setup lang="ts">
import AgendaCard from '@/components/agenda/AgendaCard.vue';
</script>
