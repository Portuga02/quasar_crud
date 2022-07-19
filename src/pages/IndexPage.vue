
<template>
  <q-page padding>
    <q-table
     title="Lista de Usuários salvos"
     :rows="posts"
     :columns="columns"
     row-key="name" >

     <template v-slot:body-cell-actions="props">
        <q-td :props="props">
         <q-btn
         style="background: #FF0080; color: white; text-align:justify-center;"
         size ="sm" label="deletar"  icon="delete" @click="handleDeletePost(props.row.id)"/>
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
        name: 'years',
        field: 'years',
        label: 'Idade',
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
