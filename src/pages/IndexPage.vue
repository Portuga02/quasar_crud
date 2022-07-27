
<template>
  <q-page padding>
    <q-table
     title="Lista de usuários"
     :rows="posts"
     :columns="columns"
     row-key="name" >
  <template v-slot:top>
  <span> Novos Usuários </span>
  <q-space/>
   <q-btn
         style="background: #FF0080; color: white; text-align:center;"
         size ="sm" label="Novo Usuário"  :to="{ name: 'formPost'}">
    </q-btn>
  </template>
     <template v-slot:body-cell-actions="props">
        <q-td :props="props">
         <q-btn
         style="background: #FF0080; color: white; text-align:justify-center;"
         size ="sm" label="Deletar"  icon="delete" @click="handleDeletePost(props.row.id)"/>
        </q-td>
      </template>
    </q-table>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import postsService from 'src/services/posts'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'IndexPage',
  setup () {
    const posts = ref([])
    const { list, remove } = postsService()
    const columns = [
      { name: 'id', field: 'id', label: 'ID', sortable: true, align: 'left' },
      {
        name: 'title',
        field: 'title',
        label: 'Titulo',
        sortable: true
      },
      {
        name: 'author',
        field: 'author',
        label: 'Autor',
        sortable: true,
        align: 'left'
      },
      {
        name: 'content',
        field: 'content',
        label: 'Conteudo sobre',
        sortable: true,
        align: 'left'
      },
      {
        name: 'years',
        field: 'years',
        label: 'Idade',
        sortable: true,
        align: 'left'
      },
      {
        name: 'address',
        field: 'address',
        label: 'Idade',
        sortable: true,
        align: 'left'
      },
      {
        name: 'country',
        field: 'country',
        label: 'País',
        sortable: true,
        align: 'left'
      },
      {
        name: 'fone',
        field: 'fone',
        label: 'Telefone',
        sortable: true,
        align: 'left'
      },
      {
        name: 'email',
        field: 'email',
        label: 'email',
        sortable: true,
        align: 'left'
      },
      {
        name: 'github',
        field: 'github',
        label: 'Git Hub ',
        sortable: true,
        align: 'left'
      },
      {
        name: 'actions',
        field: 'Actions',
        label: 'Ações',
        align: 'right'
      }
    ]
    const $q = useQuasar()
    onMounted(() => {
      getPost()
    })
    const getPost = async () => {
      try {
        // eslint-disable-next-line semi
        const data = await list();
        posts.value = data
      } catch (error) {}
    }

    const handleDeletePost = async (id) => {
      try {
        $q.dialog({
          title: 'Deletar',
          message: 'Deseja realmente apagar este registro',
          cancel: true,
          persistent: true
        }).onOk(async () => {
          await remove(id)
          $q.notify({ message: 'Registro apagado com sucesso' })
          await getPost()
        })
      } catch (error) {
        $q.notify({ message: 'Erro ao Apagar', icon: 'times', color: 'negative' })
      }
    }
    return {
      posts,
      columns,
      handleDeletePost
    }
  }
})
</script>
