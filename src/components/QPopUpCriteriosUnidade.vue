<template>
  <q-dialog :modelValue="true" persistent>
    <q-card style="width: 100%">
      <q-linear-progress v-if="isLoading" indeterminate color="primary" />

      <q-list v-bind:key="index" v-for="(item, index) in criterios">
        <q-card flat bordered class="q-ma-md">
          <q-card-section class="row">
            <div>
              <div class="text-bold">{{ item.nome }}</div>
              <div class="text-grey-8">{{ item.grupo }}</div>
            </div>
            <q-space />
            <q-btn flat :disable="item.respondido" :color="item.respondido ? 'green' : 'primary'"
              :label="item.respondido ? 'Respondido' : 'Responder'" :icon="item.respondido ? 'done' : undefined"
              @click="irParaCriterio(item.numero)" />
          </q-card-section>
        </q-card>
      </q-list>
      <q-separator />

      <q-card-actions align="right">
        <q-btn flat color="primary" label="Voltar" @click="fecharpopup()" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { defineComponent } from 'vue';
import { api } from 'boot/axios';

export default defineComponent({
  name: 'CriteriosPage',
  props: {
    unidade: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      criterios: [],
      isLoading: true,
      dimension: 'Gestão e Projeto',
    };
  },
  mounted() {
    this.handleDimensionChange();
  },
  methods: {
    async handleDimensionChange() {
      const { data } = await api.get('/dimensoes');
      this.criterios = data;
      this.isLoading = false;
    },
    irParaCriterio(numero) {
      const url = `/criterio/${numero}`;
      this.$router.push(url);
    },
    fecharpopup() {
      this.$emit('fecharpopup', false);
    },
  },
});
</script>
