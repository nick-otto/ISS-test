<template>
  <div class="todo">
    <UIButton variant="normal" @click="openCloseModal">
      Добавить новое
    </UIButton>

    <div v-if="allList.length > 0">
      <UIButton variant="to-do" @click="getCheckedList(allList)">
        Сделанные
      </UIButton>

      <UIButton variant="not-to-do" @click="getUnCheckedList(allList)">
        Не сделанные
      </UIButton>

      <UIButton variant="normal" @click="getAllList()">
        Все дела
      </UIButton>

      <UIButton variant="normal" @click="deleteCheckedList(allList)">
        Удалить выбранные(сделанные)
      </UIButton>
    </div>

    <div
        v-for="item in allList"
        :key="item.id"
    >
      <div class="todo__block__wrapper">
        <div @click="changeBlock(item.name, item.description, item.id)" class="todo__block">
          <div class="todo__block__name">
            {{ item.name }}
          </div>

          <div class="todo__block__description">
            {{ item.description }}
          </div>
        </div>

        <input :id="`checkbox-${item.id}`" type="checkbox" :checked="item.status" @click="checkInput(item)">

      </div>
    </div>

    <UIModal
        :showModal="showModal"
        @closeModal="openCloseModal"
    >
      <template #body>
        Название
        <UIInput
            v-model="nameItem"
        />
        Описание
        <UIInput
            v-model="descriptionItem"
        />
        <UIButton
            variant="save"
            @click="saveBlock(nameItem, descriptionItem)"
        >
          Сохранить
        </UIButton>
        <UIButton
            variant="delete"
            @click="deleteBlock(idItem)"
        >
          Удалить
        </UIButton>
      </template>
    </UIModal>
  </div>


</template>

<script setup>
const showModal = ref(false)
const nameItem = ref('')
const descriptionItem = ref('')
const idItem = ref('')
const allList = ref([])
const counter = ref(0)

const openCloseModal = () => {
  showModal.value = !showModal.value
  descriptionItem.value = ''
  nameItem.value = ''
  idItem.value = ''
}

const changeBlock = (name, description, id) => {
  openCloseModal()
  descriptionItem.value = description
  nameItem.value = name;
  idItem.value = id
}

const saveBlock = (nameItem, descriptionItem) => {
  if (!idItem.value) {
    localStorage.setItem(counter.value, JSON.stringify({
      id: counter.value,
      name: nameItem,
      description: descriptionItem,
      status: false
    }))
    counter.value++
    sessionStorage.setItem('counter', counter.value)
  } else {
    localStorage.setItem(idItem.value, JSON.stringify({
      id: idItem.value,
      name: nameItem,
      description: descriptionItem
    }))
  }
  getAllList()
  openCloseModal()
}

const deleteBlock = (key) => {
  localStorage.removeItem(key)
  getAllList()
  openCloseModal()
}
const getAllList = () => {
  const list = []
  const keys = Object.keys(localStorage)
  keys.forEach(key => {
    list.push(JSON.parse(localStorage.getItem(key)))
  })
  allList.value = list
}

const getCheckedList = (list) => {
  const filterList = []
  list.forEach(block => {
    if (block.status) {
      filterList.push(block)
    }
  })
  allList.value = filterList
}

const getUnCheckedList = (list) => {
  const filterList = []
  list.forEach(block => {
    if (!block.status) {
      filterList.push(block)
    }
  })
  allList.value = filterList
}

const deleteCheckedList = (list) => {
  list.forEach(item => {
    if (item.status) {
      localStorage.removeItem(item.id)
    }
  })
  getAllList()
}

const checkInput = (item) => {
  localStorage.setItem(item.id, JSON.stringify({
    id: item.id,
    name: item.name,
    description: item.description,
    status: document.getElementById(`checkbox-${item.id}`).checked
  }))
  getAllList()
}

onMounted(() => {
  getAllList()
  counter.value = sessionStorage.getItem('counter') || 0
})
</script>

<style lang="scss" scoped>
.todo {
  display: flex;
  margin: 50px auto;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  .todo__block__wrapper {
    display: flex;
    border: 1px solid black;
    padding: 10px;
    border-radius: 5px;
    margin: 10px;

    .todo__block {
      display: flex;
      max-width: 200px;
      min-width: 100px;
      justify-content: center;
      cursor: pointer;


      .todo__block__name {
        margin-right: 5px;
      }
    }
  }
}
</style>