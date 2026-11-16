<script setup lang="ts">
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const email = ref('');
const password = ref('');

function signIn() {
  void router.push('/');
}
</script>

<template>
  <q-layout>
    <q-page-container>
      <q-page class="flex flex-center">
        <q-card
          flat
          bordered
          class="q-pa-lg"
          style="width: 360px; max-width: 90vw"
        >
          <div class="text-h5">
            SK Aaigem Jeugd
          </div>
          <div class="text-body1 text-grey-8 q-mt-sm">
            Inloggen
          </div>

          <q-input
            v-model="email"
            class="q-mt-lg"
            label="E-mail"
            type="email"
            outlined
          />

          <q-input
            v-model="password"
            class="q-mt-md"
            label="Wachtwoord"
            type="password"
            outlined
            @keyup.enter="signIn"
          />

          <q-btn
            class="q-mt-lg full-width"
            color="primary"
            label="Inloggen"
            @click="signIn"
          />
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>
