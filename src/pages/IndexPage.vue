<template>
  <q-page padding>
    <q-table
    title='crud'
    :rows='posts'
    :columns='columns'
    row-key='name' />
  </q-page>
</template>


<script>
import { defineComponent, ref, onMounted } from 'vue'
import { api } from 'boot/axios'

export default defineComponent({
  name: 'IndexPage',
  setup () {
    const posts = ref([])
    const columns = [
      { name: 'id', field: 'id', label: 'ID', sortable: true, align: 'left' },
      {
        name: 'title',
        field: 'title',
        label: 'Titulo',
        sortable: true,
        align: 'left'
      },
      {
        name: 'author',
        field: 'author',
        label: 'Autor',
        sortable: true,
        align: 'left'
      }
    ]
    onMounted(() => {
      getPost()
    })
    const getPost = async () => {
      try {
        const response = api.get('http://localhost:3000/posts')
        console.log(response)
      } catch (error) {}
    }
    return {
      posts,
      columns
    }
  }
})
</script>
