<template>
  <q-layout>
    <q-card class="q-ma-lg">
      <q-linear-progress v-if="isLoading" indeterminate />

      <q-card-section>
        <div class="text-h6">Unidades</div>
      </q-card-section>
      <q-separator />

      <q-card-section>
        <div v-if="possuiUnidades">
          <q-list v-for="unidade in unidades" :key="unidade.nome">
            <q-item>
              <q-item-section class="q-gutter-y-md">
                <div>
                  <q-item-label>{{ unidade.nome }}</q-item-label>
                  <q-item-label caption>Tel.: {{ unidade.telefone }}</q-item-label>
                </div>
                <div>
                  <q-btn unelevated color="primary" label="Iniciar avaliação" @click="irParaAmbiente(unidade._id)" />
                </div>
              </q-item-section>
            </q-item>
          </q-list>
        </div>

        <span v-else>Não há unidades cadastradas.</span>
      </q-card-section>
      <q-separator />

      <q-card-actions align="right">
        <q-btn flat color="primary" label="Atualizar" @click="obterUnidades()" />
        <q-btn flat color="primary" label="Adicionar" to="/unidade" />
      </q-card-actions>
    </q-card>
  </q-layout>
</template>

<script>
import { api } from '../boot/axios';

export default {
  data() {
    return {
      isLoading: true,
      unidades: [],
    };
  },
  mounted() {
    this.obterUnidades();
  },
  methods: {
    irParaAmbiente(id) {
      localStorage.setItem('apo@unidade_id', id);
      this.$router.push('/ambiente');
    },
    async excluirUnidade(id) {
      const edificacao = localStorage.getItem('apo@usuario_edificacao');
      const endpoint = `/unidades/${edificacao}/${id}`;

      await api.delete(endpoint);
      this.obterUnidades();
    },
    async obterUnidades() {
      this.isLoading = true;
      const id = localStorage.getItem('apo@usuario_id');
      const edificacao = localStorage.getItem('apo@usuario_edificacao');

      const endpoint = `/unidades/${edificacao}/proprietario/${id}`;
      const { data } = await api.get(endpoint);

      this.unidades = data;
      this.isLoading = false;
    },
  },
  computed: {
    possuiUnidades() {
      return this.unidades.length > 0;
    },
  },
};
</script>
