<template>
  <q-page padding>
    <div>
      {{ "Novo cadastro" }}
    </div>
    <br />
    <q-form @submit="onSubmit" @reset="onReset" class="row q-col-gutter-sm">
      <q-input
        outlined
        v-model="form.title"
        label="Titulo"
        lazy-rules
        class="col-lg-6 col-xs-12"
             />
      <q-input
        outlined
        v-model="form.author"
        label="Autor"
        lazy-rules
        class="col-lg-6 col-xs-12"
            />

      <q-input
        outlined
        v-model="form.years"
        label="Idade"
        lazy-rules
        class="col-lg-6 col-xs-12"
            />
      <q-input
        outlined
        v-model="form.address"
        label="Endereço"
        lazy-rules
        class="col-lg-6 col-xs-12"

      />
      <q-input
        outlined
        v-model="form.country"
        label="País"
        lazy-rules
        class="col-lg-6 col-xs-12"
           />
      <q-input
        outlined
        v-model="form.fone"
        label="Telefone"
        lazy-rules
        class="col-lg-6 col-xs-12"

      />
      <q-input
        outlined
        v-model="form.email"
        label="Email"
        lazy-rules
        class="col-lg-6 col-xs-12"
            />
      <q-input
        outlined
        v-model="form.github"
        label="Git hub"
        lazy-rules
        class="col-lg-6 col-xs-12"

      />
      <div class="col-lg-6 col-xs-12">
        <q-editor
          outlined
          v-model="form.content"
          label="Conteudo"
          lazy-rules
          class="col-lg-6 col-xs-12"
          min-height="5rem"

        />
      </div>

      <div class="col-12 q-gutter-sm">
        <q-btn label="Salvar" color="primary" class="float-right" icon="save" type="submit" />
        <q-btn label="Cancelar" color="secondary" class="float-right" icon="cancel" :to="{ name: 'home'}" />
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'
import postsService from 'src/services/posts'
import { useQuasar } from 'quasar'
import { useRouter } from 'vue-router'
export default defineComponent({
  name: 'FormPost',
  setup () {
    const { post } = postsService()
    const $q = useQuasar()
    const route = useRouter()
    const form = ref({
      title: '',
      content: '',
      author: '',
      years: '',
      address: '',
      country: '',
      fone: '',
      email: '',
      github: ''
    })
    const onSubmit = async () => {
      try {
        await post(form.value)
        $q.notify({ message: 'Registro salvo com sucesso', icon: 'check', color: 'positive' })
        route.push({ name: 'home' })
      } catch (error) {
        console.error(error)
      }
    }
    return {
      form,
      onSubmit
    }
  }
})
</script>

<style></style>
