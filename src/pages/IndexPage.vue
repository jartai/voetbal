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
          Gepland
        </div>

        <div
          v-if="items.length === 0"
          class="text-h6"
        >
          Nog niets gepland
        </div>

        <q-list
          v-else
          separator
        >
          <q-item
            v-for="item in items"
            :key="item.id"
          >
            <q-item-section>
              <q-item-label>{{ item.title }}</q-item-label>
              <q-item-label caption>
                {{ item.type === 'wedstrijd' ? 'Wedstrijd' : 'Training' }}
              </q-item-label>
            </q-item-section>

            <q-item-section side>
              <q-btn
                flat
                round
                dense
                icon="delete"
                aria-label="Verwijderen"
                @click="removeItem(item.id)"
              />
            </q-item-section>
          </q-item>
        </q-list>

        <q-select
          v-model="kind"
          class="q-mt-md"
          label="Soort"
          outlined
          emit-value
          map-options
          :options="kinds"
        />

        <q-input
          v-model="title"
          class="q-mt-md"
          label="Naam"
          outlined
          @keyup.enter="addItem"
        />

        <q-btn
          class="q-mt-md"
          color="primary"
          label="Toevoegen"
          :disable="title.trim() === ''"
          @click="addItem"
        />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { LocalStorage } from 'quasar';
import { ref, watch } from 'vue';

type AgendaType = 'wedstrijd' | 'training';

type AgendaItem = {
  id: string;
  type: AgendaType;
  title: string;
};

const kinds = [
  {
    label: 'Wedstrijd',
    value: 'wedstrijd'
  },
  {
    label: 'Training',
    value: 'training'
  },
];

function loadItems(): AgendaItem[] {
  const stored = LocalStorage.getItem('agenda');
  return Array.isArray(stored) ? (stored as AgendaItem[]) : [];
}

const items = ref<AgendaItem[]>(loadItems());
const kind = ref<AgendaType>('wedstrijd');
const title = ref('');

watch(
  items,
  (value) => {
    LocalStorage.set('agenda', value);
  },
  {
    deep: true
  },
);

function addItem() {
  const label = title.value.trim();

  if (label === '') {
    return;
  }

  items.value.push({
    id: crypto.randomUUID(),
    type: kind.value,
    title: label,
  });

  title.value = '';
}

function removeItem(id: string) {
  items.value = items.value.filter((item) => item.id !== id);
}
</script>
