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
                <template v-if="item.date">
                  · {{ formatDate(item.date) }}
                </template>
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

        <q-input
          v-model="when"
          class="q-mt-md"
          label="Datum"
          type="date"
          outlined
        />

        <q-btn
          class="q-mt-md"
          color="primary"
          label="Toevoegen"
          :disable="title.trim() === '' || when === ''"
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
  date: string;
};

const kinds = [
  { label: 'Wedstrijd', value: 'wedstrijd' },
  { label: 'Training', value: 'training' },
];

function loadItems(): AgendaItem[] {
  const stored = LocalStorage.getItem('agenda');
  return Array.isArray(stored) ? (stored as AgendaItem[]) : [];
}

function formatDate(value: string) {
  return new Intl.DateTimeFormat('nl-BE', {
    weekday: 'short',
    day: 'numeric',
    month: 'short',
  }).format(new Date(value));
}

const items = ref<AgendaItem[]>(loadItems());
const kind = ref<AgendaType>('wedstrijd');
const title = ref('');
const when = ref('');

watch(
  items,
  (value) => {
    LocalStorage.set('agenda', value);
  },
  { deep: true },
);

function addItem() {
  const label = title.value.trim();

  if (label === '' || when.value === '') {
    return;
  }

  items.value.push({
    id: crypto.randomUUID(),
    type: kind.value,
    title: label,
    date: when.value,
  });

  title.value = '';
  when.value = '';
}

function removeItem(id: string) {
  items.value = items.value.filter((item) => item.id !== id);
}
</script>
