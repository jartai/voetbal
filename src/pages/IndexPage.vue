<template>
  <q-page padding>
    <div class="text-h5">
      Vandaag
    </div>
    <div class="text-body1 text-grey-8 q-mt-sm">
      U10
    </div>

    <q-card
      flat
      bordered
      class="q-mt-md"
    >
      <q-card-section>
        <div class="text-overline">
          Volgende wedstrijd
        </div>
        <div class="text-h6">
          {{ opponent.trim() ? opponent : 'Nog geen wedstrijd' }}
        </div>

        <q-input
          v-model="opponent"
          class="q-mt-md"
          label="Tegenstander"
          outlined
        />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { LocalStorage } from 'quasar';
import { ref, watch } from 'vue';

const stored = LocalStorage.getItem('nextOpponent');
const opponent = ref(typeof stored === 'string' ? stored : '');

watch(opponent, (value) => {
  LocalStorage.set('nextOpponent', value);
});
</script>
