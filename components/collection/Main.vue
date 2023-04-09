<template>
  <div>
    <div>
      <CollectionListProject
        :key="listRender"
        :projectInfo="projectInfo"
        @showAdd="showAdd"
        @showEdit="showEdit"
        @deleteProject="deleteProject"
      />
    </div>
    <div v-if="show">
      <CollectionAddProject @addProject="addProject" :key="renderAdd" />
    </div>
    <div v-if="showEditModal">
      <CollectionEditProject
        @editProject="editProject"
        :updateData="updateData"
        :key="renderEdit"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const show = ref(false)
const renderAdd = ref(0)
const renderEdit = ref(0)
const showEditModal = ref(false)
const listRender = ref(0)
const updateData = ref({})

const projectsData = useAuthLazyFetch(
  `https://v7-stark-db-orm.mercury.infinity-api.net/api/applications/?offset=0&limit=100`,
)
const showAdd = (data: Boolean) => {
  show.value = data
  renderAdd.value++
}
let editIndex = ref('')
const showEdit = (data, index) => {
  showEditModal.value = data
  editIndex.value = index
  renderEdit.value++
  updateData.value = data
}
let projectInfo = ref(projectsData.data._rawValue)
const addProject = (body: Object) => {
  const postOptions = {
    body: body,
  }
  const addData = useAuthLazyFetchPost(
    `https://v7-stark-db-orm.mercury.infinity-api.net/api/applications/`,
    postOptions,
  )
  projectInfo.value.unshift(body)
}
const editProject = (body: Object) => {
  const putOptions = {
    body: body,
  }
  const editData = useAuthLazyFetchPut(
    `https://v1-orm-gharpe.mercury.infinity-api.net/api/projects/${body.uid}`,
    putOptions,
  )
  listRender.value++
  // projectInfo.value.$set(editIndex.value,body)
}
const deleteProject = (data: object, index) => {
  const deleteProjectData = useAuthLazyFetchDelete(
    `https://v1-orm-gharpe.mercury.infinity-api.net/api/projects/${data.uid}`,
  )
  projectInfo.value.splice(index, 1)
}
</script>
